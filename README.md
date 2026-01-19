# 🎯 SentimentAPI v4.0 - Multilingüe ES+PT

## 📋 Descripción del Proyecto

**SentimentAPI v4.0** es un MVP de análisis de sentimientos **multilingüe (Español + Portugués)** que combina un pipeline de Machine Learning en Python (TF‑IDF + Regresión Logística calibrada) con un backend en Spring Boot y persistencia en PostgreSQL.
El sistema recibe textos libres (reseñas/comentarios), predice **Negativo / Neutro / Positivo** y expone los resultados vía API REST, dejando el cálculo de métricas y visualizaciones al Frontend.

### Clasificaciones disponibles

- 🟢 **Positivo** (estrellas 4–5)  
- 🟡 **Neutro** (estrella 3)  
- 🔴 **Negativo** (estrellas 1–2)

### Características principales

- ✅ Modelo **multilingüe ES+PT** en un único pipeline y artefacto `.joblib`.
- ✅ Probabilidades **calibradas** y umbral dinámico para `review_required`
- ✅ Persistencia de interacciones en **PostgreSQL** (usuarios, roles e historial de análisis).
- ✅ Backend Spring Boot con endpoints de **registro/login** y consumo de la API de Python vía `WebClient`. 

***

## 🕒 Línea de tiempo del MVP (v1 → v4)

La evolución del proyecto sigue tres fases de Data Science más la integración completa en v4. 

### 🔹 Fase 1 — Exploración y baseline (v1 · Español)

- Modelo inicial con **TF‑IDF (unigramas/bigramas) + Regresión Logística** sobre reseñas en español.
- Limpieza basada en regex, sin lematización ni pipelines pesados; foco en **factibilidad técnica** y bajo costo computacional.

### 🔹 Fase 2 — Robustez estadística (v2 · Español)

- Incorporación de **Naive Bayes** como baseline comparativo y **cross-validation estratificada** para medir estabilidad. 
- Introducción de la regla de **revisión humana por incertidumbre** (`review_required` cuando la probabilidad máxima cae por debajo de un threshold). 

### 🔹 Fase 3 — Modelo productivo multilingüe (v3 · ES+PT)

- Ampliación del dataset a **Español + Portugués** manteniendo un único modelo.
- Uso de **RandomizedSearchCV** y **CalibratedClassifierCV** para obtener probabilidades confiables y selección automática de umbral.

### 🔹 Fase 4 — SentimentAPI v4.0 (ES+PT + Backend)

- Consolidación del pipeline multilingüe en `Proyecto_v12.ipynb` y generación del bundle productivo.
- Integración con backend Java, persistencia en PostgreSQL y preparación para explotación desde un Frontend de dashboards.

***

## 📓 Notebooks Principales

### 1. `Proyecto_v12.ipynb` — Modelo Multilingüe ES+PT (Core DS)

Este notebook implementa el pipeline completo de ML para **entrenamiento, calibración y serialización** del modelo multilingüe. 

Incluye:

- Carga de datos estratificados (`train_es_pt.csv`, `validation_es_pt.csv`, `test_es_pt.csv`).  
- Transformación de `stars` (1–5) a sentimiento ternario (Negativo / Neutro / Positivo).  
- Limpieza de texto **Unicode-aware** y construcción de `text_raw` / `text_clean`. 
- Pipeline:  
  - `TF-IDF` (n‑grams, límites de frecuencia, max_features)  
  - `LogisticRegression` + **CalibratedClassifierCV**  
- Selección de **threshold óptimo** a partir de la relación coverage vs recall de la clase Negativo. 
- Evaluación global y por idioma (ES vs PT) y generación del artefacto `.joblib` con metadata (versión, hash, parámetros). 
> 📂 Nota: Los archivos `train_es_pt.csv`, `validation_es_pt.csv` y `test_es_pt.csv` se generarán y estarán disponibles en el repositorio o vía enlace externo (pendiente de publicación). 

### 2. `Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb`

Notebook responsable de **unificar las fuentes de datos** y generar los splits estratificados para entrenamiento, validación y test. 

Hace:

- Carga de reseñas en español (`train.csv`) y portugués (`reviews_consolidado_perfecto_v2.csv`). 
- Normalización de columnas: `stars`, `review_title`, `review_body`, `language`. 
- Construcción de `text_raw` y mapping `stars → sentiment` (`Negativo`, `Neutro`, `Positivo`). 
- Generación de un estrato `language||sentiment` y split 80/10/10 con `train_test_split` estratificado.  
- Exportación de:  
  - `train_es_pt.csv`  
  - `validation_es_pt.csv`  
  - `test_es_pt.csv`  

