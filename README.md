# Copa FutBotMX - PUMAS AE

Repositorio del equipo **PUMAS AE** para la competencia **Copa FutBotMX, Capítulo Visión por Computadora**.

El objetivo del proyecto es desarrollar un flujo reproducible de visión por computadora para analizar videos de partidos de futbol robótico mediante segmentación, seguimiento de objetos, detección de eventos y visualización de datos del juego.

## 1. Objetivo del proyecto

Desarrollar un pipeline que permita:

- Segmentar elementos relevantes del partido:
  - Campo de juego.
  - Robots aliados.
  - Robots rivales.
  - Balón.
- Rastrear las trayectorias de robots y balón a lo largo del video.
- Detectar eventos relevantes del juego, como posesión, pases, tiros, intercepciones o posibles colisiones.
- Generar visualizaciones que ayuden a interpretar la dinámica del partido.
- Producir un video demostrativo con resultados de segmentación, tracking y análisis visual.

## 2. Enfoque general

El proyecto utilizará **SAM 3 (Segment Anything Model 3)** como tecnología base para la segmentación de objetos en imágenes y video.

El flujo de trabajo propuesto es:

```text
video original
→ preprocesamiento
→ extracción de frames
→ segmentación con SAM 3
→ postprocesamiento de máscaras
→ cálculo de centroides y cajas delimitadoras
→ tracking temporal
→ detección de eventos
→ visualización de resultados
→ generación de video demo
