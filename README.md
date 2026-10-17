# Control de calidad interno del ionograma

**Evaluación de Isolation Forest como complemento a las reglas de Westgard, con EWMA y CUSUM como referencias adicionales**

Proyecto Final Integrador — Diplomatura en Ciencia de Datos y Análisis Avanzado (UTN)
Grupo 35 — María Jimena Benito Nacir — Octubre de 2026

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jimena-benito-nacir/control-calidad-ionograma/blob/main/TP_Final_Grupo35_Control_Calidad_Interno_Ionograma.ipynb)

---

## Descripción

El laboratorio evalúa el control interno del ionograma (Na, K y Cl) mediante reglas de Westgard antes de liberar resultados de pacientes. Este proyecto analiza si **Isolation Forest**, un algoritmo de Machine Learning no supervisado, aporta información adicional útil como complemento de ese control.

Se comparan cuatro métodos sobre las mismas mediciones de seguimiento:

- **Reglas de Westgard** (1₃s, 2₂s, 4₁s y 10x): método actual del laboratorio.
- **Isolation Forest:** detección de mediciones atípicas, con dos ajustes del umbral.
- **EWMA y CUSUM:** métodos estadísticos de referencia para desviaciones sostenidas.

El trabajo combina dos evaluaciones:

1. **Datos reales:** comparación de señales y revisión bioquímica de casos seleccionados.
2. **Errores simulados:** errores de magnitud conocida introducidos en series con la media, la DE y la resolución de las 24 líneas de base reales. Permite calcular recall, precisión, F1, AUC y falsas alarmas, que no pueden calcularse sobre los datos reales por falta de una clasificación independiente.

El análisis es retrospectivo y sigue la metodología CRISP-DM.

## Pregunta del proyecto

¿Aporta Isolation Forest información adicional útil como complemento de las reglas de Westgard en el control interno del ionograma?

## Resultado principal

Todos los números salen del notebook y se guardan en `results/`.

**Datos reales** (592 mediciones de seguimiento, 24 series):

| Método | Mediciones señaladas | % |
|---|---:|---:|
| Westgard | 112 | 18,9 % |
| Isolation Forest | 356 | 60,1 % |
| EWMA | 97 | 16,4 % |
| CUSUM | 182 | 30,7 % |

**Errores simulados** (1.200 corridas por escenario):

| Método | Falsas alarmas (% de mediciones estables) | Mediciones estables señaladas por mes* (Na, K y Cl) | Recall ante sesgo de 1,5 DE | Recall ante sesgo de 2 DE | AUC ante sesgo de 2 DE |
|---|---:|---:|---:|---:|---:|
| Westgard | 1,8 % | 1,2 | 0,49 | 0,72 | 0,84 (\|z\|) |
| Isolation Forest (por defecto) | 49,1 % | 30,9 | 0,77 | 0,88 | 0,81 |
| Isolation Forest (5 %) | 5,0 % | 3,1 | 0,21 | 0,30 | 0,81 |
| EWMA | 1,9 % | 1,2 | 0,67 | 0,84 | 0,96 |
| CUSUM | 4,3 % | 2,7 | 0,76 | 0,86 | 0,97 |
| Westgard + CUSUM | 5,4 % | 3,4 | 0,78 | 0,88 | — |

### Conclusión

- **Isolation Forest no se recomienda** con esta configuración, de una única variable. Con el ajuste por defecto, señala la mitad de las mediciones estables (unas 31 por mes en la simulación). Con el ajuste al 5 %, pierde alrededor de la mitad de los episodios de sesgo, incluso de 3 DE, porque todo valor fuera del rango de entrenamiento recibe el mismo puntaje que el extremo de la línea de base. Su AUC no supera la distancia a la media (|z|), que Westgard ya utiliza.
- **Westgard se mantiene como regla principal.**
- **CUSUM se selecciona como complemento candidato** para desvíos sostenidos pequeños. Obtuvo el mayor AUC en todos los escenarios. Ante un sesgo simulado de 1,5 DE, la proporción de mediciones con error no señaladas baja del 51 % (Westgard) al 22 % (Westgard + CUSUM), con unas 2 mediciones estables señaladas más por mes. Es una mejora de la detección simulada, no una reducción demostrada del riesgo para pacientes. Se propone evaluarlo de forma prospectiva antes de su uso rutinario.

\* Número esperado de mediciones estables señaladas por mes (21 días hábiles × 3 analitos); no equivale a episodios independientes ni a repeticiones realizadas.

Las tasas simuladas corresponden a condiciones favorables (distribución normal, líneas de base estables). No reemplazan una evaluación prospectiva con episodios documentados.

## Estructura del repositorio

```text
.
├── README.md
├── TP_Final_Grupo35_Control_Calidad_Interno_Ionograma.ipynb   # notebook ejecutado de punta a punta
├── requirements.txt
├── data_log.md                    # registro de decisiones del proyecto
├── LICENSE                        # licencia del código
├── .gitignore
├── data/
│   ├── README.md                  # fuente, autorización y estructura
│   └── QC DATA Control interno de ionograma.xlsx
├── results/                       # tablas y figuras usadas en el informe
│   └── README.md
└── docs/
    ├── Pre-entrega_TPfinal_Grupo35.pdf
    └── Informe_Final_Grupo35_Benito_Nacir.pdf
```

