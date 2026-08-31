# 03 — Modelo semántico

Datos obtenidos del MCP `powerbi-modeling` (conexión `ConnectFolder` al directorio
`OTC - Omia Cost Control Tower.SemanticModel/definition`).

## Resumen

| Propiedad | Valor |
|---|---|
| Nombre | `Model` |
| Cultura | `es-CO` |
| Modo | `Import` (defaultMode Import) |
| Compatibilidad | 1606 |
| Time Intelligence | deshabilitada (`__PBI_TimeIntelligenceEnabled=0`) |
| Tablas | 11 |
| Medidas | 48 (todas en `_Medidas`) |
| Relaciones | 18 (todas activas) |

## Tablas

### Dimensiones (5)

| Tabla | Columnas | Clave(s) | Notas |
|---|---|---|---|
| `Dim_proyecto` | 3 | `Id proyecto` (int), `Código de proyecto` (texto) | `Nombre de proyecto` |
| `Dim_categoria` | 11 | `Id categoria` (int) | + jerarquía `Tipo costo Jerarquía` (Tipo costo → Desc_N2 → Desc_N3_OK) |
| `Dim_subseco` | 5 | `Id subseco` (int) | `Subseco`, `Descripción`, `Distribución Costos`, `Id categoria` |
| `Dim_Subseco stanby` | 5 | — | **oculta**; `DATATABLE` con `Equivalencia` y `Tipo de Costo` |
| `Calendario` | 19 | `Date` | calculada; **no marcada como tabla de fechas** |

### Hechos (5)

| Tabla | Columnas | Granularidad / origen |
|---|---|---|
| `Fact_solicitud_de_pedido` | 30 | SOLPED abiertas (filtro `Cancelado="N"` y `Status="O"`) |
| `Fact_facturas_conformadas` | 40 | facturas conformadas |
| `Fact_ordenes_de_compra` | 64 | OC (filtro `Cancelado="N"`) |
| `Fact_presupuesto` | 4 | `COP`, `Incidencia`, `Id categoria`, `Id proyecto` |
| `Fact_detalle_documental` | 17 | unión de los 3 ETLs (`Tipo de documento`) |

### Medidas (1)

| Tabla | Medidas |
|---|---|
| `_Medidas` | 48 |

## Relaciones (18 — todas activas, ninguna inactiva)

| De (many) | A (one) | Cardinalidad | Cross-filter |
|---|---|---|---|
| `Fact_solicitud_de_pedido`.`Código de proyecto` | `Dim_proyecto`.`Código de proyecto` | *:1 | 1 dirección |
| `Fact_facturas_conformadas`.`Proyecto` | `Dim_proyecto`.`Código de proyecto` | *:1 | 1 dirección |
| `Fact_ordenes_de_compra`.`Código Proyecto` | `Dim_proyecto`.`Código de proyecto` | *:1 | 1 dirección |
| `Fact_presupuesto`.`Id proyecto` | `Dim_proyecto`.`Id proyecto` | *:1 | 1 dirección |
| `Fact_detalle_documental`.`Código de proyecto` | `Dim_proyecto`.`Código de proyecto` | *:1 | 1 dirección |
| `Fact_solicitud_de_pedido`.`Id subseco` | `Dim_subseco`.`Id subseco` | *:1 | 1 dirección |
| `Fact_facturas_conformadas`.`Id subseco` | `Dim_subseco`.`Id subseco` | *:1 | 1 dirección |
| `Fact_ordenes_de_compra`.`Id subseco` | `Dim_subseco`.`Id subseco` | *:1 | 1 dirección |
| `Fact_detalle_documental`.`Id subseco` | `Dim_subseco`.`Id subseco` | *:1 | 1 dirección |
| `Fact_solicitud_de_pedido`.`Id categoria` | `Dim_categoria`.`Id categoria` | *:1 | 1 dirección |
| `Fact_facturas_conformadas`.`Id categoria` | `Dim_categoria`.`Id categoria` | *:1 | 1 dirección |
| `Fact_ordenes_de_compra`.`Id categoria` | `Dim_categoria`.`Id categoria` | *:1 | 1 dirección |
| `Fact_presupuesto`.`Id categoria` | `Dim_categoria`.`Id categoria` | *:1 | 1 dirección |
| `Fact_detalle_documental`.`Id categoria` | `Dim_categoria`.`Id categoria` | *:1 | 1 dirección |
| `Fact_solicitud_de_pedido`.`Fecha de creación` | `Calendario`.`Date` | *:1 | 1 dirección |
| `Fact_facturas_conformadas`.`Fecha de contabilización` | `Calendario`.`Date` | *:1 | 1 dirección |
| `Fact_ordenes_de_compra`.`Fecha de creación` | `Calendario`.`Date` | *:1 | 1 dirección |
| `Fact_detalle_documental`.`Fecha de creación/Contabilización NC` | `Calendario`.`Date` | *:1 | 1 dirección |

> Las relaciones de `Fact_detalle_documental` y las de `Id categoria` llevan
> nombre `AutoDetected_*`. La relación de presupuesto usa `Id proyecto` (int)
> mientras el resto usa `Código de proyecto` (texto).

## Medidas DAX (48, en `_Medidas`)

### Medidas principales

