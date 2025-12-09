#  Análisis y Predicción de Precios de Coches Usados (R)

Este repositorio contiene un análisis exhaustivo (**EDA**) y la ingeniería de características (**Feature Engineering**) realizada sobre un dataset de vehículos de segunda mano del mercado indio. El proyecto utiliza el ecosistema **R (tidyverse)** para transformar datos crudos y sucios en un conjunto de datos optimizado y estable, listo para entrenar modelos de Machine Learning (Regresión Lineal).

##  Contenido del Repositorio

| Archivo | Descripción |
| :--- | :--- |
| **`Untitled.ipynb`** | Notebook en R con el código completo (Limpieza, EDA, Visualización y Feature Engineering). |
| **`used_cars_data.csv`** | Dataset original utilizado en el análisis. |

---

##  Sobre el Dataset

El conjunto de datos contiene información sobre miles de coches usados listados para la venta.
**Variable Objetivo:** `Price` (Precio en Lakhs - moneda india).

### Diccionario de Datos Original:
* **Name:** Marca y modelo del vehículo.
* **Location:** Ciudad donde se vende el coche.
* **Year:** Año de fabricación.
* **Kilometers_Driven:** Distancia recorrida en km.
* **Fuel_Type:** Tipo de combustible (Petrol, Diesel, CNG, LPG, Electric).
* **Transmission:** Manual o Automático.
* **Owner_Type:** Tipo de propiedad (Primer dueño, Segundo, etc.).
* **Mileage:** Consumo de combustible (kmpl o km/kg).
* **Engine:** Cilindrada del motor (CC).
* **Power:** Potencia del motor (bhp).
* **Seats:** Número de asientos.
* **Price:** Precio de venta (Variable a predecir).

---

##  1. Limpieza de Datos (Data Cleaning)

Se realizó un preprocesamiento riguroso para convertir los datos brutos en información utilizable.

### Acciones principales:
1.  **Conversión de Unidades:** Se utilizó `stringr` para limpiar y transformar las columnas `Mileage`, `Engine` y `Power` (ej: pasar de "58.16 bhp" a `58.16` numérico).
2.  **Tratamiento de Valores Nulos:**
    * Se eliminó la columna `New_Price` debido a un exceso de valores faltantes (>80%).
    * Se eliminaron filas que no tenían la variable objetivo `Price`.
3.  **Creación de Identificador Único:** Se generó una columna `ID` consecutiva para mantener la trazabilidad.

---

##  2. Análisis Exploratorio (EDA) y Visualización

A continuación, se presentan las **5 gráficas más representativas** del análisis, que justifican las decisiones tomadas para el modelado.

### Gráfica 1: Distribución del Precio (Escala Logarítmica)
<img width="840" height="840" alt="image" src="https://github.com/user-attachments/assets/b1dcb6c2-8d84-49c2-8264-e67b2c62f0ec" />

> **Análisis:** La distribución original del precio es altamente sesgada a la derecha (muchos coches baratos, pocos de lujo). Al aplicar una escala logarítmica, la distribución se normaliza, lo que justifica matemáticamente la creación de la variable `Log_Price` para mejorar la predicción del modelo.

### Gráfica 2: Potencia vs. Precio (por Tipo de Transmisión)
<img width="840" height="840" alt="image" src="https://github.com/user-attachments/assets/09acbce0-3acb-4ca0-a6af-26ac8ddd3290" />

> **Análisis:** Existe una correlación positiva clara. Los coches **automáticos** (puntos rojos) dominan los segmentos de alta potencia y precio alto, mientras que los **manuales** (puntos azules) se concentran en gamas medias y bajas. La transmisión actúa como un separador natural de segmentos de mercado.

### Gráfica 3: Depreciación por Año
<img width="840" height="840" alt="image" src="https://github.com/user-attachments/assets/0dc57a54-b92e-4083-a6ee-544d52ee9231" />

> **Análisis:** Se confirma una depreciación acelerada y no lineal. Los vehículos anteriores a 2010 pierden valor de forma drástica, mientras que los modelos recientes (2015-2019) sostienen precios mucho más altos. Esto valida el uso de la variable `Year` (transformada a antigüedad) como predictor clave.

### Gráfica 4: Distribución del Mercado (Combustible)
<img width="840" height="840" alt="image" src="https://github.com/user-attachments/assets/02dc4d90-b952-4cff-9104-dd2897f099bd" />
> **Análisis:** El mercado está prácticamente dominado por **Diesel** y **Petrol**. Las alternativas como CNG, LPG o Eléctricos representan un porcentaje marginal, por lo que se agruparon en una categoría "Otros" para reducir ruido en el análisis.

---

##  3. Ingeniería de Características (Feature Engineering)

A partir de los insights del EDA, se generaron nuevas variables para mejorar el rendimiento del modelo.

| Variable Nueva | Origen | Función / Descripción |
| :--- | :--- | :--- |
| **`Log_Price`** | `log(Price)` | Normaliza la variable objetivo para estabilizar la regresión y reducir el error en valores atípicos. |
| **`Car_Age`** | `2025 - Year` | Convierte el año en **antigüedad**, un predictor más directo y lógico de la depreciación. |
| **`Transmission`** | `Transmission` | Reconvertida a **Binaria**: `0` = Manual, `1` = Automático (One-Hot Encoding simplificado). |
| **`Brand`** | `Name` | Se extrajo la marca principal (ej. "Maruti") del nombre completo del vehículo. |

*Nota: Las columnas originales `Name` y `Year` se conservaron en el dataset final solo para referencia humana.*

---

## 🔗 4. Matriz de Correlación Estratégica (Gráfica 5)

Se evaluó la correlación entre las variables numéricas finales para detectar multicolinealidad.

<img width="840" height="840" alt="image" src="https://github.com/user-attachments/assets/2afc7e65-b972-436b-ab90-139c0373f4c0" />

### Hallazgos clave:
* **Impulsores del Precio:**
    * `Power`: Correlación muy alta y positiva (**≈ 0.80**).
    * `Engine`: Correlación alta (**≈ 0.65**).
* **Factor de Depreciación:**
    * `Car_Age`: Correlación fuerte negativa (**≈ -0.53**).
* ** Alerta de Multicolinealidad:**
    * **Power vs. Engine (0.91):** Existe una redundancia extrema entre la potencia y la cilindrada del motor.
    * **Decisión:** Para evitar inestabilidad en el modelo de regresión, se recomienda eliminar la variable `Engine` y conservar `Power` (que tiene mayor correlación con el precio).

---

##  Estado Final del Proyecto

El dataset resultante **`df_ai_ready`** contiene:
1.  Variables limpias y numéricas.
2.  Transformaciones logarítmicas aplicadas.
3.  Nuevas características creadas (`Brand`, `Car_Age`, `Transmission` binaria).

Los datos están listos para la selección definitiva de columnas y para el entrenamiento del modelo de aprendizaje automático.
