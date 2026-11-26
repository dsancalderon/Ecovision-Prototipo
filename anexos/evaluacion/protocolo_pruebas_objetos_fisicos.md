# Protocolo y resultados de las pruebas con objetos físicos

Pruebas realizadas el 25 de noviembre de 2026 en el portátil de laboratorio (Intel Core i3-1005G1, 3,8 GB de RAM, sin GPU dedicada, Windows 11) con la cámara integrada a 1280 × 720, la aplicación completa en modo real, entrada del modelo de 320 px y umbral de confianza de 0,75.

## Pruebas con las tres clases (90 intentos)

- 30 intentos por clase, con tres ejemplares de cada una distintos a los fotografiados para el conjunto de datos.
- Los intentos se repartieron en bloques de diez que cambiaban el ejemplar, la distancia (30, 50 y 80 cm), el ángulo del objeto y, en una parte de ellos, la iluminación.
- En cada intento el objeto entraba al encuadre, se mantenía quieto hasta que apareciera la tarjeta de resultado o hasta cumplir 5 s, y se retiraba por completo antes del siguiente.
- La latencia la registró la propia aplicación en `outputs/latencias.csv` (copia en `verification/latencias_2026-11-25.csv`): desde la primera detección con confianza igual o superior a 0,75 hasta la presentación de la tarjeta de resultado.
- Los intentos sin respuesta y los que terminaron con una clase equivocada se anotaron a mano. Los primeros no generan fila en el registro; los segundos sí, porque la aplicación mostró un resultado.

| Objeto presentado | Intentos | Indicación correcta | Indicación incorrecta | Sin respuesta | Latencia mediana (s) | Latencia P95 (s) | Máximo (s) |
|---|---|---|---|---|---|---|---|
| Botella plástica | 30 | 20 | 2 (mostradas como lata) | 8 | 1,34 | 2,31 | 2,47 |
| Cartón | 30 | 18 | 7 (3 como lata, un tubo de cartón; 4 como botella, una caja de huevos) | 5 | 1,27 | 2,12 | 2,34 |
| Lata de aluminio | 30 | 24 | 0 | 6 | 1,32 | 2,98 | 3,03 |
| Total | 90 | 62 | 9 | 19 | 1,31 | 2,34 | 3,03 |

Las latencias se calculan solo sobre las indicaciones correctas. En `registro_pruebas_objetos_fisicos.csv` cada fila indica el objeto presentado (según el bloque de la prueba), la clase que mostró la aplicación y si la indicación fue correcta o incorrecta.

## Pruebas con objetos fuera del alcance del modelo (50 intentos)

Diez intentos con cada objeto. Lo esperado es que la aplicación permanezca en espera, porque ninguno pertenece a las tres clases.

| Objeto presentado | Intentos | Identificado como una clase | Sin detección (espera) |
|---|---|---|---|
| Envase Tetra Pak | 10 | 0 | 10 |
| Botella de vidrio | 10 | 2 (botella plástica) | 8 |
| Vaso plástico desechable | 10 | 0 | 10 |
| Bolsa de papel kraft | 10 | 5 (cartón) | 5 |
| Hoja de papel | 10 | 0 | 10 |
| Total | 50 | 7 | 43 |

## Medición de tasas de captura e inferencia

Los registros de las tres condiciones de la Tabla 13 del documento están en `verification/medir_fps_resultado.json` (inferencia aislada y captura más inferencia, sin interfaz) y `verification/medir_app_resultado.json` (aplicación completa con la ventana abierta). Los scripts que los generan son `verification/medir_fps.py` y `verification/medir_app.py`.
