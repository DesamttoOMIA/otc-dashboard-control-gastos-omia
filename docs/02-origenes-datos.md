# 02 — Orígenes de datos

Todos los orígenes externos se leen vía `Web.Contents` desde la misma biblioteca
de SharePoint:

```
https://omiacolombia.sharepoint.com/sites/sgdingenieriamantenimiento/
  Documentos compartidos/2.Documentos Compartidos/Proyecto control de gastos/
```

## Orígenes externos (SharePoint)

| Archivo | Tipo | Hoja / detalle | Consumido por |
|---|---|---|---|
| `N.1 ABA Solped status.txt` | CSV tab-separado (30 cols, UTF-16, sin comillas) | — | `Fact_solicitud_de_pedido` |
| `N.10.2 Facturas Conformadas.txt` | CSV tab-separado (39 cols, UTF-16) | — | `Fact_facturas_conformadas` |
| `N.4.4 ABA OC_Solped_DirProv_ClasArt 1.txt` | CSV tab-separado (62 cols, UTF-16) | — | `Fact_ordenes_de_compra` |
| `Copia de REE Continental Mtto Electrico 2025 REV0_0511_12 Meses.xlsx` | Excel | `Resumen COMPL` (layout REE con varias filas de encabezado) | `Fact_presupuesto` |
| `Lista de Proyectos (1).xlsx` | Excel | `Sheet1` | `Dim_proyecto` |
| `Dimenciones.xlsx` | Excel | `Dim_categoria` y `Dim_subseco` | `Dim_categoria`, `Dim_subseco`, `Dim_categoria (1)` |

## Tablas calculadas (sin origen externo)

| Tabla | Cálculo |
|---|---|
| `Calendario` | DAX `ADDCOLUMNS(CALENDAR(...))` entre `MIN(Fecha de creación SOLPED)` y `MAX(Fecha de creación OC)` |
| `Dim_Subseco stanby` | DAX `DATATABLE` (tabla oculta de clasificación por subseco) |
| `_Medidas` | Partición dummy embebida (solo contenedor de medidas) |

## Tablas derivadas (M / ETL dentro del modelo)

| Objeto | Origen | Lógica |
|---|---|---|
| `ETL_solicitud_de_pedido` | expresión → `Fact_solicitud_de_pedido` | limpieza/rename + `Tipo de documento = "Solicitud de pedido abierta"` |
| `ETL_facturas_conformadas` | expresión → `Fact_facturas_conformadas` | limpieza/rename + `Tipo de documento = "Factura conformada"` |
| `ETL_ordenes_de_compra` | expresión → `Fact_ordenes_de_compra` | limpieza/rename + `Tipo de documento = "Orden de compra cerrada"` |
| `Fact_detalle_documental` | `Table.Combine` de los 3 ETLs | unión vertical de los tres tipos de documento |
| `Dim_categoria (1)` | expresión → `Dimenciones.xlsx` | hoja `Dim_categoria`, helper para resolver `Id categoria` en el ETL de presupuesto |

## Notas

- Los hechos `Fact_*` realizan **joins en M** contra `Dim_subseco` / `Dim_categoria`
  para resolver `Id subseco` / `Id categoria` durante la carga.
- Los nombres de archivo son **versionados** (`REV0_0511_12 Meses`, `(1)`, etc.):
  un cambio en SharePoint rompe el modelo hasta actualizar la URL.
