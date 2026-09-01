# AGENTS.md

Guía para agentes (IA y humanos) que trabajan en este repositorio.

## Proyecto

**OTC - Omia Cost Control Tower**: dashboard Power BI de control de gastos /
ejecución presupuestal (contrato de mantenimiento eléctrico, proyecto
`2170 CONTINENTAL-MTO ELECTRICO MINA BURITICÁ`). Contraste de presupuesto vs.
costo comprometido (SOLPED) y ejecutado (OC + facturas).

## Uso de MCP y skills (obligatorio)

Antes de diagnosticar o editar cualquier parte del proyecto, usa siempre los
recursos disponibles en lugar de resolver manualmente:

- Conecta primero con el MCP de modelado (`ConnectFolder` a
  `OTC - Omia Cost Control Tower.SemanticModel/definition`).
- Carga el skill relevante con la herramienta `skill("<nombre>")` antes de
  trabajar en esa área (modelado, reporte, diseño, planning, gestión).
- Usa el MCP de navegador o de búsqueda web para verificación y contexto
  externo cuando aplique.
- La lista completa está en la sección `## Skills y MCP disponibles`.

## Contexto de negocio (importante)

- Es un tablero **financiero gerencial** que debe "hablar por sí mismo".
- Hoy compara **presupuesto vs. gastado**, pero **esto cambiará**: se eliminarán
  las palabras de "presupuesto" y el análisis se concentrará en el **origen del
  dinero**.
- Origen del dinero (taxonomía):
  - **SOLPED** (solicitud de pedido) → `Costo comprometido`
  - **Factura conformada** → parte de `Costo ejecutado`
  - **ODC** (orden de compra) → parte de `Costo ejecutado`
  - **Costo entrante** → tipo de entrada **a definir más adelante** (hoy vacía)
- Conexión clave de proyecto: presupuesto entra por `Dim_proyecto.Id proyecto`
  (entero) y consumo por `Dim_proyecto.Código de proyecto` (texto). Presupuesto
  deriva `Id` desde `Código` con un reemplazo hardcodeado (`"MANTENIMIENTO
  ELECTRICO CONTINENTAL GOLD"` → `"COMIMANCCCG5058"`). Frágil; pendiente de
  unificar.
- Detalle completo en `docs/07-contexto-negocio.md`.

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

Carga los skills con la herramienta `skill("<nombre>")`. Conecta al modelo
siempre con el MCP `powerbi-modeling`.

| Recurso | Tipo | Cuándo usarlo |
|---|---|---|
| `powerbi-modeling` | MCP | Conectar (`ConnectFolder`), inspeccionar y editar el modelo semántico |
| `brave-search-remoto` | MCP | Búsqueda web, noticias e imágenes para contexto externo |
| `playwright-remoto` | MCP | Navegador automatizado: verificar render, capturas, interacción |
| `powerbi-modeling` | Skill | Buenas prácticas de modelado (DAX, star schema, RLS, rendimiento) |
| `powerbi-report-authoring` | Skill | Editar y validar PBIR/PBIP: páginas, visuales, temas, formato |
| `powerbi-report-design` | Skill | Diseño visual y guías de estilo antes de escribir PBIR |
| `powerbi-report-planning` | Skill | Requisitos → implementación de reportes nuevos |
| `powerbi-report-management` | Skill | Publicar/gestionar reportes en Fabric |

Nota: `customize-opencode` y `find-skills` (built-in) aplican solo a la
configuración de opencode o a descubrir más skills, no al proyecto.

## Marca e identidad visual (obligatorio)

Toda edición de visuales, temas y formato debe seguir la guía oficial de OMIA
(`docs/08-guia-marca.md`). Resumen imprescindible:

- **Colores**: Azul OMIA `#164379` (primario), Amarillo OMIA `#F8AA00` (acento).
  Secundarios: `#2679BA`, `#6156A1`, `#E7344C`, `#EE7600`, `#B9CE00`, `#71B73A`,
  `#575756`. Fondos claros (`#F4F6F9`/`#FFFFFF`).
- **Tipografía**: `Gotham` (fallback Segoe UI/Arial/Verdana). Títulos 14–18 pt
  en `#164379`; KPI 20–28 pt; etiquetas 9–11 pt `#575756`; tablas con encabezado
  `#164379` + texto blanco.
- **NO** usar colores fuera de paleta, fondos oscuros, efectos 3D/sombras/gradientes,
  ni el logo como marca de agua.
- Desviaciones: favorable/ahorro → verde (`#71B73A`/`#B9CE00`); desfavorable →
  rojo (`#E7344C`).

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
9. **Usa MCP y skills disponibles**: si existe un MCP o skill que cubra la
   tarea, úsalo primero (herramienta `skill` / MCP `powerbi-modeling`); no lo
   reimplementes manualmente.

## Pendientes conocidos

Ver `docs/05-diagnostico-estado.md`: clave de proyecto mixta, `Costo entrante`
vacía, calendario sin marcar, sin RLS, sin descripciones, nombres inconsistentes,
tabla `Dim_Subseco stanby` duplicada y URLs de SharePoint versionadas.

## Decisiones de negocio abiertas

Ver `docs/07-contexto-negocio.md`:
1. Reframe de naming (quitar "presupuesto", reorientar a origen del dinero).
2. Definir `Costo entrante`.
3. Unificar clave de proyecto (presupuesto vs. consumo).
