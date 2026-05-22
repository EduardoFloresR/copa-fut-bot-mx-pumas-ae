# Reproducción del proyecto

Este documento describe el procedimiento recomendado para reconstruir el ambiente de trabajo, instalar las dependencias principales y ejecutar las primeras etapas del pipeline de visión por computadora para la Copa FutBotMX.

## 1. Ambiente recomendado

El desarrollo del proyecto se plantea con la siguiente configuración:

- Sistema operativo anfitrión: Windows 11.
- Entorno Linux: Ubuntu mediante WSL2.
- GPU: NVIDIA compatible con CUDA.
- Ambiente de Python: Conda.
- Versión de Python: 3.12.
- Modelo base: SAM 3.
- Bibliotecas principales: PyTorch, OpenCV, FFmpeg, NumPy, Pandas y Matplotlib.

La configuración inicial del equipo de desarrollo utiliza una GPU NVIDIA GeForce RTX 4060 Laptop GPU con 8 GB de memoria dedicada.

## 2. Verificación de GPU en WSL2

Dentro de Ubuntu/WSL2, verificar que la GPU sea visible:

```bash
nvidia-smi
```

El resultado debe mostrar la GPU NVIDIA disponible. Si este comando falla, no se deben instalar controladores NVIDIA dentro de Ubuntu/WSL. Primero debe revisarse la instalación del controlador NVIDIA en Windows y actualizar WSL.

## 3. Activación del ambiente Conda

Activar el ambiente del proyecto:

```bash
conda activate futbotmx-sam3
```

Verificar la versión de Python:

```bash
python --version
```

Debe mostrarse una versión compatible con Python 3.12.

## 4. Instalación de PyTorch con CUDA

PyTorch debe instalarse con soporte CUDA. En este proyecto se utiliza una instalación basada en ruedas CUDA para PyTorch:

```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu128
```

Verificar que PyTorch detecta la GPU:

```bash
python - <<'PY'
import torch

print("PyTorch:", torch.__version__)
print("CUDA disponible:", torch.cuda.is_available())

if torch.cuda.is_available():
    print("GPU:", torch.cuda.get_device_name(0))
PY
```

El resultado esperado es que `torch.cuda.is_available()` devuelva `True`.

## 5. Instalación de dependencias generales

Desde la raíz del repositorio:

```bash
pip install -r requirements.txt
```

Estas dependencias cubren procesamiento de video, análisis de datos, visualización, notebooks y utilidades generales del proyecto.

## 6. Instalación de SAM 3

Se recomienda instalar SAM 3 fuera del repositorio de la competencia, para mantener separado el código externo del código propio del equipo.

```bash
cd ~
mkdir -p external
cd external
git clone https://github.com/facebookresearch/sam3.git
cd sam3
pip install -e ".[notebooks]"
```

Verificar que el ambiente sigue funcionando:

```bash
python -c "import torch; print('Ambiente listo para SAM 3')"
```

## 7. Autenticación en Hugging Face

Si el modelo requiere acceso a pesos desde Hugging Face, iniciar sesión:

```bash
hf auth login
```

Después de generar el token en Hugging Face, pegarlo en la terminal cuando se solicite. El token no debe subirse al repositorio ni escribirse en archivos del proyecto.

## 8. Organización de datos

Los videos originales deben colocarse localmente en:

```text
data/raw/
```

Estos archivos no se rastrean con Git para evitar subir archivos pesados o datos externos.

Los clips cortos de prueba se guardarán en:

```text
data/clips/
```

Los frames extraídos se guardarán en:

```text
data/frames/
```

Los resultados intermedios, como tablas CSV de detección, tracking o eventos, se guardarán en:

```text
data/processed/
```

## 9. Creación de un clip corto de prueba

Para trabajar inicialmente con un fragmento manejable del video:

```bash
ffmpeg -i data/raw/video_original.mp4 \
  -ss 00:00:10 \
  -t 00:00:20 \
  -vf "scale=960:-2,fps=10" \
  data/clips/test_20s_960px_10fps.mp4
```

Los parámetros iniciales recomendados son:

- Duración: 10 a 20 segundos.
- Ancho: 960 px.
- FPS: 10.
- Modo: inferencia, no entrenamiento.

## 10. Extracción de frames

Extraer frames del clip de prueba:

```bash
mkdir -p data/frames/test_20s

ffmpeg -i data/clips/test_20s_960px_10fps.mp4 \
  data/frames/test_20s/frame_%06d.jpg
```

Verificar que se crearon los frames:

```bash
ls data/frames/test_20s | head
```

## 11. Ejecución prevista del pipeline

El flujo completo se implementará gradualmente mediante scripts en la carpeta `scripts/`.

La ejecución general prevista será:

```bash
bash scripts/run_baseline.sh
```

El pipeline final deberá realizar:

```text
video original
→ generación de clip corto
→ extracción de frames
→ segmentación con SAM 3
→ postprocesamiento de máscaras
→ tracking de objetos
→ detección de eventos
→ generación de visualizaciones
→ generación de video demostrativo
```

## 12. Resultados esperados

Los resultados generados se almacenarán en:

```text
outputs/
```

Distribución prevista:

```text
outputs/masks/      Máscaras y overlays generados.
outputs/tracks/     Archivos CSV con trayectorias.
outputs/figures/    Gráficas y visualizaciones.
outputs/videos/     Videos demostrativos.
outputs/logs/       Registros de ejecución.
```

## 13. Recomendaciones para reproducibilidad

Para facilitar la reproducción:

- Mantener los parámetros principales en `configs/default.yaml`.
- No modificar rutas directamente dentro del código fuente.
- Registrar el nombre del video, clip usado, resolución y FPS.
- Guardar archivos CSV intermedios.
- Documentar errores, limitaciones y decisiones de diseño.
- No subir tokens, credenciales, videos originales pesados ni pesos de modelos al repositorio.

## 14. Solución de problemas comunes

### PyTorch no detecta CUDA

Verificar:

```bash
nvidia-smi
```

y después:

```bash
python - <<'PY'
import torch
print(torch.cuda.is_available())
PY
```

Si `nvidia-smi` funciona pero PyTorch no detecta CUDA, revisar la instalación de PyTorch y reinstalar la versión con soporte CUDA.

### El repositorio no sube a GitHub

Verificar el remoto:

```bash
git remote -v
```

Para HTTPS, GitHub requiere un Personal Access Token en lugar de contraseña.

### Los archivos de video no aparecen en Git

Es el comportamiento esperado. Las carpetas `data/raw`, `data/clips`, `data/frames` y algunas salidas pesadas están excluidas mediante `.gitignore`.

## 15. Estado del documento

Este documento deberá actualizarse conforme se consolide el pipeline final y se definan los comandos definitivos para reproducir los resultados enviados.
