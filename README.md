# EcoVision

Prototipo de escritorio que identifica **botellas plásticas, latas de aluminio y cartón** con la cámara del computador y le indica al usuario en qué contenedor del punto ecológico depositarlos. Toda la inferencia corre en el propio equipo, sin internet.

Proyecto de grado, Ingeniería de Sistemas, Institución Universitaria Politécnico Grancolombiano (2026). Autor: David Santiago Calderón Giraldo.

- Conjunto de datos (Roboflow Universe, versión 7): https://universe.roboflow.com/david-santiago-calderon-giraldo/ecovision-pcmsb
- Este repositorio contiene el código de la aplicación, los pesos del detector (`model/best.pt`), los diagramas del documento y los scripts y registros de la evaluación.

## Cómo funciona

1. **Captura.** Un hilo lee la cámara con OpenCV de forma continua y guarda el último fotograma.
2. **Inferencia.** Otro hilo pasa ese fotograma por un detector YOLOv8n (entrada de 320 px, umbral de confianza 0,75) y guarda la clase, el recuadro y la confianza. Los dos hilos comparten un búfer protegido con un candado.
3. **Decisión.** La interfaz solo muestra un resultado cuando la misma detección se sostiene durante 1,2 s (con tolerancia de 350 ms a pérdidas breves). Si hay más de un objeto sobre el umbral, pide retirar los demás. Si no reconoce nada, se queda en espera: nunca clasifica como "no aprovechable".
4. **Presentación.** Un servidor Flask local (solo `127.0.0.1`) sirve el video como MJPEG y envía el estado por *Server-Sent Events* cada 150 ms a una interfaz HTML/CSS/JS mostrada en una ventana de PyWebView.

Los diagramas UML (casos de uso, componentes, secuencia, estados, despliegue y actividades) están en `diagramas/` como PlantUML y PNG.

## Modelo y datos

| | |
|---|---|
| Modelo base | YOLOv8n preentrenado (3,1 M de parámetros), Ultralytics |
| Clases | `botella_reciclable`, `carton`, `lata_aluminio` |
| Fotografías originales | 867, tomadas para el proyecto |
| Imágenes tras aumento | 2 081 (aumento ×3 solo en entrenamiento) |
| Partición | 607 / 130 / 130 imágenes (entrenamiento / validación / prueba), asignación aleatoria |
| Entrenamiento | Google Colab, GPU T4, 640 px, lote 16, 60 épocas (mejor: 59), semilla 0 |
| Prueba (130 imágenes, 640 px) | mAP@0,50 = 0,954 · mAP@0,50:0,95 = 0,798 · precisión 0,977 · exhaustividad 0,954 |
| Prueba a 320 px (como en la app) | mAP@0,50 = 0,952 · mAP@0,50:0,95 = 0,742 |

Los pesos finales están en `model/best.pt`. El cuaderno de entrenamiento, los parámetros (`args.yaml`), las curvas, las matrices de confusión y `results.csv` están en `anexos/entrenamiento/`.

## Evaluación en el equipo de laboratorio

Portátil Intel Core i3-1005G1 (2 núcleos), 3,8 GB de RAM, sin GPU, Windows 11, cámara integrada a 1280 × 720.

| Medida | Valor |
|---|---|
| Inferencias por segundo, modelo solo | 29,2 |
| Inferencias por segundo, cámara + inferencia sin interfaz | 26,5 |
| Inferencias por segundo, **aplicación completa abierta** | 14,0 (9,7 fotogramas nuevos/s; la cámara entrega 9,9 FPS) |
| Prueba con objetos físicos (90 intentos, 30 por clase) | 62 correctos, 19 sin respuesta, 9 con clase equivocada |
| Latencia de respuesta (62 aciertos, registro automático) | mediana 1,31 s · percentil 95 2,34 s · máximo 3,03 s |
| Objetos fuera de alcance (50 intentos) | 43 en espera; kraft → cartón 5/10; vidrio → botella 2/10 |

