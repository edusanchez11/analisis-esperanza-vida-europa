📊 Análisis de Esperanza de Vida en Europa

Proyecto realizado con Python + Power BI

✅ Descripción del Proyecto

Este proyecto analiza la relación entre el gasto sanitario y la esperanza de vida en países europeos, con el objetivo de responder a preguntas clave como:
¿Cuál es el país fuera de España con mayor esperanza de vida?

El análisis combina Python (para limpieza y exploración de datos) y Power BI (para visualización interactiva y storytelling).

🗂 Estructura del Proyecto
```bash
analisis-esperanza-vida-europa/
│
├── data/
│   ├── df_unificado.csv          # Dataset final unificado
│   ├── health_care_expenditure.csv # Datos de gasto sanitario
│   ├── life_expectancy.csv       # Datos de esperanza de vida
│   └── poblacion.csv             # Datos de población
│
├── python/
│   ├── 01_data_exploration_cleaning.ipynb  # Limpieza y EDA
│   └── 02_analysis.ipynb                   # Análisis y métricas
│
├── Sanchez_Eduardo_SaludUE.pbix            # Dashboard en Power BI
├── Sanchez_Eduardo_SaludUE.pdf             # Informe final
└── README.md                               # Este archivo
````

🔍 1. Dataset y Contexto

Los datos provienen de fuentes oficiales europeas (Eurostat, OMS) e incluyen:

✔ Esperanza de vida (años)

✔ Gasto sanitario público per cápita (€)

✔ Población total

✔ Años analizados (2014-2023)


🧹 2. Proceso de Limpieza y Análisis (Python)

🔑 Principales pasos en 01_data_exploration_cleaning.ipynb:

✔ Carga de datasets con Pandas
✔ Unificación por país y año

✔ Conversión de columnas a formato numérico (ej. 81.6 → 81.6)

✔ Eliminación de filas con países agregados (European Union, Euro Area)

✔ Creación de nuevas métricas como:

Gasto per cápita = Gasto Sanitario / Población

Ejemplo de código:
```bash
import pandas as pd

# Cargar datasets
df_gasto = pd.read_csv("data/health_care_expenditure.csv")
df_vida = pd.read_csv("data/life_expectancy.csv")
df_pob = pd.read_csv("data/poblacion.csv")

# Unificar por país y año
df = df_gasto.merge(df_vida, on=["country", "year"])
df = df.merge(df_pob, on=["country", "year"])

# Calcular gasto per cápita
df["gasto_per_capita"] = df["gasto"] / df["poblacion"]

# Eliminar agregados
df = df[~df["country"].isin(["European Union", "Euro Area"])]
````
📈 3. KPIs y Justificación

En Power BI se crearon las siguientes métricas con DAX:
✔ Gasto público por habitante
```DAX
Gasto per cápita = SUM('Fact'[gasto_sanitario]) / SUM('Fact'[poblacion])
````

✔ Esperanza de vida media
```DAX
Esperanza de Vida (media) = AVERAGE('Fact'[esperanza_vida])
````

✔ Relación entre gasto y esperanza de vida
```DAX
Relación Gasto-Vida = DIVIDE([Gasto per cápita], [Esperanza de Vida (media)])
````

✔ Top países con mayor esperanza de vida
Usando TOPN en visualización de tabla.

✔ % de crecimiento interanual (YoY)
```DAX
Vida YoY % = 
VAR Prev = CALCULATE([Esperanza de Vida (media)],
    FILTER(ALL('DimYear'), 'DimYear'[Year] = MAX('DimYear'[Year]) - 1)
)
RETURN DIVIDE([Esperanza de Vida (media)] - Prev, Prev)
````
🎨 4. Diseño y Visualizaciones (Power BI)

📌 Elementos principales:

✔ Mapa interactivo con banderas y colores por esperanza de vida

✔ Gráfico de líneas: evolución de la esperanza de vida por país

✔ KPI Cards: Gasto per cápita y Esperanza de vida media

✔ Segmentadores: País, Año, Región

✔ Botón de navegación a detalle

🎭 5. Interactividad y Storytelling

✔ Filtros dinámicos por país y año

✔ Tooltip personalizado con gasto y esperanza de vida

✔ Narrativa: "¿Dónde se vive más y cómo influye el gasto en salud?"

✅ Conclusiones

📌 El país fuera de España con mayor esperanza de vida en el último año analizado es Italia.

📌 Existe correlación positiva entre gasto sanitario per cápita y esperanza de vida, aunque con diferencias entre países.

📌 Los países nórdicos y mediterráneos presentan mejores indicadores que la media europea.

🚀 Cómo Ejecutar el Proyecto

Clonar repositorio

git clone https://github.com/edusanchez11/analisis-esperanza-vida-europa.git

Abrir Jupyter Notebook para explorar Python (python/ folder).

Abrir Power BI Desktop y cargar el archivo:

Sanchez_Eduardo_SaludUE.pbix

🛠 Tecnologías Utilizadas








📎 Recursos del Proyecto

📄 Informe en PDF

📊 Dashboard en Power BI (.pbix)
