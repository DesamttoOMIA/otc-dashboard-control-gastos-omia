# OTC - Omia Cost Control Tower — Documentación

Documentación del **estado actual** del proyecto. Describe qué es, cómo está
estructurado y el diagnóstico técnico al momento de su redacción
(31 de agosto de 2026).

## Índice

| Doc | Contenido |
|---|---|
| [01-vision-general.md](01-vision-general.md) | Objetivo del proyecto, flujo de datos y arquitectura PBIP |
| [02-origenes-datos.md](02-origenes-datos.md) | Inventario de orígenes de datos (SharePoint, tablas calculadas y ETLs) |
| [03-modelo-semantico.md](03-modelo-semantico.md) | Tablas, relaciones y medidas DAX |
| [04-reporte.md](04-reporte.md) | Páginas y visuales del reporte |
| [05-diagnostico-estado.md](05-diagnostico-estado.md) | Hallazgos del diagnóstico y puntos de atención |

## Cómo se generó esta documentación

- **Modelo semántico**: con el MCP `powerbi-modeling` (conexión `ConnectFolder` a
  `OTC - Omia Cost Control Tower.SemanticModel/definition`) y lectura directa de los
  archivos TMDL.
- **Reporte**: con el skill `powerbi-report-authoring` y lectura de los
  `page.json` / `visual.json` del proyecto PBIR.
- **Diagnóstico**: consolidación de la inspección de relaciones, medidas y orígenes.

## Convenciones del repo

- Idioma del modelo y reporte: `es-CO`.
- Tablas: prefijo `Fact_` (hechos), `Dim_` (dimensiones), `ETL_` (consultas de
  transformación), `_Medidas` (contenedor de medidas).
- Medidas agrupadas en `displayFolder` (carpetas de visualización).
