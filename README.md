# Análisis y Predicción de Precios de Coches Usados (R)

Este repositorio contiene un análisis exhaustivo (EDA) y la ingeniería de características (Feature Engineering) realizada sobre un dataset de vehículos de segunda mano. El proyecto utiliza R (tidyverse) para transformar datos crudos en un conjunto optimizado para Machine Learning.

# Contenido del Repositorio
Archivo	Descripción
Untitled.ipynb	Notebook en R con el código completo (Limpieza, EDA, Visualización y Feature Engineering).
used_cars_data.csv	Dataset original utilizado en el análisis.
# 1. Limpieza de Datos (Data Cleaning)

Se realizó un preprocesamiento riguroso para convertir los datos brutos en información utilizable.

Acciones principales

Conversión de Unidades
Se limpiaron y transformaron las columnas Mileage, Engine y Power (por ejemplo, de "58.16 bhp" a 58.16 numérico).

Tratamiento de Valores Nulos

Se eliminó la columna New_Price debido a su alta proporción de valores faltantes.

Se eliminaron las filas sin información de Price.

Creación de Identificador Único
Se generó una columna ID consecutiva después de la depuración.

# 2. Análisis Exploratorio (EDA) y Visualización

Se generaron cuatro gráficas clave para entender el comportamiento del mercado y las relaciones entre variables.

Gráfica 1: Distribución del Precio (Escala Logarítmica)

Se aplicó una escala logarítmica al histograma del precio.

Insight: La distribución original es altamente sesgada. La escala logarítmica proporciona una visión más equilibrada y justifica la creación de la variable Log_Price.

Gráfica 2: Potencia vs. Precio (por Tipo de Transmisión)

Gráfico de dispersión que relaciona Power (bhp) y Price, coloreado por transmisión.

Insight: Existe una correlación clara. Los automáticos dominan los segmentos de mayor potencia y precio; los manuales se concentran en gamas medias y bajas.

Gráfica 3: Depreciación por Año

Gráfico de línea del precio promedio según año de fabricación.

Insight: Se confirma una depreciación acelerada. Los vehículos anteriores a 2010 pierden valor de forma marcada.

Gráfica 4: Distribución del Tipo de Combustible

Gráfico circular agrupado en Diesel, Petrol y Otros.

Insight: El mercado está prácticamente dominado por Diesel y Petrol. Las alternativas (CNG, LPG, Electric) representan un porcentaje marginal.

# 3. Ingeniería de Características (Feature Engineering)

A partir del EDA, se generaron nuevas variables para mejorar el rendimiento del modelo.

| Variable      | Origen         | Función / Descripción                                            |
|---------------|----------------|------------------------------------------------------------------|
| Log_Price     | log(Price)     | Normaliza la variable objetivo para estabilizar la regresión.   |
| Car_Age       | 2025 - Year    | Convierte el año en antigüedad, predictor clave de depreciación.|
| Transmission  | Transmission   | Reconvertida a binaria: 0 = Manual, 1 = Automático.            |
| BrandName     | Name           | Se extrajo la marca principal del nombre del vehículo. 

Las columnas originales Name y Year se conservaron para referencia, aunque el modelo utilizará únicamente las variables transformadas.

# 4. Matriz de Correlación Estratégica (Gráfica 5)

Se evaluó la correlación entre las variables numéricas finales:
Log_Price, Power, Car_Age, Engine, Mileage y Transmission.

Hallazgos clave

Impulsores del Precio

Power: ≈ 0.80

Engine: ≈ 0.65

Factor de Depreciación más relevante

Car_Age: ≈ -0.53

Multicolinealidad Detectada

Power vs. Engine: 0.91

Conclusión: Existe una redundancia extrema entre ambas. Deberá eliminarse una (preferiblemente Engine) para evitar inestabilidad en el modelo.

Estado Final del Proyecto

El dataset resultante df_ai_ready contiene:

Variables limpias.

Transformaciones aplicadas.

Nuevas características creadas.

Está listo para la selección definitiva de columnas y para el entrenamiento del modelo de aprendizaje automático.
