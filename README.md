# Detección temprana de enfermedades cardiovasculares

Proyecto académico de ciencia de datos médica para detectar presencia o ausencia de enfermedad cardiovascular usando el **Heart Disease Dataset** del **UCI Machine Learning Repository**.

El flujo compara modelos interpretables y modelos más complejos, y finaliza con explicabilidad mediante **SHAP** para apoyar una lectura clínica responsable.

## Ruta rápida

Ejecutar los notebooks en este orden:

1. `notebooks/01_EDA.ipynb` — análisis exploratorio de datos.
2. `notebooks/02_Preprocessing_Heart_Disease.ipynb` — limpieza, codificación, escalamiento y generación de `heart_disease_processed.csv`.
3. `notebooks/03_Models_Heart_Disease.ipynb` — entrenamiento y comparación de modelos.
4. `notebooks/04_SHAP_Heart_Disease.ipynb` — explicabilidad global y local con SHAP.

## Dataset

| Aspecto | Detalle |
|---|---|
| Fuente | UCI Machine Learning Repository |
| Dataset | Heart Disease Dataset |
| ID UCI | `45` |
| Carga | Directamente con `ucimlrepo` |
| Pacientes | 303 |
| Problema | Clasificación binaria |
| Objetivo | `target`: 0 = ausencia, 1 = presencia de enfermedad cardiovascular |

## Metodología

### 1. EDA

Se revisan dimensiones, tipos de datos, valores faltantes, duplicados, distribución de la variable objetivo, histogramas, correlaciones y boxplots clínicos.

Hallazgos principales:

- Valores faltantes: 6, ubicados en `ca` y `thal`.
- Duplicados: 0.
- Prevalencia de enfermedad: aproximadamente 45.9%.
- Variables más correlacionadas con enfermedad: `thal`, `ca`, `exang`.

### 2. Preprocesamiento

Decisiones principales:

- Se elimina `num` para evitar **data leakage**.
- Se conserva `target` como variable objetivo binaria.
- `ca` se imputa con mediana.
- `thal` se imputa con moda.
- Variables categóricas se codifican con One-Hot Encoding.
- Variables numéricas se escalan con `StandardScaler`.

Resultado esperado:

- 25 variables predictoras numéricas.
- 0 valores faltantes.
- 0 variables no numéricas.

### 3. Modelado

Modelos evaluados:

- Logistic Regression
- Decision Tree Classifier
- Support Vector Machine
- Random Forest Classifier
- Gradient Boosting Classifier
- Neural Network con `MLPClassifier`

Métricas usadas:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC

También se aplica validación cruzada estratificada con 5 folds.

### 4. Explicabilidad con SHAP

Aunque SVM obtuvo el mejor ROC-AUC en test, se selecciona **Logistic Regression** para explicabilidad por su interpretabilidad y desempeño competitivo.

Esta decisión se alinea con principios de IA Responsable en salud: no alcanza con predecir bien; también hay que poder explicar la predicción.

## Archivos generados

Al ejecutar los notebooks pueden generarse estos archivos locales:

| Archivo | Descripción |
|---|---|
| `heart_disease_processed.csv` | Dataset final procesado |
| `model_results.csv` | Resultados de modelos en test |
| `cv_results.csv` | Resultados de validación cruzada |
| `best_heart_disease_model.pkl` | Mejor modelo serializado |
| `shap_feature_importance.csv` | Importancia promedio SHAP |
| `*.png` | Figuras exportadas de SHAP |

Estos archivos están ignorados por Git porque son artefactos reproducibles desde los notebooks.

## Reproducibilidad

Todos los notebooks usan:

```python
random_state = 42
```

La división de datos usa `stratify=y` para conservar la proporción de clases entre entrenamiento y prueba.

## Requisitos principales

Los notebooks instalan o importan sus dependencias dentro de Google Colab. Las librerías centrales son:

- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `scikit-learn`
- `ucimlrepo`
- `shap`
- `joblib`

## Estructura

```text
.
├── notebooks/
│   ├── 01_EDA.ipynb
│   ├── 02_Preprocessing_Heart_Disease.ipynb
│   ├── 03_Models_Heart_Disease.ipynb
│   └── 04_SHAP_Heart_Disease.ipynb
├── paper/
├── README.md
└── .gitignore
```

## Nota académica

Este proyecto es una base reproducible para un artículo universitario. Los resultados no deben interpretarse como herramienta clínica validada sin evaluación externa, revisión médica y validación prospectiva.
