# 🎯 SentimentAPI v4.0 - Multilingüe ES+PT

## 📋 Descripción del Proyecto

**SentimentAPI v4.0** es un MVP de análisis de sentimientos **multilingüe (Español + Portugués)** que combina un pipeline de Machine Learning en Python (TF‑IDF + Regresión Logística calibrada) con un backend en Spring Boot y persistencia en PostgreSQL. [kaggle](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)
El sistema recibe textos libres (reseñas/comentarios), predice **Negativo / Neutro / Positivo** y expone los resultados vía API REST, dejando el cálculo de métricas y visualizaciones al Frontend. [kaggle](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)

### Clasificaciones disponibles

- 🟢 **Positivo** (estrellas 4–5)  
- 🟡 **Neutro** (estrella 3)  
- 🔴 **Negativo** (estrellas 1–2)

### Características principales

- ✅ Modelo **multilingüe ES+PT** en un único pipeline y artefacto `.joblib`.  
- ✅ Probabilidades **calibradas** y umbral dinámico para `review_required`.  
- ✅ Persistencia de interacciones en **PostgreSQL** (usuarios, roles e historial de análisis).  
- ✅ Backend Spring Boot con endpoints de **registro/login** y consumo de la API de Python vía `WebClient`.  

***

## 🕒 Línea de tiempo del MVP (v1 → v4)

La evolución del proyecto sigue tres fases de Data Science más la integración completa en v4.  

### 🔹 Fase 1 — Exploración y baseline (v1 · Español)

- Modelo inicial con **TF‑IDF (unigramas/bigramas) + Regresión Logística** sobre reseñas en español. [kaggle](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)
- Limpieza basada en regex, sin lematización ni pipelines pesados; foco en **factibilidad técnica** y bajo costo computacional. [kaggle](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)

### 🔹 Fase 2 — Robustez estadística (v2 · Español)

- Incorporación de **Naive Bayes** como baseline comparativo y **cross-validation estratificada** para medir estabilidad.  
- Introducción de la regla de **revisión humana por incertidumbre** (`review_required` cuando la probabilidad máxima cae por debajo de un threshold).  

### 🔹 Fase 3 — Modelo productivo multilingüe (v3 · ES+PT)

