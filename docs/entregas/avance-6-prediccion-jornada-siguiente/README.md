# Avance 6 — Predicción de tiempos de viaje para la jornada siguiente

**Proyecto Integrador | Equipo 42 — Route Intelligence**  
**Maestría en Inteligencia Artificial Aplicada | Tecnológico de Monterrey**  
**Noviembre de 2026**

**Integrantes:** Luis Ángel Herrera Flores, Francisco Natanael Ortiz Martínez y Edgar Iván Rosas Gómez.

## 1. Objetivo

Desarrollar y evaluar modelos para pronosticar los tiempos de desplazamiento de una red vial experimental durante la jornada siguiente, utilizando únicamente información disponible antes de la emisión del pronóstico.

Este trabajo complementa los Avances 4 y 5, dedicados a reconstruir las condiciones contemporáneas de la red mediante un número reducido de consultas origen-destino (OD) simuladas. En este avance se estudia la **predicción anticipada**, sin utilizar observaciones contemporáneas de los instantes pronosticados.

## 2. Archivo principal

- [`Avance6_Equipo42.ipynb`](./Avance6_Equipo42.ipynb): libreta técnica con la metodología, implementación, resultados, visualizaciones, discusión y exportación experimental de matrices OD.

La libreta conserva las salidas de ejecución para facilitar la revisión sin necesidad de ejecutar nuevamente los modelos.

## 3. Diseño experimental

Se utiliza una red dirigida derivada de PEMS-BAY (2017), compuesta por **322 nodos y 345 aristas**.

El pronóstico se emite de manera simulada a las **18:00 de la jornada anterior**, utilizando observaciones disponibles hasta las 17:55. Para cada jornada se estiman los tiempos de las 345 aristas en **96 instantes de cinco minutos**, desde las 08:00 hasta las 15:55.

Se comparan cinco modelos:

1. **M0:** mediana histórica por arista, día de la semana e intervalo horario.
2. **M1:** regresión lineal Ridge sobre desviaciones logarítmicas respecto de la mediana.
3. **MLP:** red neuronal sin propagación espacial.
4. **GNN completa:** red neuronal sobre grafos con intercambio de información entre aristas consecutivas.
5. **GNN sin mensajes por macronodos:** variante que excluye determinadas relaciones de propagación espacial.

Los modelos se ajustan con datos de enero a marzo de 2017. Se utiliza abril para validación y selección de épocas de entrenamiento, mientras que mayo y junio se presentan como diagnósticos adicionales, no como evaluaciones ciegas independientes.

## 4. Resultados principales

La comparación muestra que una arquitectura más compleja no garantiza mayor precisión predictiva.

En abril, la mediana histórica obtiene el menor error promedio por arista, mientras que la GNN completa registra el menor error en las dos instancias OD evaluadas. En mayo y junio, M1 obtiene el menor MAE tanto por arista como en los tiempos OD calculados para 13 macronodos y 20 sensores.

La incorporación de propagación espacial mediante la GNN implementada no muestra ventajas consistentes respecto de M1. Además, los modelos presentan dificultades para anticipar aumentos pronunciados en los tiempos de desplazamiento.

La libreta también genera experimentalmente **96 matrices OD de 13 × 13** para el 1 de julio de 2017, utilizando M0, M1 y la GNN completa. Estas matrices se exportan en archivos CSV como posible insumo para una integración posterior con el optimizador de rutas.

## 5. Datos y reproducción

Se requiere el directorio `routing_dataset_v1`, utilizado también en los avances anteriores. Debe contener, al menos:

- `nodes.csv`
- `edges.csv`
- `edge_travel_times.parquet`
- `manifest.json`

La libreta busca este directorio junto al archivo `.ipynb`, en su carpeta superior o en la ruta de Google Drive `/content/drive/MyDrive/Route Intelligence/routing_dataset_v1`.

Las principales dependencias de Python son **NumPy, pandas, SciPy, Matplotlib y PyTorch**. Para leer Parquet se utiliza un motor compatible o el lector auxiliar incluido en el conjunto de datos.

Para reproducir el experimento:

1. Abrir `Avance6_Equipo42.ipynb` en Jupyter o Google Colab.
2. Verificar la disponibilidad de `routing_dataset_v1`.
3. Instalar las dependencias necesarias.
4. Ejecutar las celdas secuencialmente.

Los parámetros de las redes neuronales se almacenan como `pesos_no_spatial.pt`, `pesos_full.pt` y `pesos_no_macro.pt` en `resultados/02_gnn_intercorredor/`. Si están disponibles, se reutilizan; de lo contrario, la libreta ejecuta el entrenamiento.

Los resultados, las figuras y los archivos CSV se generan automáticamente en `resultados/02_gnn_intercorredor/`. La exportación experimental del 1 de julio se almacena en el subdirectorio `exportacion_od_experimental/`.

## 6. Alcance y limitaciones

Los tiempos de desplazamiento por arista se derivan de velocidades históricas y distancias estimadas. Las matrices OD se calculan mediante Dijkstra sobre una red experimental; no representan mediciones independientes de viajes completos.

La exportación de matrices constituye una demostración técnica. **Todavía no se ha validado su integración con el optimizador de Berth y Trigos ni se ha evaluado su efecto sobre la recuperación de rutas ante interrupciones.**

La libreta documenta los supuestos, las limitaciones, las fuentes académicas y la declaración de apoyo con inteligencia artificial.
