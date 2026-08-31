# 07 — Contexto de negocio

Contexto y reglas de negocio del tablero. Fuente: conversación con el dueño del
proyecto (31 de agosto de 2026). Es la base para futuras decisiones de modelado
y reporte.

## Propósito

Tablero **financiero gerencial** de control de gastos del contrato de
mantenimiento eléctrico (proyecto `2170 CONTINENTAL-MTO ELECTRICO MINA BURITICÁ`).
Debe "hablar por sí mismo" para la gerencia (criterio de diseño a detallar más
adelante).

## Regla de negocio

Hoy el tablero compara **presupuesto vs. lo gastado**, pero esto **va a cambiar**:

> Se van a **eliminar las palabras de "presupuesto"** y el análisis se va a
> concentrar únicamente en **de dónde viene el dinero**: SOLPED, factura
> conformada y ODC.

Es decir, el eje analítico pasará de "presupuesto vs gastado" a "**origen del
dinero**".

## Origen del dinero (taxonomía)

| Origen | Qué es | Mapeo actual en el modelo |
|---|---|---|
| **SOLPED** (solicitud de pedido) | pedidos abiertos | `Costo comprometido = SUM(Fact_solicitud_de_pedido[Total líneas])` |
| **Factura conformada** | facturas aprobadas | parte de `Costo ejecutado` (`SUM(Fact_facturas_conformadas[Total líneas])`) |
| **ODC** (orden de compra) | órdenes cerradas | parte de `Costo ejecutado` (`SUM(Fact_ordenes_de_compra[Total Líneas OC $Cop])`) |
| **Costo entrante** | tipo de entrada de dinero **a definir más adelante** | hoy vacía (código comentado de Facturas Conformadas) |

> Nota: `Costo entrante` es un cuarto origen que se verá más adelante. No
> eliminarla todavía.

## Cadena de cálculo actual (DAX)

| Medida | Expresión |
|---|---|
| `Presupuesto total` | `SUM(Fact_presupuesto[COP])` |
| `Costo comprometido` | `SUM(Fact_solicitud_de_pedido[Total líneas])` |
| `Costo ejecutado` | `SUM(Fact_ordenes_de_compra[Total Líneas OC $Cop]) + SUM(Fact_facturas_conformadas[Total líneas])` |
| `Costo entrante` | *(vacía — a definir)* |
| `Presupuesto remanente` | `[Presupuesto total] − [Costo ejecutado] − [Costo comprometido]` |
| `% Ejecución` | `DIVIDE([Costo ejecutado] + [Costo comprometido], [Presupuesto total])` |

## Cómo conecta el modelo

- Consumo (SOLPED / Facturas / ODC) → `Dim_proyecto` por **`Código de proyecto`**
  (texto); → `Dim_categoria` por `Id categoria`; → `Dim_subseco` por `Id subseco`.
- Presupuesto → `Dim_proyecto` por **`Id proyecto`** (entero); → `Dim_categoria`
  por `Id categoria`.

### Hallazgo: doble clave de proyecto

`Fact_presupuesto` (hoja `Resumen COMPL` del REE "Resumen de costo y venta")
resuelve el proyecto así:
1. Toma `Proyecto = "MANTENIMIENTO ELECTRICO CONTINENTAL GOLD"` y lo reemplaza
   **hardcodeado** por `"COMIMANCCCG5058"`.
2. Hace `NestedJoin` contra `Dim_proyecto` por **`Código de proyecto`** para
   obtener `Id proyecto`.

Resultado: **dos rutas** hacia `Dim_proyecto` (presupuesto por `Id`, consumo por
`Código`). Coherente por construcción, pero **frágil** si cambia el nombre en el
REE o la correspondencia Id↔Código. **Pendiente de revisar/unificar.**

## Pendientes / decisiones abiertas

1. **Reframe de naming**: quitar "presupuesto" del lenguaje de medidas y
   visuales; reorientar a origen del dinero (SOLPED / Factura / ODC / Entrante).
2. **`Costo entrante`**: definir qué representa y si se reactiva.
3. **Clave de proyecto**: revisar si conviene unificar presupuesto y consumo a
   una sola columna de `Dim_proyecto`.