Los registros están en `verification/`: `medir_fps_resultado.json`, `medir_app_resultado.json`, `latencias_2026-11-25.csv` y `latencias_resumen.json`. El protocolo de las pruebas y el registro anotado de los 90 intentos están en `anexos/evaluacion/`; el índice de todos los anexos del documento, en `anexos/README.md`.

## Requisitos

- Windows 10/11, Python 3.11 o superior (probado con 3.14.0).
- Cámara integrada o USB.
- Dependencias: `pip install -r requirements.txt` (Ultralytics 8.4, OpenCV 5.0, Flask 3.1, PyWebView 6.2, PyTorch CPU). Para empaquetar, además `requirements-desktop.txt`.
- Microsoft Edge WebView2 (ya viene en Windows 11; `scripts/download_webview2.py` descarga el instalador si falta).

## Ejecutar

```bash
python realtime_ui_app.py
```

Abre la ventana con el menú para elegir **Prueba real** (cámara y modelo) o **Simulación** (solo interfaz, sin cámara). Opciones útiles:

| Opción | Efecto |
|---|---|
| `--cam N` / `--cam1` / `--cam2` | Entra directo en modo real con la cámara de índice N (0 o 1) |
| `--sin-camara` | Entra directo en simulación |
| `--conf 0.75` | Umbral de confianza (0,10 a 0,95) |
| `--model ruta.pt` | Usa otros pesos |
| `--fullscreen` | Ventana a pantalla completa |
| `--serve` / `--browser` | Solo el servidor local, o abrirlo en el navegador |
| `--dev` | Recarga automática de interfaz y código durante el desarrollo |

`realtime_cam.py` es una vista mínima en una ventana de OpenCV, sin interfaz, útil para depurar el modelo.

## Medir

Con la aplicación abierta en modo real:

```bash
python verification/medir_app.py --seg 60
```

Lee los contadores internos de la app (`/api/perf`) durante 60 s y reporta tasa de inferencia, fotogramas nuevos analizados y FPS de captura con la interfaz en ejecución.

La app registra sola la latencia de cada resultado mostrado en `outputs/latencias.csv` (desde la primera detección sobre el umbral hasta la tarjeta de resultado). Para resumirla por clase:

```bash
python verification/medir_app.py --latencias
```

`verification/medir_fps.py` mide el techo del modelo sin interfaz (inferencia aislada y cámara + inferencia).

## Empaquetar

`scripts/build_desktop.ps1` genera `dist/EcoVision/EcoVision.exe` con PyInstaller (ver `DESKTOP.md`). Las carpetas `build/`, `dist/` y `packaging/dependencies/` no se versionan.

## Estructura

```
realtime_ui_app.py      aplicación: captura, inferencia, servidor Flask y ventana
desktop_runtime.py      sesión de escritorio, rutas locales y ajustes
realtime_cam.py         vista OpenCV sin interfaz
templates/, static/     interfaz web (HTML, CSS, JS, recursos)
model/                  best.pt (3 clases) y yolov8n.pt (base)
diagramas/              figuras UML del documento (.puml y .png)
verification/           scripts de medición, registros de evaluación y pruebas de interfaz
scripts/                empaquetado y utilidades
```

## Límites conocidos

- Tres clases. Tetra Pak, vidrio y residuos no aprovechables no se reconocen; ante ellos la app se queda en espera o, si se parecen a una clase (kraft, vidrio), puede equivocarse.
- El reparto de las particiones fue aleatorio por imagen; fotos de un mismo objeto pudieron quedar en entrenamiento y prueba.
- Todo se evaluó en un laboratorio, con un equipo y una cámara. No hubo pruebas con usuarios.

## Licencia

Proyecto académico. Las fuentes e iconos de `static/vendor/` conservan sus licencias originales (OFL y MIT).
