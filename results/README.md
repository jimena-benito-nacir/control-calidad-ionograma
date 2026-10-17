# Resultados

Tablas y figuras generadas por la sección 19 del notebook. Se regeneran al ejecutarlo completo.

## Datos reales

| Archivo | Contenido | Sección |
|---|---|---|
| `calidad_duplicados_excluidos.csv` | Duplicados de transcripción excluidos | 6.4 |
| `linea_base_parametros.csv` | Media, DE y CV % de cada línea de base | 8.1 |
| `westgard_resumen_por_serie.csv` | Alertas de cada regla por analito y pool | 9.3 |
| `isolation_forest_resumen_por_serie.csv` | Señales de Isolation Forest por analito y pool | 10.4 |
| `comparacion_westgard_if_por_serie.csv` | Coincidencias y señales exclusivas Westgard / Isolation Forest | 11.2 |
| `cuatro_metodos_resumen_por_serie.csv` | Señales de los cuatro métodos por analito y pool | 14.1 |
| `cuatro_metodos_resumen_global.csv` | Total y porcentaje de señales por método | 14.1 |
| `cuatro_metodos_coincidencias.csv` | Combinaciones de métodos que señalan cada medición | 14.2 |
| `senales_por_medicion.csv` | Detalle de las 592 mediciones de seguimiento y sus señales | 14 |
| `fig_frecuencia_senales.png` | Porcentaje de mediciones señaladas por método | 14.1 |
| `fig_k_pool7_levey_jennings.png` | Carta de control de K del pool 7 | 15.2 |

## Evaluación con errores simulados

| Archivo | Contenido | Sección |
|---|---|---|
| `simulacion_falsas_alarmas.csv` | Falsas alarmas por medición y por corrida, y mediciones estables señaladas esperadas por mes | 16.4 |
| `simulacion_metricas_deteccion.csv` | Recall, precisión, F1, episodios detectados y mediciones hasta la señal | 16.5 |
| `simulacion_auc.csv` | AUC-ROC de los puntajes continuos | 16.6 |
| `fig_simulacion_errores.png` | Detección frente a falsas alarmas y según la magnitud del sesgo | 16.8 |