- Ampliación del dataset a **Español + Portugués** manteniendo un único modelo. [huggingface](https://huggingface.co/datasets/ruanchaves/b2w-reviews01)
- Uso de **RandomizedSearchCV** y **CalibratedClassifierCV** para obtener probabilidades confiables y selección automática de umbral.  

### 🔹 Fase 4 — SentimentAPI v4.0 (ES+PT + Backend)

- Consolidación del pipeline multilingüe en `Proyecto_v12.ipynb` y generación del bundle productivo.  
- Integración con backend Java, persistencia en PostgreSQL y preparación para explotación desde un Frontend de dashboards.  

***

## 📓 Notebooks Principales

### 1. `Proyecto_v12.ipynb` — Modelo Multilingüe ES+PT (Core DS)

Este notebook implementa el pipeline completo de ML para **entrenamiento, calibración y serialización** del modelo multilingüe. [kaggle](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)

Incluye:

- Carga de datos estratificados (`train_es_pt.csv`, `validation_es_pt.csv`, `test_es_pt.csv`).  
- Transformación de `stars` (1–5) a sentimiento ternario (Negativo / Neutro / Positivo).  
- Limpieza de texto **Unicode-aware** y construcción de `text_raw` / `text_clean`.  
- Pipeline:  
  - `TF-IDF` (n‑grams, límites de frecuencia, max_features).  
  - `LogisticRegression` + **CalibratedClassifierCV**.  
- Selección de **threshold óptimo** a partir de la relación coverage vs recall de la clase Negativo.  
- Evaluación global y por idioma (ES vs PT) y generación del artefacto `.joblib` con metadata (versión, hash, parámetros).  

> 📂 Nota: Los archivos `train_es_pt.csv`, `validation_es_pt.csv` y `test_es_pt.csv` se asumen presentes en el repositorio o accesibles vía enlace externo.  

### 2. `Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb`

Notebook responsable de **unificar las fuentes de datos** y generar los splits estratificados para entrenamiento, validación y test. [huggingface](https://huggingface.co/datasets/ruanchaves/b2w-reviews01)

Hace:

- Carga de reseñas en español (`train.csv`) y portugués (`reviews_consolidado_perfecto_v2.csv`).  
- Normalización de columnas: `stars`, `review_title`, `review_body`, `language`.  
- Construcción de `text_raw` y mapping `stars → sentiment` (`Negativo`, `Neutro`, `Positivo`).  
- Generación de un estrato `language||sentiment` y split 80/10/10 con `train_test_split` estratificado.  
- Exportación de:  
  - `train_es_pt.csv`  
  - `validation_es_pt.csv`  
  - `test_es_pt.csv`  

### 3. `Estructurar_datos_portugues.ipynb`

Este notebook **prepara y consolida los datos en portugués** a partir de múltiples fuentes externas. [github](https://github.com/luminati-io/eCommerce-dataset-samples)

- Usa reseñas en portugués de:  
  - [B2W-Reviews01 (Hugging Face)](https://huggingface.co/datasets/ruanchaves/b2w-reviews01). [huggingface](https://huggingface.co/datasets/ruanchaves/b2w-reviews01)
  - [Ecommerce product dataset – MercadoLivre BR](https://github.com/octaprice/ecommerce-product-dataset/tree/main/data/mercadolivre_com_br). [github](https://github.com/luminati-io/eCommerce-dataset-samples)
- Combina estos datos con reseñas en español de [Amazon Reviews Multi](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi). [kaggle](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)
- Genera el archivo consolidado multilingüe `reviews_consolidado_perfecto_v2.csv`, utilizado luego en el proceso de split estratificado.  

***

## 💻 Cómo ejecutar el modelo (entorno Data Science)

En **Visual Studio Code**, **Google Colab** o **Jupyter Notebook** puedes abrir y ejecutar `Proyecto_v12.ipynb`. [kaggle](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)

1. Asegúrate de tener en la carpeta de trabajo los archivos:

   - `train_es_pt.csv`  
   - `validation_es_pt.csv`  
   - `test_es_pt.csv`  

2. Crea y activa tu entorno (ejemplo con `venv`):

```bash
python -m venv .venv
source .venv/bin/activate   # En Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

3. Ejecuta todas las celdas de `Proyecto_v12.ipynb` para:

- Entrenar el modelo multilingüe ES+PT.  
- Calibrar probabilidades.  
- Generar el artefacto `.joblib` con metadata y threshold óptimo.  

***

## 🌍 Origen de los datos multilingües (ES + PT)

El dataset multilingüe ES+PT se construye en tres pasos: [github](https://github.com/luminati-io/eCommerce-dataset-samples)

1. **Datos en portugués**  
   - Reseñas tomadas desde:  
     - [B2W-Reviews01](https://huggingface.co/datasets/ruanchaves/b2w-reviews01). [huggingface](https://huggingface.co/datasets/ruanchaves/b2w-reviews01)
     - [Ecommerce product dataset – MercadoLivre BR](https://github.com/octaprice/ecommerce-product-dataset/tree/main/data/mercadolivre_com_br). [github](https://github.com/luminati-io/eCommerce-dataset-samples)
   - Procesados y estandarizados en `Estructurar_datos_portugues.ipynb`.  

2. **Datos en español**  
   - Reseñas en español del dataset [Amazon Reviews Multi](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi). [kaggle](https://www.kaggle.com/datasets/mexwell/amazon-reviews-multi)

3. **Consolidación y split estratificado**  
   - `Estructurar_datos_portugues.ipynb` genera `reviews_consolidado_perfecto_v2.csv` con reseñas en ambos idiomas.  
   - `Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb`:
     - Lee el consolidado y los archivos fuente.  
     - Crea el estrato `language||sentiment`.  
     - Realiza split 80/10/10 estratificado para **train/validation/test**.  
     - Exporta `train_es_pt.csv`, `validation_es_pt.csv`, `test_es_pt.csv`, usados luego en `Proyecto_v12.ipynb`.  

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

La versión v4 incorpora un backend en Spring Boot con foco en **seguridad**, **persistencia** y **exposición de servicios** para el Frontend. 

### 1. Modelado de Base de Datos y Persistencia

- Migración a **PostgreSQL** con la base de datos `hackathonone` usando **Spring Data JPA`.  
- Entidades core:  
  - `User` y `Rol` para autenticación/autorización.  
  - `Interaccion` para almacenar el historial de análisis de sentimientos.  
- Seguridad: uso de **BCrypt** para almacenar contraseñas hash en lugar de texto plano.  

### 2. Endpoints y Lógica de Negocio

- Servicios bajo el contexto:  
  - `/project/api/v2/usuario` para **registro** y **login**.  
- Procesamiento de sentimientos:  
  - El backend consume la API de Python mediante `WebClient`, obteniendo **etiqueta de sentimiento** y **score de confianza**.  
- Respuesta JSON estructurada que incluye:  
  - Texto analizado.  
  - Sentimiento predicho (Negativo / Neutro / Positivo).  
  - Probabilidad / score de confianza.  
  - Metadatos de la interacción (usuario, timestamp, etc.).  

### 3. Conectividad y Visualización (Back–Front)

- El backend entrega **data cruda** lista para explotar.  
- El Frontend (React) se encarga de:  
  - Consumir el JSON retornado por la API.  
  - Agregar resultados (conteo de sentimientos, tendencias).  
  - Renderizar métricas visuales usando librerías como **Chart.js** o **Recharts**.  

### 4. Configuración del Servidor

- Backend ejecutándose en el **puerto 8080**, con validaciones mediante **Jakarta Validation**.  
- Más detalles de implementación en:  
  👉 [Repositorio Backend – SentimentAPI](https://github.com/Jona-9/SentimentAPI) 

***

## 💻 Frontend – Dashboards de Sentimiento

El cliente web para visualización de métricas y resultados de SentimentAPI v4.0 se encuentra en el siguiente repositorio: 

- 👉 **Repositorio análisis de sentimientos – Rama Ale-dev**  
  https://github.com/Alexandracleto/sentimientos/tree/Ale-dev  

***

## 📦 Archivos grandes (> 25 MB)

Por límite de tamaño en GitHub, algunos artefactos y datasets se alojan externamente. 

- 👉 **Google Drive – Artefactos y datasets SentimentAPI v4.0**  
  https://drive.google.com/drive/folders/1BJm3XgbgJzD-CzCSt2f5LFeIvOrIqwyB?usp=drive_link  

***

## 🚀 Flujo de Ejecución (alto nivel)

1. **Consolidar dataset multilingüe**  
   - Ejecutar `Estructurar_datos_portugues.ipynb` y luego `Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb` para generar `train_es_pt.csv`, `validation_es_pt.csv` y `test_es_pt.csv`.  

2. **Entrenar y calibrar el modelo**  
   - Ejecutar `Proyecto_v12.ipynb` para entrenar el modelo, realizar tuning/calibración y generar el artefacto `.joblib` con su metadata y threshold.  

3. **Integrar con Backend**  
   - Configurar la API Python para exponer el endpoint de inferencia.  
   - Levantar el backend Spring Boot, configurar PostgreSQL y revisar los endpoints `/project/api/v2/usuario` y los dedicados a análisis de sentimientos.  

4. **Consumir desde Frontend**  
   - Construir un cliente (React u otro) que consuma el JSON del backend y renderice dashboards de sentimiento y métricas de confianza.  

***

## 📁 Estructura del repositorio DS

```text
📦 sentimentapi-v4-es-pt/
├── 📓 Proyecto_v12.ipynb
├── 📓 Estructurar_datos_portugues.ipynb
├── 📓 Consolidacion_de_dataset_multilingue_con_split_estratificado_train_validation_test.ipynb
├── 📊 train_es_pt.csv
├── 📊 validation_es_pt.csv
├── 📊 test_es_pt.csv
├── 📊 reviews_consolidado_perfecto_v2.csv
├── 📄 README.md
├── 📄 requirements.txt
├── 📄 Línea de tiempo del MVP de Data Science v1.docx
└── 📦 sentiment_bundle_es_pt_v2.joblib
```

***

## 👥 Equipo

Proyecto desarrollado por **“No Data - No Code”** en el marco del Hackatón **No Country**, dentro del programa **Oracle Next Education (ONE)** en alianza con **Alura Latam**.

***

## 📄 Licencia

Este proyecto está bajo licencia **MIT**.