| Medida | Expresión |
|---|---|
| `Costo ejecutado` | `SUM(Fact_ordenes_de_compra[Total Líneas OC $Cop]) + SUM(Fact_facturas_conformadas[Total líneas])` |
| `Costo comprometido` | `SUM(Fact_solicitud_de_pedido[Total líneas])` |
| `Costo entrante` | *(vacía — código comentado)* |
| `Presupuesto total` | `SUM(Fact_presupuesto[COP])` |
| `Presupuesto remanente` | `[Presupuesto total] - [Costo ejecutado] - [Costo comprometido]` |

### Medidas secundarias

| Medida | Expresión (resumida) |
|---|---|
| `% Ejecución` | `DIVIDE([Costo ejecutado] + [Costo comprometido], [Presupuesto total])` |
| `Presupuesto total Visible` | `IF([Presupuesto total] = 0, BLANK(), [Presupuesto total])` |
| `Costo comprometido visible` | `SUM(Fact_solicitud_de_pedido[Total líneas])` |
| `Costo ejecutado visible` | `IF(SUM(OC[Total Líneas OC $Cop]) + SUM(Facturas[Total líneas]) = 0, BLANK(), ...)` |
| `Presupuesto remanente visible > 0` | `pres - ejec_dentro - comp_dentro` (clamp con `MIN`/`MAX`) |
| `Consumo total visible` | `[Costo ejecutado visible] + [Costo comprometido visible]` |
| `Sobrepresupuesto visible (>0)` | `cons - pres` si `> 0` |
| `Presupuesto remanente visible` | `[Presupuesto total Visible] - ejec - comp` |
| `% Ejecución visible` | `DIVIDE([Consumo total visible], [Presupuesto total Visible])` |
| `% Consumo presupuesto` | `DIVIDE(cons, pres)` (con guardas) |
| `Consumo % visible` | `MIN([% Consumo presupuesto], 1)` |
| `Restante % visible` | `MAX(1 - [% Consumo presupuesto], 0)` |
| `Exceso % visible` | `MAX([% Consumo presupuesto] - 1, 0)` |
| `% Consumo presupuesto texto` | `FORMAT([% Consumo presupuesto], "0.0%")` |

### Desglose presupuesto

| Medida | Expresión (resumida) |
|---|---|
| `Ejecutado dentro presupuesto` | `MIN(ejec, pres)` si `> 0` |
| `Ejecutado excedente` | `MAX(ejec - pres, 0)` si `> 0` |
| `Comprometido dentro presupuesto` | `MIN(comp, pres - ejec_dentro)` si `> 0` |
| `Comprometido excedente` | `MAX(comp - saldo_pres, 0)` si `> 0` |
| `Dentro presupuesto visual` | `[Ejecutado dentro presupuesto] + [Comprometido dentro presupuesto]` |
| `Excedente visual` | `[Ejecutado excedente] + [Comprometido excedente]` |

### Tarjetas (medidas HTML y auxiliares)

| Grupo | Medidas |
|---|---|
| `Medidas tarjeta presupuesto total` | `Card HTML - Presupuesto Total`, `% Incidencia` |
| `Medidas tarjeta presupuesto remanente` | `Sobrecosto visible`, `% Sobrecosto`, `% Sobrecosto texto`, `Presupuesto remanente visible tarjeta`, `Card HTML - Presupuesto disponible` |
| `Medidas tarjeta consumo total` | `Cantidad ODC cerradas`, `Cantidad NFC`, `Cantidad SOLPED abiertas`, `Card HTML - Consumo Total` |

> Las medidas `Card HTML - *` generan cadenas HTML (tarjetas renderizadas como
> texto enriquecido). Por ejemplo `Card HTML - Consumo Total` calcula consumo
> vs. presupuesto y produce una tarjeta con barra de progreso verde/amarillo/rojo.

### Detalle documental

| Medida | Expresión |
|---|---|
| `Total documentos documentales` | `DISTINCTCOUNT(Fact_detalle_documental[Número de documento])` |
| `Total factura conformada` | `CALCULATE(SUM(Fact_detalle_documental[Total línea]), [Tipo de documento] = "Factura conformada")` |
| `Total orden de compra cerrada` | `CALCULATE(SUM(...), [Tipo de documento] = "Orden de compra cerrada")` |
| `Total solicitud de pedido abierta` | `CALCULATE(SUM(...), [Tipo de documento] = "Solicitud de pedido abierta")` |
| `Cantidad de facturas conformadas` | `CALCULATE(DISTINCTCOUNT([Número de documento]), ... = "Factura conformada")` |
| `Cantidad de orden de compra cerrada` | `CALCULATE(DISTINCTCOUNT([Número de documento]), ... = "Orden de compra cerrada")` |
| `Cantidad de lineas orden de compra cerrada` | `CALCULATE(COUNTROWS(...), ... = "Orden de compra cerrada")` |
| `Cantidad de solicitud de pedido abierta` | `CALCULATE(DISTINCTCOUNT([Número de documento]), ... = "Solicitud de pedido abierta")` |
| `Cantidad de lineas solicitud de pedido abierta` | `CALCULATE(COUNTROWS(...), ... = "Solicitud de pedido abierta")` |
| `Card HTML - Factura Conformada` / `Card HTML - ODC Cerradas` / `Card HTML - SOLPED Abiertas` | tarjetas HTML por tipo de documento |
