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

## Modelo seleccionado

Random Forest balanceado con feature engineering.

Resultados en holdout:

- Recall: 0,805
- Precision: 0,535
- F1-score: 0,642
- ROC-AUC: 0,844
- PR-AUC: 0,659

En el Top 20% de mayor riesgo concentra 192 de 374 bajas reales (51,34%), con Precision @ Top 20% de 68,09% y Lift de 2,57x.

## Alcance

El modelo identifica y prioriza clientes con mayor riesgo de churn. No demuestra por sí solo que una campaña de retención reduzca el abandono. El efecto de una intervención comercial debe validarse con un experimento controlado, por ejemplo mediante grupos de tratamiento y control.
