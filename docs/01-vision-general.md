# 01 — Visión general

## Objetivo

**OTC - Omia Cost Control Tower** es un dashboard de **control de gastos /
ejecución presupuestal** de OTC (Omia) para el contrato de mantenimiento
eléctrico de mina (Continental Gold / proyecto `2170 CONTINENTAL-MTO ELECTRICO
MINA BURITICÁ`).

El dashboard contrasta:

- **Presupuesto** (desde el REE / resumen de costo y venta).
- **Costo comprometido** (solicitudes de pedido abiertas).
- **Costo ejecutado** (órdenes de compra cerradas + facturas conformadas).

y expone indicadores como presupuesto remanente, % de ejecución, sobrepresupuesto
y desglose por proyecto, categoría y subseco.

## Flujo de datos

```
SAP (reportes .txt exportados)  ──┐
Excel de dimensiones (.xlsx)     ──┼──► SharePoint (Web.Contents) ──► Power Query (M) ──► Modelo (Import) ──► Reporte (PBIR)
Excel de presupuesto (REE .xlsx) ──┘
```

1. Los reportes de SAP se exportan como `.txt` (TSV) y los maestros como `.xlsx`,
   alojados en una carpeta de SharePoint.
2. Power Query (`Web.Contents`) los consume y aplica limpieza/joins en M.
3. El modelo se carga en modo **Import** y expone tablas `Fact_*` / `Dim_*`.
4. Las medidas DAX (en `_Medidas`) calculan los indicadores.
5. El reporte PBIR presenta las páginas y visuales.

## Arquitectura del repo (PBIP)

```
.
├── OTC - Omia Cost Control Tower.pbip          # proyecto PBIP (apunta al reporte)
├── OTC - Omia Cost Control Tower.SemanticModel # modelo semántico
│   └── definition/
│       ├── model.tmdl, database.tmdl, relationships.tmdl
│       ├── expressions.tmdl                    # consultas ETL_* y Dim_categoria (1)
│       ├── cultures/es-CO.tmdl                 # metadatos lingüísticos
│       └── tables/*.tmdl                       # una tabla por archivo
├── OTC - Omia Cost Control Tower.Report        # reporte PBIR
│   └── definition/pages/<id>/                  # page.json + visual.json por visual
├── .agents/                                    # skills + AGENTS.md
│   └── skills/…
├── docs/                                       # esta documentación
├── opencode.json                               # config MCP (powerbi-modeling)
└── skills-lock.json                            # skills instalados
```

## Herramientas / MCP / Skills

| Recurso | Uso |
|---|---|
| MCP `powerbi-modeling` | Conectar e inspeccionar el modelo (`ConnectFolder`) |
| Skill `powerbi-modeling` | Mejores prácticas de modelado |
| Skill `powerbi-report-authoring` | Editar/validar PBIR |
| Skill `powerbi-report-design` | Guía de diseño de reportes |
| Skill `powerbi-report-planning` | Requisitos → implementación |
| Skill `powerbi-report-management` | Publicación en Fabric |
