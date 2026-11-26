# Anexos del documento de grado

Esta carpeta reúne los anexos citados en el documento *EcoVision: prototipo de visión artificial para la detección e identificación de materiales reciclables en puntos ecológicos* (Politécnico Grancolombiano, 2026). Cada anexo indica dónde está el material dentro de este repositorio o fuera de él.

| Anexo | Contenido | Ubicación |
|---|---|---|
| A | Código fuente del prototipo, pesos del modelo y manual de instalación y uso | Raíz del repositorio: `realtime_ui_app.py`, `model/best.pt`, `README.md`, `DESKTOP.md` |
| B | Conjunto de datos EcoVision, versión 7 (867 fotografías originales, 2081 imágenes con aumento) | https://universe.roboflow.com/david-santiago-calderon-giraldo/ecovision-pcmsb |
| C | Cuaderno de entrenamiento en Google Colab y registros generados por Ultralytics | `anexos/entrenamiento/` |
| D | Registros y scripts de la medición de funcionamiento (tasas de captura e inferencia) | `verification/medir_fps.py`, `verification/medir_app.py`, `verification/medir_fps_resultado.json`, `verification/medir_app_resultado.json` |
| E | Protocolo y registro de las pruebas con objetos físicos y con objetos fuera de alcance | `anexos/evaluacion/` y `verification/latencias_2026-11-25.csv` |
| F | Diagramas UML del documento en formato fuente (PlantUML) y PNG | `diagramas/` |

## Anexo C. Entrenamiento

- `Entrenamiento_Modelo_EcoVision.ipynb`: cuaderno ejecutado en Google Colab (GPU Tesla T4). Descarga la versión 7 del conjunto desde Roboflow, entrena YOLOv8n durante 60 épocas, evalúa los pesos finales sobre la partición de prueba a 640 y 320 px y exporta los resultados. La clave de Roboflow se lee desde los secretos de Colab; las salidas de las celdas se eliminaron para publicarlo.
- `configuracion_entrenamiento.md`: versión en texto de la Tabla 9 del documento.
- `args.yaml`: parámetros completos con los que Ultralytics ejecutó el entrenamiento.
- `results.csv`: pérdidas y métricas de validación por época (la mejor fue la 59).
- `results.png`: curvas de entrenamiento (Figura 8 del documento).
- `val_confusion_matrix.png`, `val_PR_curve.png`: matriz de confusión y curva precisión-exhaustividad sobre validación.
- `test640_confusion_matrix.png`, `test640_PR_curve.png`, `test320_confusion_matrix.png`, `test320_PR_curve.png`: lo mismo sobre la partición de prueba, a los dos tamaños de entrada (Tablas 11 y 12).

## Anexo E. Pruebas con objetos físicos

- `protocolo_pruebas_objetos_fisicos.md`: cómo se hicieron los 90 intentos con las tres clases y los 50 con objetos fuera de alcance, y los conteos que resultaron.
- `registro_pruebas_objetos_fisicos.csv`: las 71 indicaciones que mostró la aplicación durante los 90 intentos, una por fila, con el objeto presentado, la clase mostrada, si fue correcta o incorrecta, la confianza y la latencia registrada por la propia aplicación. Los 19 intentos sin respuesta no generan fila porque la aplicación no mostró nada. Las estadísticas de la Tabla 14 se calculan sobre las 62 filas marcadas como correctas.
