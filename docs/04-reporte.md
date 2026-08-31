# 04 — Reporte

Proyecto PBIR. Cada página vive en `OTC - Omia Cost Control Tower.Report/definition/pages/<id>/`
con un `page.json` (metadatos de la página) y un `visual.json` por visual.

Todas las páginas de detalle tienen un **filtro fijo de proyecto** a
`2170 CONTINENTAL-MTO ELECTRICO MINA BURITICÁ` sobre `Dim_proyecto[Nombre de proyecto]`.

## Páginas (7)

| Página | id | Tamaño / tipo |
|---|---|---|
| Página 1 | `3669ff3a15d8738aebfb` | 1450×1300, ActualSize |
| Página 2 | `41a3c02a448803210081` | 1280×720, FitToPage |
| Detalle documental | `66c72b3b634480967536` | 1450×1300, ActualSize |
| Tool tip G1 | `77c673e71ca0970dd6e8` | 320×270, Tooltip |
| Solicitud de Pedido | `90bac25de67fb4f25efe` | 1280×720, FitToPage |
| Facturas Comformadas | `917148e003ff3ed2388e` | 1280×720, FitToPage |
| Ordenes de Compra | `f494cbf10ecb722c0ab4` | 1280×720, FitToPage |

---

### Página 1 — dashboard principal

Visuales: 13 (tarjetas HTML KPI, gráficos, tabla pivot e imágenes/logo).

| Tipo | Título / contenido | Campos / medidas |
|---|---|---|
| htmlContent | `Card HTML - Presupuesto disponible` | `_Medidas[Card HTML - Presupuesto disponible]` |
| tableEx | matriz | `Dim_categoria`, `Dim_subseco[Descripción]`, `_Medidas[Consumo total visible]`, `Fact_presupuesto[COP]` |
| image | — | (logo/imagen) |
| textbox | — | (texto) |
| barChart | "Detalle consumo por subseco" | `Dim_subseco[Descripción]`, `_Medidas[Consumo total visible]` |
| shape | — | (fondo) |
| lineStackedColumnComboChart | "Análisis consumo de presupuesto por categoría desagregada" | `Dim_categoria`, `_Medidas[Dentro presupuesto visual]`, `_Medidas[Presupuesto remanente visible > 0]`, `_Medidas[Excedente visual]`, `_Medidas[Presupuesto total Visible]` |
| htmlContent | `Card HTML - Consumo Total` | `_Medidas[Card HTML - Consumo Total]` |
| htmlContent | `Card HTML - Presupuesto Total` | `_Medidas[Card HTML - Presupuesto Total]` |
| image / image | — | (logos/imágenes) |
| pivotTable | matriz | `Dim_categoria[Tipo costo]`, `Dim_categoria[Desc_N2]`, `_Medidas[Presupuesto total]`, `_Medidas[Presupuesto remanente]`, `_Medidas[Costo ejecutado]`, `_Medidas[Costo comprometido]` |

---

### Página 2 — detalle documental (tabla)

| Tipo | Título | Campos |
|---|---|---|
| tableEx | — | `Fact_detalle_documental[Fecha de creación/Contabilización NC]`, `[Tipo de documento]`, `[Número de documento]`, `[Descripción artículo/serv.]`, `[Comentarios]`, `[Estado línea/documento]`, `[Número interno]` |
| slicer | — | `Dim_proyecto[Nombre de proyecto]` |
| slicer | — | `Fact_detalle_documental[Tipo de documento]` |

---

### Detalle documental — matriz + tarjetas

| Tipo | Título | Campos / medidas |
|---|---|---|
| tableEx | — | `Fact_detalle_documental[Fecha de creación/Contabilización NC]`, `[Tipo de documento]`, `[Número de documento]`, `[Descripción artículo/serv.]`, `[Número interno]` |
| shape | — | (fondo) |
| pivotTable | — | `Dim_categoria[Tipo costo/Desc_N2/Desc_N3_OK]`, `Dim_subseco[Descripción]`, `Fact_detalle_documental[Tipo de documento/Número de documento/Total línea]` |
| slicer | — | `Fact_detalle_documental[Tipo de documento]` |
| htmlContent | `Card HTML - SOLPED Abiertas` | `_Medidas[Card HTML - SOLPED Abiertas]` |
| htmlContent | `Card HTML - ODC Cerradas` | `_Medidas[Card HTML - ODC Cerradas]` |
| htmlContent | `Card HTML - Factura Conformada` | `_Medidas[Card HTML - Factura Conformada]` |

---

### Tool tip G1 — tooltip