> 🔗 Estos archivos se asumen **presentes** para ejecutar `Proyecto_v12.ipynb`; la ubicación o link definitivo se documentará en el repositorio de datos una vez estén disponibles. 

***

## 🏗️ Arquitectura General

La solución completa combina el pipeline de ML en Python con un backend Java y persistencia en PostgreSQL.

```text
┌─────────────────────────────────────────────────────────────────────┐
│                    SentimentAPI v4.0 (ES + PT)                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  📊 Data Science (Python)            🐿 Backend (Spring Boot)       │
│  ─────────────────────────            ─────────────────────────     │
│  • Notebooks de consolidación        • API REST /project/api/v2     │
│    y entrenamiento                   • Integración con modelo       │
│  • TF-IDF + Logistic Regression        Python vía WebClient         │
│  • Calibración de probabilidades     • Persistencia en PostgreSQL   │
│  • Bundle .joblib + metadata         • Seguridad con BCrypt         │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

***

## 🔌 Backend API y Persistencia

La versión v4 incorpora un backend en Spring Boot con foco en seguridad, persistencia y exposición de servicios para el Frontend. 

### 1. Modelado de Base de Datos y Persistencia

- Migración a **PostgreSQL** con la base de datos `hackathonone` usando **Spring Data JPA**.
- Entidades core:  
  - `User` y `Rol` para gestión de autenticación/autorización.  
  - `Interaccion` para almacenar el historial de análisis de sentimientos. 
- Seguridad: uso de **BCrypt** para almacenar contraseñas hash en lugar de texto plano. 

### 2. Endpoints y Lógica de Negocio

- Nuevos servicios bajo el contexto:  
  - `/project/api/v2/usuario` para **registro** y **login**.
- Procesamiento de sentimientos:  
  - El backend consume la API de Python mediante `WebClient`, obteniendo **etiqueta de sentimiento** y **score de confianza**. 
- Respuesta JSON estructurada que incluye:  
  - Texto analizado.  
  - Sentimiento predicho (Negativo / Neutro / Positivo).  
  - Probabilidad / score de confianza.  
  - Metadatos de la interacción (usuario, timestamp, etc.).
### 3. Conectividad y Visualización (Back–Front)

- El backend se centra en entregar **data cruda** lista para explotar. 
- El Frontend es responsable de:  
  - Consumir el JSON retornado por la API.  
  - Agregar resultados (por ejemplo, conteo de sentimientos por tipo, tendencias).  
  - Renderizar métricas visuales usando librerías como **Chart.js** o **Recharts**. 

### 4. Configuración del Servidor

- Backend ejecutándose en el **puerto 8080**, con validaciones mediante **Jakarta Validation**. 
- Más detalles de implementación pueden revisarse en el repositorio de backend:  
  👉 [Repositorio Backend – SentimentAPI](https://github.com/Jona-9/SentimentAPI) 

***

## 🚀 Flujo de Ejecución (alto nivel)

1. **Consolidar dataset multilingüe**  
   - Ejecutar `Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb` para generar `train_es_pt.csv`, `validation_es_pt.csv` y `test_es_pt.csv` (o descargarlos desde el repositorio de datos cuando estén publicados).

2. **Entrenar y calibrar el modelo**  
   - Ejecutar `Proyecto_v12.ipynb` para entrenar el modelo, realizar tuning/calibración y generar el artefacto `.joblib` con su metadata y threshold. 

3. **Integrar con Backend**  
   - Configurar la API Python para exponer el endpoint de inferencia.  
   - Levantar el backend Spring Boot, configurar PostgreSQL y revisar los endpoints `/project/api/v2/usuario` y los dedicados a análisis de sentimientos. 

4. **Consumir desde Frontend**  
   - Construir un cliente que consuma el JSON del backend y renderice dashboards de sentimiento, métricas de confianza, etc. 

***

## 📁 Estructura sugerida del repositorio DS

```text
📦 sentimentapi-v4-es-pt/
├── 📓 Proyecto_v12.ipynb
├── 📓 Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb
├── 📊 train_es_pt.csv                 # (referenciado, se agregará vía link/repositorio)
├── 📊 validation_es_pt.csv           # (referenciado, se agregará vía link/repositorio)
├── 📊 test_es_pt.csv                 # (referenciado, se agregará vía link/repositorio)
├── 📄 README.md                      # Este documento
└── 📄 requirements.txt               # Dependencias Python (scikit-learn, pandas, etc.)
```

***
## 👥 Equipo

Proyecto desarrollado por **"No Data - No Code"** en el marco del Hackatón **No Country** 🌎

## 📄 Licencia

Este proyecto está bajo licencia MIT.

***
