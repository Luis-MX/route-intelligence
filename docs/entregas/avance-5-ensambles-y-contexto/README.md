# Avance 5 — Ensambles y fuentes contextuales para la estimación de tiempos de viaje

**Proyecto Integrador · Route Intelligence · Equipo 42**  
**Maestría en Inteligencia Artificial Aplicada · Tecnológico de Monterrey**

## Propósito de esta entrega

Este avance reúne tres exploraciones complementarias que extienden el trabajo del [Avance 4](../avance-4-modelos-alternativos/Avance4_Equipo42.ipynb). El problema central sigue siendo **reconstruir los tiempos de viaje relevantes de una red vial utilizando un número reducido de observaciones actuales**, con miras a apoyar la replaneación de rutas ante interrupciones.

Se desarrollaron dos líneas de trabajo en paralelo:

1. **Reconstrucción de tiempos mediante ensambles:** comprobar si combinar modelos mejora la estimación de los tiempos de las aristas y de los trayectos origen–destino (OD), frente a los modelos individuales ya evaluados.
2. **Incorporación potencial de información contextual:** explorar registros de cierres programados de Caltrans y su correspondencia espacial y temporal con la red de PEMS-BAY, como preparación para futuros modelos que utilicen información de eventos.

Las tres libretas **son contribuciones separadas**. Sus resultados aún no constituyen un sistema integrado de predicción de tráfico ni una evaluación de replaneación de rutas.

## Contenido

| Libreta | Pregunta abordada | Aportación principal |
|---|---|---|
| [`Avance5_Ensambles_Heterogeneos_Equipo42_documentado.ipynb`](Avance5_Ensambles_Heterogeneos_Equipo42_documentado.ipynb) | ¿Es conveniente combinar modelos de distintas familias? | Evalúa *blending* de Ridge, KNN y MLP, y una primera variante de *stacking*; analiza los errores OD y el riesgo de subestimación. |
| [`Avance5_ensambles_homogeneos.ipynb`](Avance5_ensambles_homogeneos.ipynb) | ¿Ayuda combinar modelos de una misma familia? | Compara bagging de árboles, Random Forest, Extra Trees y bagging de Ridge con referencias individuales; examina la estabilidad de las diferencias. |
| [`LCS_D04_2017_Equipo42_Avance5-2 (1).ipynb`](LCS_D04_2017_Equipo42_Avance5-2%20%281%29.ipynb) | ¿Qué información sobre cierres programados puede asociarse con la red? | Analiza solicitudes del *Lane Closure System* (LCS) de Caltrans de enero a junio de 2017 y las relaciona con aristas de corredor mediante carreteras, sentidos y postmillas. |

## Enfoque experimental y resultados principales

### A. Ensambles para reconstrucción de tiempos

Las dos exploraciones de ensambles parten del escenario de **32 consultas guía simuladas**, que permiten inferir los tiempos de **345 aristas** de la red experimental derivada de PEMS-BAY. A partir de los tiempos reconstruidos, se calculan trayectos OD con Dijkstra. Se utiliza abril de 2017 para explorar o seleccionar configuraciones y mayo para la comparación temporal posterior.

En el escenario de mayo reportado en las libretas:

| Método | MAE de tiempos OD |
|---|---:|
| Ridge individual (`alpha=1000`) | 11.5116 s |
| Blending heterogéneo (75 % Ridge + 25 % KNN) | **11.2710 s** |
| Bagging de Ridge | 11.4853 s |

El *blending* Ridge–KNN reduce el MAE OD en aproximadamente **2.09 %** frente a Ridge. La mejora es pequeña, y el análisis exploratorio no demuestra que sea estadísticamente concluyente. Además, el *blending* subestima el tiempo de un mayor porcentaje de trayectos que Ridge. En el caso homogéneo, la diferencia favorable a bagging de Ridge es aún menor y parte se reproduce aumentando la regularización del modelo individual.

**Interpretación:** los ensambles amplían la comparación metodológica, pero **Ridge continúa siendo una referencia competitiva**. No se ha demostrado todavía una mejora operativa en cumplimiento de ventanas de tiempo, costos o factibilidad de las rutas.