| Tipo | Título | Medidas |
|---|---|---|
| cardVisual | "Detalle consumo de presupuesto" | `_Medidas[Comprometido excedente]`, `[Ejecutado excedente]`, `[Presupuesto remanente visible > 0]`, `[Comprometido dentro presupuesto]`, `[Ejecutado dentro presupuesto]` |

---

### Solicitud de Pedido

| Tipo | Título | Campos / medidas |
|---|---|---|
| textbox | — | (texto) |
| slicer | "Numero de solicitud de pedido" | `Fact_solicitud_de_pedido[Número de documento]` |
| slicer | "Adjudicación Directa" | `Fact_solicitud_de_pedido[Adjudicacion Directa]` |
| cardVisual | "Solicitudes de pedido" | `Fact_solicitud_de_pedido[Número de documento]` |
| cardVisual | "Líneas de solicitudes de pedido" | `Fact_solicitud_de_pedido[#]` |
| image | — | (imagen) |
| slicer | "Código - Base Cero" | `Dim_categoria[Codigo]` |
| image | — | (imagen) |
| tableEx | — | `Fact_solicitud_de_pedido[#]`, `[Número de documento]`, `[Fecha de creación]`, `[Adjudicacion Directa]`, `[Cancelado]`, `[Unidad]`, `[Total líneas]`, `[Comentarios]`, `Dim_categoria[Codigo/Descripción]`, `Dim_subseco[Descripción]` |
| slicer | "Fecha de Creación" | `Calendario[Date]` |
| slicer | "Descripción - Subseco" | `Dim_subseco[Descripción]` |
| shape | — | (fondo) |
| cardVisual | "Total Solped Abierta" | `Fact_solicitud_de_pedido[Total líneas]` |

---

### Facturas Comformadas

| Tipo | Título | Campos / medidas |
|---|---|---|
| tableEx | — | `Fact_facturas_conformadas[#]`, `[Número interno]`, `[Fecha de contabilización]`, `[Status de documento]`, `[Unidad]`, `[Cantidad]`, `[Total líneas]`, `[Descripción artículo/serv.]`, `[Comentarios]`, `Dim_categoria[Codigo/Descripción]`, `Dim_subseco[Descripción]` |
| image | — | (imagen) |
| slicer | "Estado de la Linea" | `Fact_facturas_conformadas[Status de documento]` |
| slicer | "Código - Base Cero" | `Dim_categoria[Codigo]` |
| slicer | "Descripción - Subseco" | `Dim_subseco[Descripción]` |
| textbox | — | (texto) |
| slicer | "Fecha de Creación" | `Calendario[Date]` |
| slicer | "Numero de Factura Conformada" | `Fact_facturas_conformadas[Número interno]` |
| cardVisual | "Facturas Conformadas" | `Fact_facturas_conformadas[Entrada mercancías]` |
| cardVisual | "Total Facturas Conformadas" | `Fact_facturas_conformadas[Total líneas]` |

---

### Ordenes de Compra

| Tipo | Título | Campos / medidas |
|---|---|---|
| shape | — | (fondo) |
| cardVisual | "Ordenes de Compra" | `Fact_ordenes_de_compra[Orden de Compra]` |
| image | — | (imagen) |
| tableEx | — | `Fact_ordenes_de_compra[#]`, `[Orden de Compra]`, `[Fecha de creación]`, `[Cancelado]`, `[Unidad Medida OC]`, `[Cantidad OC]`, `[Total Líneas OC $Cop]`, `[Descripción general]`, `[Comentarios adicionales]`, `[Detalle adicional]`, `Dim_categoria[Codigo/Descripción]`, `Dim_subseco[Descripción]` |
| slicer | "Estado de la Linea" | `Fact_ordenes_de_compra[Status línea OC]` |
| slicer | "Fecha de Creación" | `Calendario[Date]` |
| slicer | "Numero de Orden de Compra" | `Fact_ordenes_de_compra[Orden de Compra]` |
| slicer | "Código - Base Cero" | `Dim_categoria[Codigo]` |
| slicer | "Descripción - Subseco" | `Dim_subseco[Descripción]` |
| cardVisual | "Total Ordenes de Compra" | `Fact_ordenes_de_compra[Total Líneas OC $Cop]` |
| cardVisual | "Total de lineas" | `Fact_ordenes_de_compra[#]` |

---

## Recursos estáticos

`OTC - Omia Cost Control Tower.Report/StaticResources/`:

- **RegisteredResources** (imágenes): `OMIA_Positivo_Mesa-de-trabajo-…png`, `calendario…png`, `menu_(2)…png`.
- **SharedResources/BaseThemes**: `CY25SU12.json` (tema del reporte).
