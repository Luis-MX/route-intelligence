# Planeación de rutas con interrupciones

**Proyecto Integrador | Maestría en Inteligencia Artificial Aplicada | Tecnológico de Monterrey**

**Equipo 42**

- Luis Ángel Herrera Flores
- Francisco Natanael Ortiz Martínez
- Edgar Iván Rosas Gómez

**Asesor y sponsor:** Dr. Gerardo Jesús Camacho González

## Descripción del proyecto

La planeación y recuperación de rutas de distribución requiere estimar los tiempos de traslado entre ubicaciones. Cuando las condiciones del tráfico cambian, los tiempos previstos pueden dejar de representar adecuadamente las condiciones de circulación, afectando los horarios de llegada y las decisiones de ajuste de rutas.

El proyecto se planteó inicialmente en el contexto de la recuperación de rutas ante interrupciones, combinando técnicas de inteligencia artificial y optimización. Durante su desarrollo, el trabajo se concentró en un componente previo de ese problema: **la estimación y el pronóstico de tiempos de viaje que pueden utilizarse como información de entrada para algoritmos de planeación y recuperación de rutas**.

Los resultados se documentan en el trabajo académico *Estimación y pronóstico de tiempos de viaje en redes dirigidas*.

## Solución desarrollada

Se desarrollaron y evaluaron dos procedimientos complementarios:

- **M1 — Pronóstico anticipado:** estima los tiempos de traslado de la jornada siguiente utilizando información disponible hasta las 18:00 del día anterior.
- **K=32 — Reconstrucción contemporánea:** estima los tiempos correspondientes a un instante determinado, combinando información histórica con 32 consultas de tiempos de viaje entre ubicaciones, simuladas en el entorno experimental.

Ambos procedimientos utilizan modelos de regresión Ridge para estimar tiempos de desplazamiento por conexión vial. Posteriormente, se calculan matrices de tiempos de viaje origen-destino (OD) mediante caminos de menor duración en una red dirigida.

El entorno experimental se construyó a partir de datos históricos de velocidad del conjunto PEMS-BAY, correspondientes a 2017. La red utilizada comprende **322 nodos y 345 aristas dirigidas**.

## Principales resultados

En las evaluaciones realizadas con datos de junio de 2017, ambos procedimientos mejoraron la precisión de las estimaciones frente a sus respectivas referencias históricas.

| Procedimiento | Error promedio de referencia | Error promedio del modelo | Reducción |
|---|---:|---:|---:|
| M1 — Pronóstico anticipado | 38.15 s | 32.67 s | **14.36 %** |
| K=32 — Reconstrucción contemporánea | 31.75 s | 11.42 s | **64.04 %** |

Las cifras corresponden al error absoluto medio (MAE) de los tiempos de viaje OD. Los procedimientos se evaluaron con diferentes conjuntos de ubicaciones, condiciones de información y protocolos, por lo que sus resultados no representan una comparación directa entre ambos.

Además de los modelos evaluados, se generaron matrices OD exportables, catálogos de nodos y conexiones, y herramientas computacionales para consultar y reproducir resultados.

## Organización del repositorio

Los materiales del proyecto se encuentran organizados cronológicamente en [`docs/entregas/`](docs/entregas/), con una carpeta para cada avance académico.

Los documentos principales de difusión y cierre son:

- [Artículo académico](docs/entregas/avance-7-proyecto-de-difusion/articulo_academico.pdf): metodología, experimentos, evaluación y conclusiones.
- [Resumen ejecutivo](docs/entregas/avance-7-proyecto-de-difusion/resumen_ejecutivo.pdf): síntesis de resultados y consideraciones para una posible aplicación.
- [Póster del Proyecto Integrador](docs/entregas/avance-8-poster-y-cierre/poster.pdf): presentación visual del problema, los procedimientos desarrollados y los principales resultados.

Los avances anteriores contienen los informes, cuadernos de experimentación y materiales técnicos que respaldan el desarrollo del proyecto.

## Alcance y limitaciones

El trabajo tiene un carácter académico y experimental. Los tiempos de referencia se calcularon a partir de velocidades históricas de sensores y longitudes de conexiones de la red, algunas de ellas aproximadas; no corresponden a mediciones independientes de viajes completos.

Asimismo, las consultas contemporáneas utilizadas por K=32 fueron simuladas a partir de los propios datos experimentales.

Aunque los procedimientos desarrollados mejoraron la precisión de las estimaciones, **todavía no se ha evaluado su efecto sobre la calidad, factibilidad o costo de las rutas generadas por un algoritmo de optimización**. Tampoco se ha implementado la solución en una operación logística real.

## Trabajo futuro

La continuación principal consiste en integrar las matrices de tiempos de viaje en un algoritmo de optimización de rutas con ventanas de atención y evaluar su efecto sobre la planeación y recuperación de recorridos.

También será necesario examinar el comportamiento de los procedimientos con información externa, consultas sujetas a errores de medición y condiciones de circulación atípicas.

## Referencias principales

- Berth, M. O., & Trigos, F. (2025). *A Transdisciplinary Framework for Handling Disruptions in Vehicle Routing with Customer Time Windows*. En *Transdisciplinarity for a Better World*, 201–210. https://doi.org/10.3233/ATDE251092
- Li, Y., Yu, R., Shahabi, C., & Liu, Y. (2018). *Diffusion Convolutional Recurrent Neural Network: Data-Driven Traffic Forecasting*. International Conference on Learning Representations. https://openreview.net/forum?id=SJiHXGWAZ
