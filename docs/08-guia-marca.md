# 08 — Guía de marca e identidad visual (OMIA Colombia S.A.S.)

Reglas de diseño, identidad visual y directrices de marca **oficiales** de OMIA
Colombia S.A.S. Deben aplicarse rigurosamente en la creación, maquetación y
edición de reportes y tableros en Power BI.

## 1. Identidad y Filosofía de Marca

- **Razón Social:** OMIA Colombia S.A.S.
- **Sigla:** OMIA (Operación y Mantenimiento Integral de Activos) — siempre en
  **MAYÚSCULAS**.
- **Filosofía de los 5 Ceros:**
  1. Cero pérdidas
  2. Cero faltas de ética
  3. Cero incidentes ambientales
  4. Cero accidentes
  5. Cero defectos
- **Tono de Comunicación:** Cercano, activo, profesional y transparente. En
  textos descriptivos, usar la primera persona del plural (*"nosotros"*,
  *"nuestro modelo"*).

## 2. Paleta de Colores Corporativa

### Colores Principales

| Color | HEX | RGB |
|---|---|---|
| **Azul OMIA** (Primario / Estructura / Headers) | `#164379` | 22, 68, 121 |
| **Amarillo OMIA** (Acento / Destacados / Energía) | `#F8AA00` | 248, 170, 0 |

### Colores Secundarios (Categorías, desgloses y métricas)

| Color | HEX | RGB |
|---|---|---|
| Azul Claro (secundario estándar) | `#2679BA` | 43, 121, 186 |
| Morado (categorías secundarias) | `#6156A1` | 97, 86, 161 |
| Rojo / Coral (alertas / variación negativa / riesgo) | `#E7344C` | 231, 52, 76 |
| Naranja (advertencias / estados intermedios) | `#EE7600` | 238, 118, 1 |
| Verde Lima (metas cumplidas / acento positivo) | `#B9CE00` | 185, 206, 0 |
| Verde (variación positiva / estados óptimos) | `#71B73A` | 101, 179, 46 |
| Gris Oscuro (textos secundarios / ejes / grillas) | `#575756` | 87, 87, 86 |

### Colores Neutros de Soporte (UI)

| Uso | HEX |
|---|---|
| Fondo de Reporte (Canvas) | `#F4F6F9` o `#FFFFFF` |
| Fondo de Tarjetas / Visuales | `#FFFFFF` |
| Bordes de Contenedores | `#E2E8F0` o `#D1D5DB` |
| Texto Principal | `#1E293B` o `#575756` |

## 3. Tipografía y Jerarquía Textual

- **Familia principal:** `Gotham` (Light, Medium, Bold).
- **Fallbacks admitidos (Power BI):** `Segoe UI`, `Arial`, `Verdana`.

### Reglas de Uso Tipográfico

| Elemento | Fuente / peso | Tamaño | Color |
|---|---|---|---|
| Títulos de página / headers de sección | Gotham Bold / Segoe UI Bold | 14–18 pt | `#164379` |
| Valores de KPI (callout values) | Gotham Bold / Segoe UI Semibold | 20–28 pt | — |
| Etiquetas de KPI y subtítulos | Gotham Medium / Segoe UI | 9–11 pt | `#575756` |
| Encabezados de tablas/matrices | Gotham Medium | 10 pt | texto `#FFFFFF` sobre fondo `#164379` |
| Celdas de datos | Gotham Light / Segoe UI | 9–10 pt | — |

- **Alineación:** texto a la **izquierda**; valores numéricos/monetarios a la
  **derecha**. Prohibido justificar o centrar párrafos explicativos.
- **Tamaño mínimo legible:** 8 pt (solo notas al pie o etiquetas densas).

## 4. Reglas de Diseño Visual (Power BI)

1. **Estructura modular (cards / contenedores):** agrupar métricas afines en
   contenedores blancos con bordes sutiles y esquinas redondeadas (4–8 px);
   márgenes consistentes de 8–16 px entre visuales.
2. **Uso del color en visualizaciones:**
   - No saturar con colores arbitrarios. **Azul OMIA (`#164379`)** como
     barra/línea principal o base comparativa (ej. Presupuesto o año anterior).
   - **Amarillo OMIA (`#F8AA00`)** o **Azul Claro (`#2679BA`)** para la métrica
     actual (Real / Actual).
   - Desviaciones presupuestales: favorable/ahorro → Verde (`#71B73A`) o Verde
     Lima (`#B9CE00`); desfavorable/sobregasto → Rojo (`#E7344C`).
3. **Logotipo y header:**
   - Ubicarlo preferentemente en esquina superior izquierda o derecha.
   - Respetar el área de reserva mínima (diámetro del círculo del logo).
   - Sobre barra de encabezado Azul OMIA (`#164379`), el logo debe ser
     **100% blanco**.

## 5. Prohibiciones Estrictas (DON'Ts)

- **NO** alterar los códigos HEX ni introducir colores fuera de la paleta oficial.
- **NO** usar fondos oscuros saturados para todo el lienzo; priorizar fondos
  claros y limpios.
- **NO** deformar o cambiar las proporciones del logotipo de OMIA.
- **NO** aplicar efectos 3D, sombras excesivas, biseles o gradientes complejos en
  gráficos y tarjetas.
- **NO** usar el logo como marca de agua en el fondo de las páginas de reporte.