## Flujo metodológico (CRISP-DM)

| Fase | Secciones del notebook | Salidas en `results/` |
|---|---|---|
| Comprensión del negocio | 1-2: problema, público, indicadores, criterios | — |
| Comprensión de los datos | 3, 5, 6 y 8: fuente, calidad, líneas de base | `calidad_duplicados_excluidos.csv`, `linea_base_parametros.csv` |
| Preparación | 5-7: reconstrucción de la serie, limpieza, pools, línea de base y seguimiento | — |
| Modelado | 9, 10, 12 y 13: Westgard, Isolation Forest, EWMA y CUSUM | `westgard_*.csv`, `isolation_forest_*.csv` |
| Evaluación con datos reales | 11, 14 y 15: coincidencias, señales exclusivas, revisión bioquímica | `comparacion_*.csv`, `cuatro_metodos_*.csv`, `senales_por_medicion.csv`, `fig_frecuencia_senales.png`, `fig_k_pool7_levey_jennings.png` |
| Evaluación con errores simulados | 16: falsas alarmas, recall, precisión, F1, AUC, selección del método | `simulacion_*.csv`, `fig_simulacion_errores.png` |
| Limitaciones, ética y uso potencial | 17-19 | — |

## Datos

`data/QC DATA Control interno de ionograma.xlsx` contiene el registro del control interno del ionograma del laboratorio, con tres hojas (una por analito), del 26/08/2024 al 31/08/2026. Se usa con autorización de la dirección técnica, corresponde únicamente al material de control y **no contiene información de pacientes**. Ver `data/README.md`.

## Cómo ejecutar

### Google Colab

1. Abrir el notebook con el botón **Abrir en Colab** de arriba.
2. Ejecutar **Entorno de ejecución → Ejecutar todas**.

El archivo de datos se descarga automáticamente desde la carpeta `data/` de este repositorio.

### Local

```bash
git clone https://github.com/jimena-benito-nacir/control-calidad-ionograma.git
cd control-calidad-ionograma
python -m venv .venv
# Windows: .venv\Scripts\activate
# Linux/Mac: source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook TP_Final_Grupo35_Control_Calidad_Interno_Ionograma.ipynb
```

Ejecutar todas las celdas en orden. **Tiempo aproximado: 7-9 minutos**; la simulación de la sección 16 ocupa la mayor parte, porque entrena 1.200 modelos de Isolation Forest.

Los resultados son reproducibles: los modelos y las simulaciones usan semillas fijas (`random_state=42`, `default_rng(0)`, `default_rng(1)` y `default_rng(2026)`). Al terminar, las tablas y figuras se regeneran en `results/`.

## Decisiones metodológicas clave

El detalle completo está en [`data_log.md`](data_log.md).

- **No se usa un split aleatorio train/test.** El control de calidad es secuencial: las primeras 20 mediciones de cada pool son la referencia o entrenamiento, y las posteriores, el seguimiento.
- **Westgard es comparador, no verdad de referencia.** Usarlo como etiqueta favorecería artificialmente a los métodos parecidos a él.
- **No se usan modelos supervisados** (Random Forest, XGBoost): no existen mediciones clasificadas como normales o anómalas.
- **Las métricas se calculan con errores simulados de magnitud conocida**, el método habitual para evaluar reglas de control de calidad, usando la media, la DE y la resolución de cada serie real.
- **Las líneas de base se conservan sin exclusiones por distancia a la media**: excluir valores reduciría artificialmente la DE y aumentaría las señales.

## Limitaciones

- Un único equipo y nivel de control, con menos de tres años de datos.
- Cada modelo de Isolation Forest se entrena con 20 mediciones por analito y pool.
- La simulación supone mediciones independientes y normales y líneas de base estables; no reproduce los cambios de nivel observados en los datos reales.
- La carga operativa se expresa como mediciones estables señaladas por mes bajo la simulación, no como episodios ni repeticiones observadas; no se dispone del costo de cada repetición.

El detalle se presenta en la sección 17 del notebook.

## Bibliografía

- Westgard, J. O., Barry, P. L., Hunt, M. R., y Groth, T. (1981). A multi-rule Shewhart chart for quality control in clinical chemistry. *Clinical Chemistry, 27*(3), 493–501. https://doi.org/10.1093/clinchem/27.3.493
- Liu, F. T., Ting, K. M., y Zhou, Z.-H. (2008). Isolation Forest. *2008 Eighth IEEE International Conference on Data Mining*, 413–422. https://doi.org/10.1109/ICDM.2008.17
- NIST/SEMATECH. *e-Handbook of Statistical Methods*: EWMA y CUSUM Control Charts.

La bibliografía completa se encuentra en la sección 20 del notebook.

## Licencia

El código se publica bajo licencia MIT (ver `LICENSE`). El archivo de datos pertenece al laboratorio y se publica con fines académicos con autorización de su dirección técnica; no está cubierto por la licencia MIT.