### B. Contexto de cierres programados de Caltrans

La tercera libreta examina **27,448 solicitudes administrativas LCS** del Distrito 4 entre enero y junio de 2017. Encuentra **1,806 solicitudes** con alguna asociación espacial con **242 de las 273 aristas de corredor** del grafo. La vinculación se basa en referencias viales y postmillas; no confirma la ejecución de los cierres.

Esta exploración caracteriza horarios, duraciones, estados administrativos y presencia programada sobre la red. Su aportación es preparar una fuente contextual potencialmente aprovechable en modelos posteriores; **no demuestra todavía un efecto sobre las velocidades observadas ni una mejora predictiva**.

## Lectura y reproducción

Las libretas contienen explicaciones, gráficos y salidas guardadas, por lo que **pueden revisarse en GitHub sin reejecutarlas**. Para una reproducción completa se necesitan archivos de datos y, en algunos casos, rutas o dependencias de versiones anteriores:

- **Ensambles heterogéneos:** requiere `routing_dataset_v1` (`nodes.csv`, `edges.csv`, `edge_travel_times.parquet`) y procedimientos recuperados desde una libreta del Avance 4 denominada `Avance4_42.ipynb`, buscada en una ruta específica de Google Drive. La versión a la que apunta no coincide necesariamente con el archivo oficial `Avance4_Equipo42.ipynb`; antes de ejecutar desde cero, debe revisarse esa dependencia.
- **Ensambles homogéneos:** trabaja con `routing_dataset_v1`, situado junto al notebook o indicado mediante `ROUTE_INTELLIGENCE_DATASET_DIR`. No necesita ejecutar otras libretas ni montar Google Drive. Los archivos de datos y versiones del entorno se gestionan por separado.
- **Exploración LCS:** requiere `LCS_D04_2017_enero_marzo.txt`, `LCS_D04_2017_abril_junio.txt`, `d04_text_meta_2017_01_04.txt` y `edges.csv`. Busca estos archivos en el entorno de ejecución y, opcionalmente, en la carpeta `Route Intelligence` de Google Drive.

**Nota:** este directorio contiene los tres notebooks del avance; **no incluye necesariamente todos los datos fuente, archivos derivados o dependencias necesarios para ejecutarlos de principio a fin**. La presencia de salidas guardadas no equivale a una comprobación independiente de reproducibilidad.

## Alcance y limitaciones comunes

- Las consultas OD utilizadas son **simuladas retrospectivamente** con datos históricos de PEMS-BAY; no son solicitudes en tiempo real a Google Maps u otro proveedor.
- La comparación se centra en la **reconstrucción contemporánea** de tiempos; no demuestra predicción a futuro ni incorpora todavía GNN o modelos de lenguaje.
- Abril y mayo han intervenido en el desarrollo y exploración del proyecto. Mayo representa una comparación temporal posterior de las configuraciones reportadas, **no una prueba final completamente inédita del proyecto**. Es necesario comprobar la identidad de los conjuntos de evaluación antes de atribuir equivalencia exacta a todas las comparaciones entre libretas.
- Las solicitudes LCS son registros administrativos; sus fechas y estados no certifican cierres ejecutados ni identifican por sí solos cambios causales en el tráfico. La exportación histórica no permite reconstruir con certeza la información que estaba disponible antes de cada evento.
- Los errores de estimación de tiempos aún no se han propagado a una evaluación completa de decisiones de replaneación.

## Siguientes pasos

1. Consolidar un protocolo compartido de datos, selección de consultas y evaluación para comparar modelos en condiciones idénticas.
2. Estudiar la relación temporal y espacial entre los registros LCS y las desviaciones observadas en PEMS-BAY, cuidando la disponibilidad de información en cada instante.
3. Explorar modelos que incorporen estructura espacial de la red e información contextual, en función de la evidencia disponible.
4. Evaluar cómo los errores de reconstrucción modifican las decisiones, la factibilidad y los costos de recuperación de rutas ante interrupciones.

---

**Equipo 42 — Route Intelligence**  
Luis Ángel Herrera Flores · Francisco Natanael Ortiz Martínez · Edgar Iván Rosas Gómez
