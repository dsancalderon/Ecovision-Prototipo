# Anexos

Índice de los anexos citados en el documento de grado de EcoVision (Politécnico Grancolombiano, 2026).

| Anexo | Qué es | Dónde está |
|---|---|---|
| A | Aplicación, pesos, instrucciones | raíz del repo: `realtime_ui_app.py`, `model/best.pt`, `README.md`, `DESKTOP.md` |
| B | Dataset v7 | https://universe.roboflow.com/david-santiago-calderon-giraldo/ecovision-pcmsb |
| C | Notebook de Colab + salidas de Ultralytics | `anexos/entrenamiento/` |
| D | Scripts y JSON de medición de FPS | `verification/medir_fps.py`, `verification/medir_app.py`, `verification/*_resultado.json` |
| E | Pruebas con objetos (protocolo + log) | `anexos/evaluacion/`, `verification/latencias_2026-11-25.csv` |
| F | Fuentes PlantUML de las figuras | `diagramas/` |

## anexos/entrenamiento/

| Archivo | Contenido |
|---|---|
| `Entrenamiento_Modelo_EcoVision.ipynb` | notebook (Colab, T4): descarga dataset v7, train YOLOv8n 60 épocas, val en test a 640/320, export. Clave Roboflow vía `userdata`; salidas borradas |
| `configuracion_entrenamiento.md` | Tabla 9 en texto |
| `args.yaml` | hiperparámetros completos (Ultralytics) |
| `results.csv` | métricas por época; mejor = 59 |
| `results.png` | curvas (Figura 8) |
| `val_*.png` | matriz de confusión y curva PR, partición de validación |
| `test640_*.png`, `test320_*.png` | lo mismo, partición de prueba, dos tamaños de entrada |

## anexos/evaluacion/

| Archivo | Contenido |
|---|---|
| `protocolo_pruebas_objetos_fisicos.md` | condiciones de las 90 + 50 pruebas y conteos |
| `registro_pruebas_objetos_fisicos.csv` | 71 filas = indicaciones mostradas por la app (62 ok + 9 erradas); columna `resultado`. Sin fila para los 19 intentos sin respuesta. Tabla 14 = estadísticas de las 62 filas `correcta` |
