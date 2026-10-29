# Proyecto Final - Predicción de Churn en Telecomunicaciones

Proyecto desarrollado para la Diplomatura en Ciencia de Datos y Análisis Avanzado - Unidad N.º 9, Proyecto Final Integrador.

## Objetivo

Estimar la probabilidad de abandono (churn) de clientes de una empresa de telecomunicaciones y generar un ranking de riesgo que permita priorizar acciones de Retención, CRM y Marketing.

## Archivos necesarios

- `Proyecto_Final_Churn.ipynb`: notebook principal.
- `Telco-Customer-Churn.csv`: dataset utilizado por el notebook.

Ambos archivos deben ubicarse en la misma carpeta para ejecutar el proyecto sin modificar la ruta de carga.

## Ejecución local

1. Crear y activar un entorno virtual de Python.
2. Instalar las dependencias incluidas en `requirements.txt`.
3. Colocar `Proyecto_Final_Churn.ipynb` y `Telco-Customer-Churn.csv` en la misma carpeta.
4. Iniciar Jupyter Notebook o JupyterLab.
5. Abrir `Proyecto_Final_Churn.ipynb` y ejecutar las celdas en orden desde el comienzo.

Ejemplo de instalación:

```bash
python -m venv .venv
```

En Windows:

```bash
.venv\Scripts\activate
```

En macOS/Linux:

```bash
source .venv/bin/activate
```

Luego:

```bash
pip install -r requirements.txt
jupyter notebook
```

## Dependencias principales

El notebook utiliza pandas, NumPy, scikit-learn, XGBoost, LightGBM, SHAP, Matplotlib y Seaborn. La ejecución registrada en el notebook informa pandas 3.1.0, scikit-learn 1.9.1 y XGBoost 3.4.1.

## Metodología

El desarrollo sigue CRISP-DM:

1. Comprensión del negocio.
2. Comprensión de los datos.
3. Preparación de los datos.
4. Modelado.
5. Evaluación.
6. Despliegue propuesto como paso futuro.

## Modelos evaluados

- DummyClassifier (baseline).
- Regresión Logística estándar.
- Regresión Logística balanceada.
- Árbol de Decisión balanceado.
- Random Forest balanceado con feature engineering.
- XGBoost balanceado con feature engineering.

## Protocolo de selección

La comparación entre modelos se hace **solo con validación cruzada estratificada de 5 folds sobre el entrenamiento** (Recall, Precision, F1, ROC-AUC, PR-AUC y Recall/Precision en el Top 20%, también con probabilidades out-of-fold de `cross_val_predict`).

Criterio fijado de antemano: se elige el modelo de mayor PR-AUC en CV; si otro queda a menos de un desvío estándar, se consideran empatados y se elige el más simple (Regresión Logística < Árbol < Random Forest < XGBoost). Entre variantes igual de simples se prefiere la de mayor Recall.

El modelo elegido se evalúa **una sola vez** en el conjunto de prueba.

> Nota de transparencia: en la versión anterior el modelo (Random Forest) se había elegido mirando el test. En esta versión se corrigió el protocolo; el test ya había sido consultado antes, por lo que su resultado puede ser algo optimista.

## Modelo seleccionado

**Regresión Logística balanceada.** XGBoost (PR-AUC 0,669 ± 0,021), Random Forest (0,664) y las dos regresiones (0,661 y 0,660) quedaron empatados dentro de un desvío; se eligió la más simple y de mayor Recall en CV (0,801).

Resultados en el test (evaluación única):

- Recall: 0,783
- Precision: 0,504
- F1-score: 0,614
- ROC-AUC: 0,842
- PR-AUC: 0,633

En el Top 20% de mayor riesgo concentra 186 de 374 bajas reales (49,73%), con Precision @ Top 20% de 65,96% y Lift de 2,48x (en CV: 50,2%).

El notebook incluye además un anexo de negocio con facturación en riesgo por segmento (facturación × antigüedad) y escenarios hipotéticos de costo de contacto contra valor retenido.

## Alcance

El modelo identifica y prioriza clientes con mayor riesgo de churn. No demuestra por sí solo que una campaña de retención reduzca el abandono. El efecto de una intervención comercial debe validarse con un experimento controlado, por ejemplo mediante grupos de tratamiento y control.
