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
```

## 3. Estructura del repositorio

```text
configs/        Archivos de configuración del proyecto.
data/           Datos locales, clips, frames y archivos procesados.
docs/           Documentación técnica, metodología, resultados y reproducción.
media/          Material final para documentación, capturas, GIFs o enlaces.
notebooks/      Pruebas exploratorias y validaciones rápidas.
outputs/        Resultados generados por el pipeline.
scripts/        Scripts ejecutables para correr etapas del flujo.
src/            Código fuente principal del proyecto.
tests/          Pruebas básicas del código.
```

## 4. Requisitos de hardware y software

Ambiente recomendado:

- Windows 11 con WSL2.
- Ubuntu en WSL2.
- GPU NVIDIA compatible con CUDA.
- Python 3.12.
- PyTorch con soporte CUDA.
- FFmpeg.
- OpenCV.
- SAM 3.

El desarrollo inicial se realizó con una GPU **NVIDIA GeForce RTX 4060 Laptop GPU**.

## 5. Instalación

Activar el ambiente de Conda:

```bash
conda activate futbotmx-sam3
```

Instalar dependencias generales del proyecto:

```bash
pip install -r requirements.txt
```

La instalación de PyTorch con CUDA y SAM 3 se documenta en:

```text
docs/reproduction.md
```

## 6. Datos

Los videos originales deben colocarse localmente en:

```text
data/raw/
```

Por tamaño y control de versiones, los videos originales no se almacenan directamente en este repositorio.

Los clips cortos de trabajo se guardarán en:

```text
data/clips/
```

Los frames extraídos se guardarán en:

```text
data/frames/
```

Los resultados intermedios, como archivos CSV de detección, tracking o eventos, se guardarán en:

```text
data/processed/
```

## 7. Uso previsto

El proyecto se desarrollará de manera incremental. El flujo completo se ejecutará mediante scripts ubicados en:

```text
scripts/
```

Ejemplo de ejecución prevista:

```bash
bash scripts/run_baseline.sh
```

## 8. Resultados esperados

Los resultados generados se almacenarán en:

```text
outputs/
```

El proyecto buscará producir:

- Frames segmentados.
- Máscaras de objetos.
- Trayectorias de robots y balón.
- Archivos CSV con centroides, cajas delimitadoras y objetos rastreados.
- Visualizaciones de actividad del partido.
- Video demostrativo de máximo 2 minutos.
- Material para reel de Instagram de al menos 30 segundos.

## 9. Visualizaciones propuestas

Las visualizaciones iniciales consideradas son:

- Video original con máscaras superpuestas.
- Trayectorias de robots y balón.
- Mapa de calor de actividad.
- Métricas temporales de posesión.
- Línea de tiempo de eventos.
- Anotaciones visuales para narrar momentos relevantes del partido.

## 10. Estado actual

El proyecto se encuentra en fase inicial de configuración del repositorio y preparación del ambiente de desarrollo.

## 11. Reproducibilidad

Los parámetros principales del proyecto se concentran en:

```text
configs/default.yaml
```

Las instrucciones detalladas de reproducción se documentan en:

```text
docs/reproduction.md
```

## 12. Créditos y uso de terceros

Este proyecto podrá utilizar herramientas de código abierto como SAM 3, PyTorch, OpenCV, FFmpeg, NumPy, Pandas, Matplotlib y bibliotecas de tracking o visualización.

Las dependencias utilizadas, sus funciones dentro del proyecto y sus licencias se documentarán en:

```text
docs/third_party.md
```

## 13. Licencia

Este proyecto se distribuye bajo licencia MIT. Véase el archivo:

```text
LICENSE
```

## 14. Enlaces del entregable

Esta sección se completará al final del desarrollo.

- Video demostrativo: pendiente.
- Reel de Instagram: pendiente.
- Resultados principales: pendiente.
- Versión final del repositorio: pendiente.
