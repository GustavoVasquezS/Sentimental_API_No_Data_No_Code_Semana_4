# 🎯 SentimentAPI v4.0 - Multilingüe ES+PT

## 📋 Descripción del Proyecto

**SentimentAPI v4.0** es un MVP de análisis de sentimientos **multilingüe (Español + Portugués)** que combina un pipeline de Machine Learning en Python (TF‑IDF + Regresión Logística calibrada) con un backend en Spring Boot y persistencia en PostgreSQL. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)
El sistema recibe textos libres (reseñas/comentarios), predice **Negativo / Neutro / Positivo** y expone los resultados vía API REST, dejando el cálculo de métricas y visualizaciones al Frontend. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

### Clasificaciones disponibles

- 🟢 **Positivo** (estrellas 4–5)  
- 🟡 **Neutro** (estrella 3)  
- 🔴 **Negativo** (estrellas 1–2) [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)

### Características principales

- ✅ Modelo **multilingüe ES+PT** en un único pipeline y artefacto `.joblib`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)
- ✅ Probabilidades **calibradas** y umbral dinámico para `review_required` (revisión humana por baja confianza). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- ✅ Persistencia de interacciones en **PostgreSQL** (usuarios, roles e historial de análisis). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- ✅ Backend Spring Boot con endpoints de **registro/login** y consumo de la API de Python vía `WebClient`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

***

## 🕒 Línea de tiempo del MVP (v1 → v4)

La evolución del proyecto sigue tres fases de Data Science más la integración completa en v4. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

### 🔹 Fase 1 — Exploración y baseline (v1 · Español)

- Modelo inicial con **TF‑IDF (unigramas/bigramas) + Regresión Logística** sobre reseñas en español. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- Limpieza basada en regex, sin lematización ni pipelines pesados; foco en **factibilidad técnica** y bajo costo computacional. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

### 🔹 Fase 2 — Robustez estadística (v2 · Español)

- Incorporación de **Naive Bayes** como baseline comparativo y **cross-validation estratificada** para medir estabilidad. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- Introducción de la regla de **revisión humana por incertidumbre** (`review_required` cuando la probabilidad máxima cae por debajo de un threshold). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

### 🔹 Fase 3 — Modelo productivo multilingüe (v3 · ES+PT)

- Ampliación del dataset a **Español + Portugués** manteniendo un único modelo. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)
- Uso de **RandomizedSearchCV** y **CalibratedClassifierCV** para obtener probabilidades confiables y selección automática de umbral. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)

### 🔹 Fase 4 — SentimentAPI v4.0 (ES+PT + Backend)

- Consolidación del pipeline multilingüe en `Proyecto_v12.ipynb` y generación del bundle productivo. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)
- Integración con backend Java, persistencia en PostgreSQL y preparación para explotación desde un Frontend de dashboards. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

***

## 📓 Notebooks Principales

### 1. `Proyecto_v12.ipynb` — Modelo Multilingüe ES+PT (Core DS)

Este notebook implementa el pipeline completo de ML para **entrenamiento, calibración y serialización** del modelo multilingüe. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)

Incluye:

- Carga de datos estratificados (`train_es_pt.csv`, `validation_es_pt.csv`, `test_es_pt.csv`).  
- Transformación de `stars` (1–5) a sentimiento ternario (Negativo / Neutro / Positivo).  
- Limpieza de texto **Unicode-aware** y construcción de `text_raw` / `text_clean`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)
- Pipeline:  
  - `TF-IDF` (n‑grams, límites de frecuencia, max_features)  
  - `LogisticRegression` + **CalibratedClassifierCV**  
- Selección de **threshold óptimo** a partir de la relación coverage vs recall de la clase Negativo. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)
- Evaluación global y por idioma (ES vs PT) y generación del artefacto `.joblib` con metadata (versión, hash, parámetros). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)

> 📂 Nota: Los archivos `train_es_pt.csv`, `validation_es_pt.csv` y `test_es_pt.csv` se generarán y estarán disponibles en el repositorio o vía enlace externo (pendiente de publicación). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)

### 2. `Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb`

Notebook responsable de **unificar las fuentes de datos** y generar los splits estratificados para entrenamiento, validación y test. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)

Hace:

