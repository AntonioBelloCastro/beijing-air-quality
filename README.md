# Predicción de PM2.5 en Beijing — Series Temporales

Proyecto de la asignatura **Aprendizaje Automático en Series Temporales y Flujo de Datos**
(Máster en IA Aplicada, UC3M).

## 1. Descripción del problema

La contaminación por partículas finas (PM2.5) es uno de los principales problemas de
calidad del aire en grandes ciudades. 
Rellenarlo con lo que pongamos en la memoria

## 2. Dataset

- **Fuente:** [Beijing PM2.5 Data — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/381/beijing+pm2+5+data)
- **Descripción:** Mediciones horarias de PM2.5 en la estación de la Embajada de EE. UU.
  en Beijing, entre el 1 de enero de 2010 y el 31 de diciembre de 2014, junto con
  variables meteorológicas asociadas.
- **Variables principales:**
  | Variable | Descripción |
  |---|---|
  | `year`, `month`, `day`, `hour` | Marca temporal (se combinan en un índice datetime) |
  | `pm2.5` | Concentración de PM2.5 (µg/m³) — variable objetivo |
  | `DEWP` | Punto de rocío (°C) |
  | `TEMP` | Temperatura (°C) |
  | `PRES` | Presión atmosférica (hPa) |
  | `cbwd` | Dirección combinada del viento (categórica) |
  | `Iws` | Velocidad acumulada del viento (m/s) |
  | `Is` | Horas acumuladas de nieve |
  | `Ir` | Horas acumuladas de lluvia |
- **Granularidad:** horaria (~43.800 registros).
- **Consideraciones conocidas:** existen valores ausentes en `pm2.5` (especialmente al
  inicio de la serie) que deberán tratarse explícitamente durante el preprocesado.

El fichero original (`data/raw/beijing_data.csv`) se mantiene sin modificar;

## 3. Equipo

| Integrante | Email |
|---|---|
| Álvaro Alonso Haya | |
| Antonio Ángel Bello Castro | |
| Martín Hernández Tonzán| |

## 4. Estructura del repositorio

```
beijing-air-quality/
├── README.md
├── .gitignore
├── data/
│   ├── raw/                 # Datos originales de UCI, sin modificar
│   └── processed/            # Datos finales listos para modelado (no versionados)
├── notebooks/
│   ├── experimentation/      # Notebooks de exploración y pruebas por etapa
│   └── notebook_final.ipynb  # Cuaderno de entrega: integra el pipeline completo
├── src/                       # Código reutilizable importado desde los notebooks
│   ├── preprocessing/         # Limpieza, imputación, feature engineering, splits temporales
│   ├── models/                # Baselines y modelos de pronóstico
│   └── evaluation/            # Métricas, validación temporal, comparación de modelos
└── docs/
    └── propuesta/             # Propuesta de proyecto enviada/aprobada
```

### Lógica de la estructura

- **`data/`** separa claramente lo *raw* (intocable) de lo *interim/processed*
  (regenerable), evitando ambigüedad sobre qué versión de los datos se está usando en
  cada notebook.
- **`notebooks/experimentation/`** aloja el trabajo exploratorio y las pruebas por
  etapa (EDA, preprocesado, baselines, modelado avanzado, evaluación), numerados para
  reflejar el orden del proceso. Es el espacio de trabajo libre del grupo.
- **`notebooks/notebook_final.ipynb`** es el cuaderno de entrega definitivo: debe
  poder ejecutarse de principio a fin y reflejar de forma reproducible los resultados
  presentados en la memoria, apoyándose en el código de `src/`.
- **`src/`** contiene funciones reutilizables (carga y limpieza de datos, construcción
  de features, definición de modelos, métricas y validación) para no duplicar lógica
  entre notebooks de experimentación y el notebook final, favoreciendo reproducibilidad
  y consistencia.
- **`docs/propuesta/`** conserva la propuesta de proyecto presentada y aprobada por el
  profesor.

## 5. Metodología prevista

1. **Definición y contextualización del problema**, con breve revisión de enfoques
   relacionados (modelos estadísticos clásicos, ML y deep learning para calidad del
   aire).
2. **Descripción y preparación de los datos**: tratamiento de valores ausentes,
   codificación de variables categóricas (`cbwd`), construcción de un índice temporal
   y análisis de calidad/granularidad.
3. **Análisis exploratorio (EDA)**: estacionalidad, tendencia, autocorrelación,
   relación con variables meteorológicas.
4. **Estrategia de modelado y evaluación**: definición del horizonte de predicción,
   partición temporal train/validation/test (sin fugas de información), métricas de
   error y modelos baseline (p. ej. persistencia, media móvil).
5. **Modelado**: aplicación de técnicas vistas en la asignatura (y, si procede,
   técnicas adicionales justificadas) para el pronóstico de PM2.5.
6. **Evaluación y comparación** de modelos con metodología apropiada para series
   temporales (validación *walk-forward* / *time series split*).
7. **Análisis crítico de resultados y conclusiones**, relacionándolos con el problema
   planteado inicialmente.

## 6. Entregables y fechas clave

| Fecha | Hito |
|---|---|
| 22 de septiembre | Envío de la propuesta de proyecto |
| 29 de septiembre | Sesión de seguimiento (formativa, no evaluable) |
| 4 de octubre | Entrega definitiva: memoria, cuaderno de Colab y vídeo (≤10 min) |
| 5–10 de octubre | Defensa y validación individual (sesión síncrona online) |


## 7. Cómo trabajar en este repositorio

- Los notebooks de exploración van en `notebooks/experimentation/`; el cuaderno de
  entrega final se mantiene siempre en `notebooks/notebook_final.ipynb`.
- El código reutilizable (funciones de limpieza, modelos, métricas) se ubica en `src/`
  para mantener los notebooks legibles y evitar duplicación.
- `data/raw/` no se modifica nunca manualmente; cualquier transformación se genera por
  código a partir de él.
- Antes de dar por buena una comparación de modelos, verificar que no existe fuga de
  información entre train/validation/test (p. ej. normalización ajustada solo con
  train, *lags*/features construidos sin mirar al futuro).
