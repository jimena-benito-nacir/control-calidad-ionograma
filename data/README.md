# Datos

## Archivo

`QC DATA Control interno de ionograma.xlsx` — registro de control interno de calidad del ionograma del laboratorio, gestionado en Google Sheets y exportado a Excel.

| Característica | Detalle |
|---|---|
| Período | 26/08/2024 al 31/08/2026 |
| Hojas | `LIBRO 1 Y 1.1` (K), `LIBRO 2 Y 2.1` (Na), `LIBRO 3 Y 3.1` (Cl) |
| Registros originales | 1.193 (398 Na, 396 K, 399 Cl) |
| Registros tras la limpieza | 1.189 (397 Na, 395 K, 397 Cl) |
| Pools de control | 11 |
| Unidades | mmol/L |

Cada hoja está organizada en bloques de columnas (páginas del registro original), con las columnas `Date`, `Time`, `QC1` y `Comment`. El notebook reconstruye una serie cronológica continua por analito (sección 5).

## Autorización y privacidad

Los datos se utilizan con autorización de la dirección técnica del laboratorio. Corresponden únicamente al material de control (pools de sueros) y **no contienen información que permita identificar pacientes**.

El archivo pertenece al laboratorio y se publica con fines académicos; no está cubierto por la licencia MIT del código.

## Correcciones aplicadas en la fuente

- Cl del 12/11/2025: 140,4 → 104,4 mmol/L (transposición de dígitos confirmada).

Las demás exclusiones (3 duplicados y 1 error de carga) se aplican en el notebook, sin modificar el archivo. Ver `data_log.md`.

## Descarga

El notebook lee el archivo desde esta carpeta. Si no lo encuentra, por ejemplo en Google Colab, lo descarga automáticamente desde este repositorio.