- Carga de reseñas en español (`train.csv`) y portugués (`reviews_consolidado_perfecto_v2.csv`). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)
- Normalización de columnas: `stars`, `review_title`, `review_body`, `language`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)
- Construcción de `text_raw` y mapping `stars → sentiment` (`Negativo`, `Neutro`, `Positivo`). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)
- Generación de un estrato `language||sentiment` y split 80/10/10 con `train_test_split` estratificado. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)  
- Exportación de:  
  - `train_es_pt.csv`  
  - `validation_es_pt.csv`  
  - `test_es_pt.csv`  

> 🔗 Estos archivos se asumen **presentes** para ejecutar `Proyecto_v12.ipynb`; la ubicación o link definitivo se documentará en el repositorio de datos una vez estén disponibles. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)

***

## 🏗️ Arquitectura General

La solución completa combina el pipeline de ML en Python con un backend Java y persistencia en PostgreSQL. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)

```text
┌─────────────────────────────────────────────────────────────────────┐
│                    SentimentAPI v4.0 (ES + PT)                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  📊 Data Science (Python)            🐿 Backend (Spring Boot)       │
│  ─────────────────────────            ─────────────────────────      │
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

La versión v4 incorpora un backend en Spring Boot con foco en seguridad, persistencia y exposición de servicios para el Frontend. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

### 1. Modelado de Base de Datos y Persistencia

- Migración a **PostgreSQL** con la base de datos `hackathonone` usando **Spring Data JPA**. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- Entidades core:  
  - `User` y `Rol` para gestión de autenticación/autorización.  
  - `Interaccion` para almacenar el historial de análisis de sentimientos. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- Seguridad: uso de **BCrypt** para almacenar contraseñas hash en lugar de texto plano. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

### 2. Endpoints y Lógica de Negocio

- Nuevos servicios bajo el contexto:  
  - `/project/api/v2/usuario` para **registro** y **login**. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- Procesamiento de sentimientos:  
  - El backend consume la API de Python mediante `WebClient`, obteniendo **etiqueta de sentimiento** y **score de confianza**. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- Respuesta JSON estructurada que incluye:  
  - Texto analizado.  
  - Sentimiento predicho (Negativo / Neutro / Positivo).  
  - Probabilidad / score de confianza.  
  - Metadatos de la interacción (usuario, timestamp, etc.). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

### 3. Conectividad y Visualización (Back–Front)

- El backend se centra en entregar **data cruda** lista para explotar. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- El Frontend es responsable de:  
  - Consumir el JSON retornado por la API.  
  - Agregar resultados (por ejemplo, conteo de sentimientos por tipo, tendencias).  
  - Renderizar métricas visuales usando librerías como **Chart.js** o **Recharts**. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

### 4. Configuración del Servidor

- Backend ejecutándose en el **puerto 8080**, con validaciones mediante **Jakarta Validation**. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)
- Más detalles de implementación pueden revisarse en el repositorio de backend:  
  👉 [Repositorio Backend – SentimentAPI](https://github.com/Jona-9/SentimentAPI) [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

***

## 🚀 Flujo de Ejecución (alto nivel)

1. **Consolidar dataset multilingüe**  
   - Ejecutar `Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb` para generar `train_es_pt.csv`, `validation_es_pt.csv` y `test_es_pt.csv` (o descargarlos desde el repositorio de datos cuando estén publicados). [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/4641cbbe-abb7-444e-a3c8-719a2efc437c/Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb)

2. **Entrenar y calibrar el modelo**  
   - Ejecutar `Proyecto_v12.ipynb` para entrenar el modelo, realizar tuning/calibración y generar el artefacto `.joblib` con su metadata y threshold. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/19abc82f-4085-4fec-ad1c-4b122bd4c268/Proyecto_v12.ipynb)

3. **Integrar con Backend**  
   - Configurar la API Python para exponer el endpoint de inferencia.  
   - Levantar el backend Spring Boot, configurar PostgreSQL y revisar los endpoints `/project/api/v2/usuario` y los dedicados a análisis de sentimientos. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

4. **Consumir desde Frontend**  
   - Construir un cliente que consuma el JSON del backend y renderice dashboards de sentimiento, métricas de confianza, etc. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/103844043/e57fa6bd-8882-4bc7-a5bf-71b235eef056/Linea-de-tiempo-del-MVP-de-Data-Science-v1.docx)

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

Si quieres, en el siguiente mensaje se puede añadir una tabla de métricas (cuando tengas los resultados finales del test set de v4) y una sección de “Limitaciones y próximos pasos” similar a la del `README_ejemplo` para cerrar el documento con roadmap.
