# Avance 4. Modelos alternativos

**Proyecto Integrador:** Planeación inteligente de rutas ante interrupciones mediante técnicas de optimización e inteligencia artificial.

**Equipo 42 - Route Intelligence**

**Maestría en Inteligencia Artificial Aplicada**  
Tecnológico de Monterrey  
25 de octubre de 2026

## Descripción

Este avance desarrolla y compara seis modelos individuales de aprendizaje automático para reconstruir tiempos de desplazamiento en una red vial experimental a partir de un conjunto reducido de consultas origen-destino (OD) contemporáneas simuladas.

El trabajo comprende la comparación de modelos, el ajuste de hiperparámetros de los dos mejores candidatos y la selección del modelo individual final.

## Entregable

- [Avance4_Equipo42.ipynb](./Avance4_Equipo42.ipynb) - Notebook ejecutada con el desarrollo metodológico, los experimentos, las tablas de resultados, las visualizaciones y las conclusiones.

## Resultados principales

Se evaluaron Ridge, KNN, MLP, PPCA, PLS y árbol de decisión.

La comparación y el ajuste de hiperparámetros permitieron seleccionar **Ridge multisalida**, con regularización $\alpha=1000$ y $K=32$ consultas guía.

En la evaluación final sobre junio de 2017, el modelo obtuvo un MAE OD de **11.4161 segundos**, frente a **31.7485 segundos** de la referencia histórica, equivalente a una reducción relativa del **64.04 %**.

## Alcance y reproducibilidad

Los experimentos utilizan datos derivados de PEMS-BAY y consultas OD simuladas. Los resultados corresponden a la reconstrucción de matrices de tiempos de desplazamiento dentro del grafo experimental; no incluyen consultas comerciales reales ni la ejecución de algoritmos de replaneación de rutas.

La notebook conserva las salidas de una ejecución secuencial completa. Para reproducir los experimentos desde cero se requiere acceso a los archivos de datos preparados, utilizados desde Google Drive.
