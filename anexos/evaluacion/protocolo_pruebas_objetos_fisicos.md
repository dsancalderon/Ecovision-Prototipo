# Pruebas con objetos físicos: condiciones y conteos

Fecha: 2026-11-25. Equipo: portátil i3-1005G1, 3,8 GB RAM, sin GPU, Windows 11. Cámara integrada, 1280 × 720. App completa en modo real; modelo a 320 px; umbral 0,75.

## Tres clases (90 intentos)

- 30 por clase; 3 ejemplares por clase, ninguno usado en el dataset.
- Bloques de 10: cambia ejemplar, distancia (30 / 50 / 80 cm), ángulo; en algunos bloques, luz.
- Secuencia por intento: objeto al encuadre → quieto hasta tarjeta de resultado o 5 s → fuera del encuadre.
- Latencia: la escribe la app en `outputs/latencias.csv` (primera detección ≥ 0,75 → tarjeta). Copia: `verification/latencias_2026-11-25.csv`.
- Anotación manual: intentos sin tarjeta (no quedan en el log) y tarjetas con clase errada (sí quedan).

| Objeto | n | ok | errada | sin respuesta | mediana s | P95 s | máx s |
|---|---|---|---|---|---|---|---|
| Botella plástica | 30 | 20 | 2 → lata | 8 | 1,34 | 2,31 | 2,47 |
| Cartón | 30 | 18 | 3 → lata (tubo), 4 → botella (caja de huevos) | 5 | 1,27 | 2,12 | 2,34 |
| Lata de aluminio | 30 | 24 | 0 | 6 | 1,32 | 2,98 | 3,03 |
| Total | 90 | 62 | 9 | 19 | 1,31 | 2,34 | 3,03 |

Latencias: solo filas `correcta` de `registro_pruebas_objetos_fisicos.csv`. Columna `objeto_presentado` = bloque de la prueba; `clase_mostrada` = salida de la app.

## Fuera de alcance (50 intentos)

10 por objeto. Esperado: app en espera.

| Objeto | n | tarjeta mostrada | en espera |
|---|---|---|---|
| Tetra Pak | 10 | 0 | 10 |
| Botella de vidrio | 10 | 2 (botella plástica) | 8 |
| Vaso plástico | 10 | 0 | 10 |
| Bolsa kraft | 10 | 5 (cartón) | 5 |
| Hoja de papel | 10 | 0 | 10 |
| Total | 50 | 7 | 43 |

## FPS

Tabla 13 del documento: `verification/medir_fps_resultado.json` (sin interfaz) y `verification/medir_app_resultado.json` (interfaz abierta). Scripts: `verification/medir_fps.py`, `verification/medir_app.py`.
