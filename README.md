# RandomForest4-sensibilidad-farmacologica
**Introducción**

La predicción de la sensibilidad de fármacos en células cancerígenas es un área de gran interés en la investigación farmacológica, pues permite personalizar tratamientos y mejorar los resultados terapéuticos en pacientes con cáncer. En este contexto, los modelos de Machine Learning han demostrado ser herramientas poderosas para predecir cómo diferentes células cancerígenas responderán a diversos fármacos, basándose en datos genómicos y moleculares. 

Esta investigación se enfoca en el uso de **Random Forest**, un modelo de Machine Learning basado en árboles de decisión, para predecir la sensibilidad de varios fármacos en células cancerosas. A través del uso de datos proporcionados por el proyecto **Genomics of Drug Sensitivity in Cancer (GDSC)**, se busca entrenar un modelo que sea capaz de estimar con precisión la **sensibilidad a fármacos** en función de características genómicas de las células. En particular, la variable objetivo de este proyecto es el **LN_IC50**, que indica la concentración de un fármaco necesaria para inhibir la viabilidad celular al 50%.

A lo largo de este proyecto, se compararán los resultados del modelo de **Random Forest** con los de otros enfoques, como el **Árbol de Decisión**, para determinar cuál proporciona un mejor desempeño en la predicción de la sensibilidad de los fármacos en cáncer. El objetivo final es identificar un modelo robusto y eficiente que pueda contribuir a la mejora de tratamientos farmacológicos dirigidos y personalizados para pacientes con cáncer.

Este análisis busca profundizar en la aplicabilidad de Random Forest y Desicion Tree en la predicción farmacológica, un área prometedora de la inteligencia artificial en biomedicina.

El data set utilizado proviene del proyecto **Genomics of drugs Sensitivity in cancer (GDSC)**, que contiene información sobre la respuesta de células cancerígenas a varios tratamientos. Este dataset esta distribuido bajo licencia GPL 3 y puedes descargarlo directamente de este link [Kaggle GDSC](https://www.kaggle.com/datasets/samiraalipour/genomics-of-drug-sensitivity-in-cancer-gdsc).
### Librerías utilizadas
- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn
## Cambios realizados a las columnas  
Como la mayoría de las columnas contienen variables categóricas, utilizamos `LabelEncoder` para codificarlas como enteros, asegurándonos primero de llenar los valores vacíos con la categoría `"desconocidos"`.  

## Matriz de correlación  
Después de codificar las variables, obtuvimos la siguiente matriz de correlación de las columnas.  

🔹 **Aclaración:** Las columnas con el sufijo `_encoder` eran originalmente categóricas.  
![Matriz de correlacion](/imagenes/Matriz.png)
Con esta matriz podemos inferir que las columnas 'AUC' y 'Z_SCORE' son las que mejor van a explicar nuestra variable objetivo 'LN_IC50'.

## Variables utilizadas:
Utilicé selectKbest para encontrar los mejores predictores para 'LN_IC50'. Las variables seleccionadas fueron:
<<<Características seleccionadas: ['AUC', 'Z_SCORE', 'GDSC Tissue descriptor 2_encoder', 'Growth Properties_encoder', 'TARGET_PATHWAY_encoder']>>>

## Entrenamiento del modelo:
Para entrenar el modelo, primero se realizó una validación cruzada de 10 pliegues (folds), obteniendo una puntuación media de 0.76.
Luego, se realizó una búsqueda de cuadrícula con GridSearchCV para encontrar los mejores hiperparámetros para Random Forest, y el resultado fue el siguiente:

Mejores hiperparámetros:
{'max_depth': 15, 'min_samples_leaf': 2, 'min_samples_split': 7, 'n_estimators': 500}
RMSE: 1.6713
R2 Score: 0.7809

## Rendimiento del modelo:
Después de entrenar el modelo, procedimos a validar sus métricas con el dataset creado para el test.

Conteo de errores en la predicción
![Gráfico de barra](/imagenes/Distr.png)
Como podemos observar, tenemos un índice alto de aciertos, ya que la barra de error en 0.0 es la más alta.

Gráfico de dispersión con línea de tendencia
![Gráfico de disperción y línea](/imagenes/Pred.png)
Análisis del gráfico de predicciones vs valores reales
En este gráfico se comparan los valores reales con las predicciones del modelo RandomForestRegressor.

La línea roja representa la relación ideal donde predicción = valor real.
Los puntos de colores indican el error del modelo:
- Azul: el modelo subestimó el valor real.
- Rojo: el modelo sobreestimó el valor real.
- Blanco/Gris: errores pequeños, predicciones más precisas.
→ Se observa una buena alineación con la línea roja, aunque hay mayor dispersión en valores extremos.
Esto sugiere que el modelo tiene un buen desempeño, pero puede mejorar en la predicción de valores más alejados del centro de la distribución.