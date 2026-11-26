# Tabla 9 (versión en texto)

Parámetros completos: `args.yaml`. Métricas por época: `results.csv`.

| Parámetro | Valor |
|---|---|
| Modelo base | YOLOv8n con pesos preentrenados (yolov8n.pt): 3,1 millones de parámetros y 8,6 GFLOPs |
| Clases | botella_reciclable, carton, lata_aluminio |
| Conjunto de datos | Roboflow, versión 7 (2081 imágenes: 867 originales; 607 / 130 / 130 en entrenamiento / validación / prueba; aumento ×3 solo en entrenamiento) |
| Preprocesamiento | Auto-orientación y redimensionamiento a 640 × 640 px conservando la proporción (bordes negros) |
| Aumento de datos (solo entrenamiento) | Tres salidas por imagen: volteo horizontal, rotación ±15°, brillo ±25 %, exposición ±15 % y desenfoque hasta 1,5 px |
| Tamaño de entrada en entrenamiento | 640 px |
| Épocas (máximo / paciencia / ejecutadas / mejor) | 60 / 15 / 60 / 59 |
| Tamaño de lote / semilla | 16 / 0 |
| Desempeño en validación (mejor época) | mAP@0,50 = 0,989; mAP@0,50:0,95 = 0,820 |
| Entorno de entrenamiento | Google Colab, GPU NVIDIA Tesla T4, Python 3.13.16, PyTorch 2.14.1, Ultralytics 8.4.218 |
| Entorno de ejecución de la aplicación | Windows 11 Home (10.0.26200), Python 3.14.0, OpenCV 5.0.0, Flask 3.1.3, PyWebView 6.2.1, Ultralytics 8.4.123, PyTorch 2.13.0 (CPU) |
| Inferencia en la aplicación | Tamaño de entrada de 320 px; umbral de confianza de 0,75 |

Partición de prueba (130 imágenes, 169 instancias):

| Tamaño de entrada | Precisión | Exhaustividad | mAP@0,50 | mAP@0,50:0,95 |
|---|---|---|---|---|
| 640 px | 0,977 | 0,954 | 0,954 | 0,798 |
| 320 px (como en la aplicación) | 0,941 | 0,928 | 0,952 | 0,742 |
