# AGENTS.md

Guía para agentes (IA y humanos) que trabajan en este repositorio.

## Proyecto

**OTC - Omia Cost Control Tower**: dashboard Power BI de control de gastos /
ejecución presupuestal (contrato de mantenimiento eléctrico, proyecto
`2170 CONTINENTAL-MTO ELECTRICO MINA BURITICÁ`). Contraste de presupuesto vs.
costo comprometido (SOLPED) y ejecutado (OC + facturas).

## Estructura

```
OTC - Omia Cost Control Tower.pbip           # proyecto PBIP
OTC - Omia Cost Control Tower.SemanticModel/ # modelo (TMDL en definition/)
OTC - Omia Cost Control Tower.Report/        # reporte (PBIR en definition/)
.agents/skills/                              # skills instalados
docs/                                        # documentación del estado actual
opencode.json                                # config MCP
skills-lock.json                             # skills instalados (lock)
```

## Comandos útiles

- Conectar al modelo con el MCP:
  `connection_operations` → `ConnectFolder` apuntando a
  `OTC - Omia Cost Control Tower.SemanticModel/definition`.
- Inspeccionar: `table_operations(List)`, `relationship_operations(List)`,
  `measure_operations(List)`, `model_operations(Get)`.
- Validar DAX: `dax_query_operations(Validate)`.
- Git: el repo usa `git` en rama `feature/jean`.

## Skills y MCP disponibles

| Recurso | Cuándo usarlo |
|---|---|
| MCP `powerbi-modeling` | Inspeccionar/editar el modelo semántico |
| `powerbi-modeling` | Mejores prácticas de modelado |
| `powerbi-report-authoring` | Editar/validar PBIR |
| `powerbi-report-design` | Diseño visual |
| `powerbi-report-planning` | Requisitos → implementación |
| `powerbi-report-management` | Publicación en Fabric |

## Convenciones del modelo

- Cultura `es-CO` en todo el modelo y reporte.
- Prefijos: `Fact_` (hechos), `Dim_` (dimensiones), `ETL_` (transformaciones),
  `_Medidas` (contenedor de medidas).
- Medidas agrupadas en `displayFolder`.
- Todo en modo **Import**; orígenes vía `Web.Contents` a SharePoint.

## Reglas para agentes

1. **Conecta primero** con el MCP (`ConnectFolder`) antes de diagnosticar/editar
   el modelo; no asumas la estructura desde memoria.
2. **No rompas relaciones** existentes al crear tablas/columnas; revisa
   cardinalidad y cross-filter antes de activar/desactivar.
3. **Usa medidas** explícitas antes que columnas calculadas o medidas implícitas.
4. **Respeta es-CO** en formatos y cultura (`formatString`, `culture`).
5. **Verifica** cambios de DAX con `dax_query_operations(Validate)`.
6. **No expongas** credenciales ni URLs con secretos en commits.
7. **No versiones** archivos locales de `.pbi/` (ya en `.gitignore`).
8. Al editar TMDL, mantén un objeto por archivo en `definition/tables/` y
   declara `ref table` en `model.tmdl`.

## Pendientes conocidos

Ver `docs/05-diagnostico-estado.md`: clave de proyecto mixta, `Costo entrante`
vacía, calendario sin marcar, sin RLS, sin descripciones, nombres inconsistentes,
tabla `Dim_Subseco stanby` duplicada y URLs de SharePoint versionadas.
