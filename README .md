# Control de calidad interno del ionograma

**Evaluación de Isolation Forest como complemento a las reglas de Westgard, con EWMA y CUSUM como referencias adicionales**

Proyecto Final Integrador — Diplomatura en Ciencia de Datos y Análisis Avanzado (UTN)
Grupo 35 — María Jimena Benito Nacir — Octubre de 2026

---

## Descripción

El laboratorio evalúa el control interno del ionograma (Na, K y Cl) mediante reglas de Westgard antes de liberar resultados de pacientes. Este proyecto analiza si **Isolation Forest**, un algoritmo de Machine Learning no supervisado, aporta información adicional útil como complemento de ese control.

Para ello, se aplican cuatro métodos sobre las mismas mediciones de seguimiento:

- **Reglas de Westgard** (1₃s, 2₂s, 4₁s y 10x): método actual del laboratorio.
- **Isolation Forest:** detección de mediciones atípicas.
- **EWMA y CUSUM:** métodos estadísticos de referencia para desviaciones sostenidas.

Las señales se comparan entre sí y se revisan casos seleccionados con criterio bioquímico. El análisis es retrospectivo y exploratorio y sigue la metodología CRISP-DM.

## Pregunta del proyecto

¿Aporta Isolation Forest información adicional útil como complemento de las reglas de Westgard en el control interno del ionograma?

## Principales resultados

- Se analizaron 1.189 registros válidos de 11 pools de control (26/08/2024 al 31/08/2026) y se compararon 592 mediciones de seguimiento.
- Westgard señaló 112 mediciones; Isolation Forest, 356; EWMA, 97; y CUSUM, 182.
- Las señales de Isolation Forest son muy sensibles al umbral: en una simulación con mediciones estables, sin desvíos, señaló el 51,0 % de los valores con `contamination="auto"` y el 5,3 % con `contamination=0.05`.
- La revisión bioquímica mostró que la interpretación de las señales depende de la adecuación de la línea de base utilizada como referencia.
- **Conclusión:** con los datos disponibles, no se justifica incorporar Isolation Forest de forma rutinaria. Las tablas comparativas pueden utilizarse como apoyo a la revisión bioquímica.

## Estructura del repositorio

```
.
├── README.md
├── requirements.txt
├── TP_Final_Grupo35_Control_Calidad_Interno_Ionograma.ipynb
└── data/
    └── QC DATA Control interno de ionograma.xlsx
```

## Datos

El archivo `QC DATA Control interno de ionograma.xlsx` contiene el registro del control interno del ionograma del laboratorio, con tres hojas (una por analito). Cada registro incluye la fecha, la hora, el resultado (mmol/L) y los comentarios de eventos (pool nuevo, cambio de kit, calibración, cambio de electrodos).

Los datos se utilizan con autorización de la dirección técnica del laboratorio. Corresponden únicamente al material de control y **no contienen información de pacientes**.

## Cómo ejecutar el notebook

### Opción 1: de forma local

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/jimena-benito-nacir/control-calidad-ionograma.git
   cd control-calidad-ionograma
   ```

2. (Opcional) Crear y activar un entorno virtual:

   ```bash
   python -m venv venv
   source venv/bin/activate        # En Windows: venv\Scripts\activate
   ```

3. Instalar las dependencias:

   ```bash
   pip install -r requirements.txt
   ```

4. Abrir el notebook:

   ```bash
   jupyter notebook TP_Final_Grupo35_Control_Calidad_Interno_Ionograma.ipynb
   ```

5. Ejecutar todas las celdas en orden. El notebook lee el archivo desde la carpeta `data/`; si no lo encuentra, lo descarga desde este repositorio.

### Opción 2: en Google Colab

1. Abrir el notebook en Colab.
2. Ejecutar todas las celdas (*Entorno de ejecución → Ejecutar todas*).

El archivo de datos se descarga automáticamente desde la carpeta `data/` de este repositorio, por lo que no es necesario subirlo manualmente.

Los resultados son reproducibles: los modelos y la simulación utilizan semillas fijas (`random_state=42` y `default_rng(0)`).

## Requisitos

- Python 3.10 o superior
- pandas
- numpy
- matplotlib
- scikit-learn
- openpyxl (lectura del archivo Excel)
- jinja2 (formato de tablas)
- jupyter (solo para ejecución local)

## Limitaciones

- Datos de un único equipo y nivel de control, durante aproximadamente dos años.
- Cada modelo de Isolation Forest se entrena con 20 mediciones por analito y pool.
- No se dispone de una clasificación independiente de errores analíticos, por lo que no se calcularon métricas como precisión o recall.

El detalle se presenta en la sección 16 del notebook.

## Bibliografía

- Westgard, J. O., Barry, P. L., Hunt, M. R., y Groth, T. (1981). A multi-rule Shewhart chart for quality control in clinical chemistry. *Clinical Chemistry, 27*(3), 493–501.
- Liu, F. T., Ting, K. M., y Zhou, Z.-H. (2008). Isolation Forest. *2008 Eighth IEEE International Conference on Data Mining*, 413–422.
- NIST/SEMATECH. *e-Handbook of Statistical Methods*: EWMA y CUSUM Control Charts.

La bibliografía completa se encuentra en la sección 18 del notebook.
