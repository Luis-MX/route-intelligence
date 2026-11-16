# Avance 7 — Proyecto de difusión

**Proyecto Integrador | Equipo 42 — Route Intelligence**  
Maestría en Inteligencia Artificial Aplicada  
Tecnológico de Monterrey

## Descripción

Esta entrega presenta los resultados del proyecto sobre estimación y pronóstico de tiempos de viaje en redes dirigidas, desarrollado como apoyo potencial a la planeación y recuperación de rutas con ventanas de tiempo.

Se documentan dos procedimientos: el pronóstico anticipado de tiempos de viaje para la jornada siguiente (M1) y la reconstrucción contemporánea de matrices origen–destino mediante consultas simuladas (K=32).

## Materiales de la entrega

- **`articulo_academico.pdf`**: artículo que presenta la metodología, los experimentos, los resultados y las conclusiones del proyecto.
- **`resumen_ejecutivo.pdf`**: síntesis de los principales objetivos, resultados y aportaciones.
- **`Analisis_complementario_M1_K32_y_matrices_OD_Equipo42.ipynb`**: cuaderno de análisis complementario, evaluación de errores, comparación de condiciones de circulación y generación de matrices OD.
- **`fuentes/`**: archivos editables y recursos necesarios para reproducir el artículo académico y el resumen ejecutivo, organizados en subcarpetas independientes.

## Antecedentes técnicos

Los procedimientos desarrollados y sus evaluaciones se documentan en las carpetas de los Avances 3 a 6, disponibles en el directorio `docs/entregas` de este mismo repositorio.

Los resultados corresponden a un entorno experimental construido con datos derivados de PEMS-BAY. La integración de las matrices generadas en un optimizador de rutas queda fuera del alcance de esta entrega.


## Consulta y descarga de matrices OD

Como parte del proyecto se desarrolló una interfaz interactiva para consultar y descargar matrices de tiempos de viaje origen--destino (OD).

La interfaz está integrada en la libreta
`Analisis_complementario_M1_K32_y_matrices_OD_Equipo42.ipynb`
y permite:

- Seleccionar el procedimiento de estimación (M1 o K=32).
- Elegir una fecha de junio de 2017.
- Seleccionar los horarios disponibles y los nodos de interés.
- Generar matrices de tiempos OD mediante caminos mínimos dirigidos.
- Descargar las matrices en formato CSV, junto con los catálogos y metadatos correspondientes.

### Paquete de datos

El archivo `entrega_matrices_OD_optimizador.zip` contiene las estimaciones y los recursos necesarios para generar las matrices sin volver a entrenar los modelos.

**Descarga del paquete:**

[Avance 7 — Paquete de matrices OD](https://github.com/Luis-MX/route-intelligence/releases/tag/avance-7)

### Instrucciones de uso

1. Descargar el archivo ZIP desde la sección de archivos de la release.
2. Abrir la libreta complementaria en Google Colab.
3. Ejecutar las celdas de preparación y cargar el paquete ZIP cuando se solicite.
4. Acceder a la sección "Consulta personalizada de matrices OD".
5. Seleccionar el modelo, la fecha, los horarios y los nodos.
6. Generar y descargar las matrices solicitadas.

Las estimaciones corresponden exclusivamente a junio de 2017 y a la red vial experimental construida a partir de PEMS-BAY. No representan condiciones de tráfico en tiempo real.
