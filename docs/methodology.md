# Metodología

Este documento describe la metodología técnica propuesta para analizar videos de futbol robótico de la Copa FutBotMX mediante segmentación, tracking, detección de eventos y visualización de datos.

## 1. Objetivo metodológico

Construir un pipeline reproducible de visión por computadora que permita:

- Segmentar elementos relevantes del partido.
- Rastrear trayectorias de robots y balón.
- Estimar eventos relevantes del juego.
- Generar visualizaciones interpretables.
- Producir un video demostrativo claro y verificable.

El enfoque inicial prioriza robustez, reproducibilidad y claridad sobre complejidad algorítmica excesiva.

## 2. Elementos de interés

El análisis se enfocará en los siguientes elementos:

| Elemento | Descripción | Salida esperada |
|---|---|---|
| Campo de juego | Región principal donde ocurre el partido | Máscara o región de referencia |
| Balón | Objeto principal de interacción | Máscara, centroide y trayectoria |
| Robots aliados | Robots de un equipo | Máscaras, identificadores y trayectorias |
| Robots rivales | Robots del equipo contrario | Máscaras, identificadores y trayectorias |
| Porterías o zonas objetivo | Regiones relevantes para tiros o anotaciones | Región geométrica o máscara |

## 3. Pipeline general

El flujo metodológico propuesto es:

```text
video original
→ preprocesamiento
→ extracción de frames
→ segmentación con SAM 3
→ filtrado de máscaras
→ extracción de características geométricas
→ asociación temporal
→ tracking
→ detección de eventos
→ visualización
→ video demostrativo
```

## 4. Preprocesamiento de video

Antes de aplicar segmentación, se generarán clips cortos de trabajo para reducir el costo computacional.

Parámetros iniciales:

- Duración del clip: 10 a 20 segundos.
- Ancho del video: 960 px.
- FPS de análisis: 10.
- Formato: MP4 para clips y JPG para frames.

El objetivo del preprocesamiento es obtener un conjunto manejable de frames para validar el flujo completo antes de escalar a videos más largos.

## 5. Segmentación con SAM 3

SAM 3 se utilizará como modelo base de segmentación.

La estrategia inicial será usar prompts de texto y, si es necesario, prompts visuales como puntos o cajas en frames representativos.

Prompts iniciales propuestos:

| Clase | Prompt inicial |
|---|---|
| Campo | `playing field` |
| Balón | `soccer ball` |
| Robot genérico | `robot soccer player` |
| Robot aliado | `blue robot` |
| Robot rival | `red robot` |
| Portería | `goal` |

Estos prompts podrán ajustarse después de observar los resultados reales en los videos de la competencia.

## 6. Postprocesamiento de máscaras

Las máscaras producidas por SAM 3 se filtrarán mediante reglas geométricas simples.

Características calculadas por objeto:

- Área de máscara.
- Caja delimitadora.
- Centroide.
- Clase estimada.
- Frame de aparición.
- Tiempo asociado.
- Identificador temporal, si existe tracking.

Criterios iniciales de filtrado:

- Eliminar máscaras con área menor a un umbral mínimo.
- Priorizar objetos dentro de la región del campo.
- Revisar consistencia temporal en objetos detectados en frames consecutivos.
- Separar balón y robots por tamaño, forma, posición y prompt asociado.

## 7. Tracking temporal

El método base de tracking será asociación por distancia entre centroides.

Para cada frame:

1. Obtener objetos detectados.
2. Calcular centroides.
3. Comparar centroides con objetos del frame anterior.
4. Asignar identidad si la distancia es menor a un umbral.
5. Crear nuevo identificador si no existe coincidencia.
6. Permitir desapariciones breves por oclusión.

Parámetros iniciales:

- Distancia máxima de asociación: 80 px.
- Máximo número de frames perdidos: 5.
- Método base: distancia euclidiana entre centroides.

Si el tracking por centroides no es suficiente, se considerará integrar un tracker externo como ByteTrack, DeepSORT o una herramienta equivalente, documentando claramente su uso.

## 8. Detección de eventos

La detección de eventos se planteará inicialmente mediante reglas geométricas y temporales.

### 8.1 Posesión aproximada

Se considera que un robot tiene posesión aproximada si su centroide está suficientemente cerca del balón.

Regla inicial:

```text
posesión = robot más cercano al balón si distancia(robot, balón) < umbral
```

### 8.2 Pase

Un pase se estima cuando la posesión cambia entre dos robots del mismo equipo.

### 8.3 Intercepción

Una intercepción se estima cuando la posesión cambia de un equipo a otro.

### 8.4 Tiro

Un tiro se estima cuando el balón presenta velocidad alta y su trayectoria se dirige hacia una región de portería.

### 8.5 Posible colisión

Una posible colisión se estima cuando dos robots se encuentran a una distancia menor que un umbral o cuando sus máscaras/cajas delimitadoras se superponen de forma significativa.

## 9. Visualizaciones

Las visualizaciones tendrán como objetivo narrar el desarrollo del partido de forma clara.

Visualizaciones propuestas:

- Video original con máscaras superpuestas.
- Video con cajas, centroides e identificadores.
- Trayectorias de robots y balón.
- Mapa de calor de actividad.
- Gráfica temporal de posesión.
- Línea de tiempo de eventos.
- Capturas comparativas del video original y el resultado segmentado.

## 10. Formato de salidas tabulares

Se generará al menos un archivo CSV de tracking con estructura similar a:

```text
frame,time_s,object_id,class,x_px,y_px,bbox_x,bbox_y,bbox_w,bbox_h,area_px
```

También podrá generarse un archivo de eventos con estructura similar a:

```text
time_s,event_type,object_id,team,description,confidence
```

## 11. Métricas internas de evaluación

Aunque el reto no imponga una métrica única, se documentarán métricas internas para evaluar el comportamiento del pipeline.

Métricas iniciales:

- Número de frames procesados.
- Tasa aproximada de objetos detectados por frame.
- Continuidad de tracks.
- Número de eventos detectados.
- Tiempo de procesamiento por clip.
- Porcentaje de frames con balón detectado.

Estas métricas ayudarán a comparar cambios en prompts, resolución, FPS o métodos de tracking.

## 12. Limitaciones esperadas

El sistema puede presentar limitaciones por:

- Oclusiones entre robots.
- Desenfoque o movimiento rápido del balón.
- Variación de iluminación.
- Similitud visual entre robots.
- Cambios de escala por perspectiva.
- Prompts de texto insuficientemente específicos.
- Errores de segmentación en objetos pequeños.

Estas limitaciones se documentarán como parte del análisis final, junto con ejemplos visuales cuando sea posible.

## 13. Estrategia de desarrollo

El desarrollo se organizará en etapas:

1. Cargar y visualizar frames.
2. Probar segmentación en un frame individual.
3. Procesar un clip corto.
4. Generar máscaras y centroides.
5. Implementar tracking básico.
6. Detectar eventos simples.
7. Generar visualizaciones.
8. Crear video demostrativo.
9. Documentar resultados, limitaciones y créditos.

## 14. Criterio de éxito inicial

El primer hito técnico será generar un video corto donde se observe:

- Video original.
- Máscaras superpuestas.
- Identificación visual de balón y robots.
- Trayectorias acumuladas.
- Al menos una visualización adicional del juego.

Este hito permitirá validar el flujo completo antes de refinar precisión o ampliar el análisis.
