# 05 — Diagnóstico del estado actual

Consolidado de la inspección del modelo semántico, las relaciones, las medidas y
los orígenes de datos.

## Hallazgos

### 1. Clave de proyecto heterogénea
`Fact_presupuesto` se relaciona por `Id proyecto` (int) mientras los demás hechos
usan `Código de proyecto` (texto). Coexisten dos rutas de join hacia `Dim_proyecto`.

### 2. Sin relaciones inactivas
Las 18 relaciones están activas. `Fact_detalle_documental` comparte dimensiones
con los hechos fuente, lo que puede generar doble filtrado si se cruzan.

### 3. `Costo entrante` vacía
La medida `Costo entrante` no tiene expresión (código comentado). Además,
`Costo ejecutado` suma dos hechos (`Fact_ordenes_de_compra` +
`Fact_facturas_conformadas`) sin `CALCULATE`/`USERELATIONSHIP`, con riesgo de
doble conteo por fan-out con dimensiones compartidas.

### 4. Calendario sin marcar como tabla de fechas
`Calendario` es calculada pero no está marcada como tabla de fechas y la
Time Intelligence está deshabilitada (`__PBI_TimeIntelligenceEnabled=0`).
No hay medidas YTD/MoM disponibles.

### 5. Sin seguridad (RLS)
No existen roles de seguridad definidos en el modelo.

### 6. Sin documentación embebida
Tablas, columnas y medidas no tienen `description` (descripciones) en el modelo.

### 7. Nombres inconsistentes
- `Adjudicacion Directa` vs `Adjudicación Directa`
- `Código Proyecto` (OC) vs `Código de proyecto` (resto)
- Columnas clave con `summarizeBy: sum` indebido (p. ej. algunos IDs, `PU OC`)
- Formatos mixtos: `Total factura conformada` usa `formatString` es-US, el resto es-CO

### 8. Tabla duplicada / experimental
`Dim_Subseco stanby` es una tabla oculta calculada (`DATATABLE`) que replica
lógica de clasificación por subseco (`Equivalencia`, `Tipo de Costo`), aparente
banco de pruebas.

### 9. URLs versionadas y frágiles
Los orígenes apuntan a archivos con nombres versionados
(`REV0_0511_12 Meses`, `(1)`, etc.) en SharePoint. Cualquier cambio de nombre
rompe la carga hasta actualizar la URL.

## Riesgo acumulado

| Riesgo | Impacto |
|---|---|
| Doble conteo en medidas de costo | Cálculos incorrectos de ejecución/sobrecosto |
| Claves mixtas de proyecto | Filtros no válidos entre presupuesto y consumo |
| Sin RLS | Datos sensibles expuestos a todos los usuarios |
| URLs frágiles | Falla de refresco sin aviso |
