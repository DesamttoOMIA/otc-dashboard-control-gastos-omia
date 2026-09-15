# 09 — Mapa de la Página 1 (Control de presupuesto)

Mapa funcional de la pestaña `Página 1` (page id `3669ff3a15d8738aebfb`) del
reporte **OTC - Omia Cost Control Tower**. Es la pestaña final que se va a
intervenir; este documento sirve de referencia para ubicar visuales y medidas
cuando se ordenen cambios.

> Estado capturado: tras el merge de `development` (commit `fc027ad`) y la
> reparación de Resumen Gerencial (`d4c0bfb`). Toda medida referenciada
> resuelve en el modelo.

## 1. Página

| Propiedad | Valor |
|---|---|
| `name` | `3669ff3a15d8738aebfb` |
| `displayName` | `Página 1` (pendiente de renombrar) |
| `displayOption` | `FitToPage` |
| Canvas | 1450 × 1300 px |
| Fondo | Blanco, sin wallpaper ni imagen |
| Filtro de página | `Dim_proyecto[Nombre de proyecto]` (sin valores fijos) |
| Slicers | Ninguno en la página |

## 2. Visuales

### 2.1 Encabezado (chrome)

| ID | Tipo | Posición | Notas |
|---|---|---|---|
| `4464e3f8` | shape | (11,8) 52×785 | barra lateral azul, decorativa |
| `ea0fb0ad` | image | (75,8) 153×58 | logo OMIA (variante `-Photoroom`) |
| `20244db5` | textbox | (228,8) 672×58 | título, altura justa 58 px |
| `133c428a` | image | (19,66) 36×48 | icono menú, decorativo sobre la barra |
| `e494d344` | image | (22,114) 29×37 | icono calendario, decorativo sobre la barra |

### 2.2 Fila de KPIs (cards)

| ID | Posición | Medida | Rol |
|---|---|---|---|
| `e172d774` | (75,66) 247×119 | `Presupuesto Anual` | presupuesto COP × 12 |
| `40c882a3` | (326,66) 175×102 | `SOLPED en gestión` | costo comprometido SOLPED |
| `9d8210f5` | (504,66) 175×102 | `Comprometido` | OC no canceladas (displayName "Orden de Compra" en pivot) |
| `6ece7d5b` | (682,66) 175×102 | `Recibido (Entrada Mercancia)` | EM con OC |
| `be1aa25f` | (859,66) 175×102 | `Ejecutado Conformada` | EM sin OC (compra directa) |
| `dcf8b042` | (1037,66) 168×102 | `Facturado Total` | facturas SAP + conformada |
| `2a6d1f78` | (1208,66) 175×102 | `Pagado Total` | facturas con pago |
| `25bb79f3` | (75,185) 247×111 | `Disponible Anual` | presupuesto − consumo |

### 2.3 Gráficos

| ID | Tipo | Posición | Título | Roles |
|---|---|---|---|---|
| `1c73f215` | lineStackedColumnComboChart | (75,296) 890×497 | Consumo presupuesto por categoría REE | Category: jerarquía `Tipo costo → Desc_N2 → Desc_N3_OK`; Y: `Consumo dentro presupuesto`, `Disponible dentro presupuesto`, `Exceso presupuesto` (disp. "Sobrepresupuesto"); Y2 (línea): `Presupuesto Anual` |
| `3701b50d` | barChart | (978,296) 460×497 | Consumo por subceco [sic] | X: `Dim_subseco.Descripción`; Y: `Consumo Total` |
| `f692668c` | pivotTable | (75,818) 1343×467 | — | Rows: jerarquía `Tipo costo → Desc_N2 → Desc_N3_OK`; Values: `Presupuesto Anual`, `Consumo Total`, `Disponible Anual`, `Comprometido` ("Orden de Compra"), `Ejecutado Conformada` ("Factura Conformada") |

## 3. Contexto semántico

- **Tubería de gasto** (modelo entrante de development):
  SOLPED → `Comprometido` (OC no canceladas) → `Recibido` (EM con OC) /
  `Ejecutado Conformada` (EM directo) → `Facturado Total` (FPE + conformada) →
  `Pagado Total`.
- **Presupuesto**: `Presupuesto Anual` = `SUM(Fact_presupuesto_final[COP]) × 12`
  por proyecto; filtrable por categoría vía `Fact_presupuesto_final` →
  `Dim_categoria` y `Dim_proyecto`.
- **Relaciones clave**: no hay relación `Dim_subseco → Dim_categoria`
  (herencia por `SELECTEDVALUE`/`LOOKUPVALUE`, igual que en Resumen);
  `Fact_factura_proveedor_pago_efectuado` solo se relaciona con `Dim_proyecto`
  y `Calendario` (no tiene vínculo a subseco ni categoría).
- **Medidas involucradas** (todas en `_Medidas.tmdl`): `Presupuesto Anual`,
  `SOLPED en gestión`, `Comprometido`, `Recibido (Entrada Mercancia)`,
  `Ejecutado Conformada`, `Facturado Total`, `Pagado Total`, `Disponible Anual`,
  `Consumo Total`, `Consumo dentro presupuesto`, `Disponible dentro presupuesto`,
  `Exceso presupuesto`.

## 4. Pendientes conocidos en esta pestaña

- `displayName` sigue siendo `Página 1` (renombrar a algo descriptivo, p. ej.
  "Control de presupuesto").
- Typo en título de `3701b50d`: "Consumo por subceco" → "subseco".
- La barra lateral e iconos (menú/calendario) son decorativos; el logo usa la
  variante `-Photoroom` (inconsistente con Resumen).
- Estilos del tapiz aún no alineados con Resumen Gerencial (fondo blanco vs.
  `#F4F6F9`, títulos 24 pt vs. jerarquía de marca).