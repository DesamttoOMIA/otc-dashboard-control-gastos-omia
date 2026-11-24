# Contribuir a otc-dashboard-control-gastos-omia

## Modelo de ramas

| Rama | Propósito | Quién escribe |
|---|---|---|
| `main` | Producción | Solo mediante PR desde `dev`, promovido por el owner |
| `dev` | Integración (rama por defecto) | Mediante PR desde ramas de trabajo |
| `feat/*`, `fix/*`, `chore/*`, `docs/*`, `refactor/*` | Trabajo diario | Autor del cambio |

Reglas:

1. Nunca se hace push directo a `dev` ni a `main`.
2. Toda rama de trabajo nace de `dev` y muere al mergearse.
3. Los PRs de trabajo apuntan siempre a `dev`.
4. `dev` → `main` es un PR abierto por el owner (`DesamttoOMIA`). No requiere
   aprobación de otro colaborador, pero el check `solo-owner-promueve`
   exige que el PR venga de `dev` y sea del owner. Si un colaborador abre un
   PR a `main`, el check lo bloquea: se cierra y el owner abre el suyo.
5. No se permite bypass de los rulesets, ni siquiera para el owner.

## Revisión cruzada

Somos dos colaboradores y GitHub no permite autoaprobarse, por lo que cada PR
a `dev` necesita la revisión del otro:

- PR de `gajhomia` → lo revisa `DesamttoOMIA`.
- PR de `DesamttoOMIA` → lo revisa `gajhomia`.

Un PR no se mergea con conversaciones abiertas ni con cambios posteriores sin
aprobar.

## Commits

Se usa [Conventional Commits](https://www.conventionalcommits.org/):

```text
feat: agrega gráfica de consumo por subseco
fix: corrige color de etiqueta del eje en gráfica de subcecos
docs: actualiza guía de marca
chore: actualiza dependencias
refactor: separa medida de color por nivel REE
```

Tipos permitidos: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`,
`build`, `ci`, `chore`, `revert`.

## Pull requests

- Título en formato Conventional Commits.
- Descripción con contexto, cambios y cómo probarlo.
- Enlazar el issue con `Closes #N` cuando aplique.
- PRs pequeños y con un solo propósito.
- Marcar el tipo con los labels `type/*`.

## Merges

| Origen → destino | Método |
|---|---|
| Rama de trabajo → `dev` | Squash merge |
| `dev` → `main` | Merge commit |

El merge a `dev` requiere 1 aprobación, rama actualizada y conversaciones
resueltas. El merge a `main` requiere el check `solo-owner-promueve` en verde.

## Labels

- `type/*`: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`, `ci`,
  `epic`, `sub-epic`
- `area/*`: `modelo`, `reporte`, `datos`, `pipeline`, `github`
- `priority/*`: `p0-critical`, `p1-high`, `p2-medium`, `p3-low`
- `status/*`: `needs-triage`, `in-progress`, `needs-review`, `blocked`

## Reglas de higiene del PBIP

- No commitear secretos, `.env`, claves ni credenciales.
- No commitear archivos locales de Power BI (`.pbi/localSettings.json`,
  `.pbi/cache.abf`) ni archivos de agentes/IDE.
- Antes de abrir el PR, correr el `validate` del PBIR: 0 errores nuevos.
- No reescribir historia de ramas compartidas (`force push` bloqueado).
