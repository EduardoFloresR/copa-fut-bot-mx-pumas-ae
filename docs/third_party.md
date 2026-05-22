# Software de terceros y créditos

Este documento registra las herramientas, bibliotecas, modelos y recursos externos utilizados o considerados en el proyecto.

El objetivo es mantener transparencia sobre el uso de código de terceros, respetar licencias y facilitar la revisión del repositorio.

## 1. Política de atribución

Toda herramienta externa utilizada en el proyecto debe documentarse indicando:

- Nombre de la herramienta.
- Propósito dentro del pipeline.
- Sitio oficial o repositorio.
- Licencia correspondiente.
- Forma en que se utiliza en el proyecto.

No se presentará como propio código externo sin atribución. Si se adapta código de terceros, deberá indicarse claramente en el archivo fuente correspondiente y en este documento.

## 2. Herramientas principales

| Herramienta | Uso previsto | Sitio o repositorio | Licencia / nota |
|---|---|---|---|
| SAM 3 | Segmentación de objetos en imágenes y video | `github.com/facebookresearch/sam3` | Revisar licencia oficial de SAM 3 |
| PyTorch | Inferencia del modelo en GPU | `pytorch.org` | Licencia BSD-style |
| OpenCV | Lectura de imágenes, video y procesamiento visual | `opencv.org` | Licencia Apache 2.0 |
| FFmpeg | Conversión de video y extracción de frames | `ffmpeg.org` | Revisar licencias de componentes usados |
| NumPy | Cálculo numérico | `numpy.org` | Licencia BSD |
| Pandas | Manejo de tablas y CSV | `pandas.pydata.org` | Licencia BSD |
| Matplotlib | Generación de gráficas | `matplotlib.org` | Licencia Matplotlib |
| Supervision | Utilidades de anotación, detección y tracking visual | `github.com/roboflow/supervision` | Revisar licencia oficial |
| Hugging Face Hub | Descarga/autenticación de modelos | `huggingface.co` | Revisar términos del modelo específico |

## 3. Herramientas opcionales

Las siguientes herramientas podrán evaluarse si el tracking básico por centroides no resulta suficiente:

| Herramienta | Uso posible | Nota |
|---|---|---|
| ByteTrack | Asociación temporal de objetos | Usar solo si aporta mejora clara al pipeline |
| DeepSORT | Tracking con apariencia visual | Puede requerir mayor costo computacional |
| Roboflow Supervision | Visualización y utilidades de tracking | Debe citarse si se usa en resultados finales |

## 4. Modelo SAM 3

SAM 3 se considera la tecnología base de segmentación del proyecto.

Uso previsto:

- Segmentación de balón.
- Segmentación de robots.
- Segmentación del campo o regiones relevantes.
- Posible tracking o propagación temporal, si el modelo y los recursos disponibles lo permiten.

Consideraciones:

- Los pesos del modelo no deben subirse al repositorio.
- El acceso al modelo puede requerir autenticación en Hugging Face.
- El uso debe respetar la licencia oficial del modelo.
- Cualquier modificación o ajuste debe documentarse.

## 5. Datos de la competencia

Los videos de partidos de futbol robótico utilizados en el proyecto provienen del repositorio oficial del reto.

Consideraciones:

- No subir videos originales pesados al repositorio salvo que la organización lo permita explícitamente.
- Documentar el nombre del video o clip usado en cada experimento.
- Mantener los datos originales en `data/raw/`.
- Guardar clips derivados en `data/clips/`.

## 6. Código propio del equipo

El código desarrollado por el equipo se ubicará principalmente en:

```text
src/futbotmx/
```

Los scripts ejecutables se ubicarán en:

```text
scripts/
```

Los notebooks de exploración se ubicarán en:

```text
notebooks/
```

El código propio deberá estar suficientemente comentado para explicar el papel de cada etapa del pipeline.

## 7. Licencia del proyecto

El repositorio utiliza licencia MIT, indicada en el archivo:

```text
LICENSE
```

Esta licencia cubre el código propio del equipo, pero no reemplaza ni modifica las licencias de herramientas, modelos, datos o bibliotecas de terceros.

## 8. Pendientes de verificación

Antes de la entrega final se deberá verificar:

- Licencia oficial del repositorio de SAM 3.
- Términos de uso de los pesos del modelo en Hugging Face.
- Licencia de cualquier tracker externo utilizado.
- Licencia y requisitos de atribución de cualquier código adaptado.
- Permisos de uso de clips, capturas o videos en el reel y video demostrativo.

## 9. Registro de dependencias efectivamente usadas

Esta sección debe actualizarse al final del desarrollo.

| Dependencia | ¿Usada en entrega final? | Función concreta | Comentarios |
|---|---|---|---|
| SAM 3 | Pendiente | Segmentación | Pendiente |
| PyTorch | Sí | Inferencia en GPU | Pendiente |
| OpenCV | Pendiente | Video/imágenes | Pendiente |
| FFmpeg | Pendiente | Preprocesamiento de video | Pendiente |
| NumPy | Pendiente | Cálculo numérico | Pendiente |
| Pandas | Pendiente | CSV y tablas | Pendiente |
| Matplotlib | Pendiente | Gráficas | Pendiente |
| Supervision | Pendiente | Visualización/tracking | Pendiente |
