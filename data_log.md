# Data log — decisiones del proyecto

Registro de las decisiones tomadas durante el proyecto, su motivo y su consecuencia. Las secciones indicadas corresponden al notebook.

| # | Etapa | Decisión | Motivo | Evidencia / consecuencia |
|---|---|---|---|---|
| 1 | Datos | Reconstruir una serie cronológica continua por analito a partir de los bloques de columnas de cada hoja | La planilla estaba paginada para impresión (bloques «LIBRO — PAG») | Se conservan `hoja_origen`, `pagina_origen` y `fila_excel` para trazar cada registro hasta la planilla original (5.2) |
| 2 | Datos | Usar los datos con autorización de la dirección técnica y publicar solo el material de control | Privacidad y uso institucional | El archivo no contiene identificadores de pacientes |
| 3 | Calidad | Excluir 3 duplicados de transcripción (Cl 03/12/2024; Na y Cl 18/08/2026) | Confirmados mediante cotejo con los registros originales | 1.193 → 1.190 registros (6.4) |
| 4 | Calidad | Conservar las demás mediciones repetidas del mismo día | Corresponden a repeticiones reales, por ejemplo, alrededor de calibraciones | Se analizan en su secuencia (6.4) |
| 5 | Calidad | Excluir el registro de K del 29/07/2026 | Error de carga sin medición real asociada, confirmado con el laboratorio | 1.190 → 1.189 registros (6.5) |
| 6 | Calidad | Corregir en el archivo fuente Cl 140,4 → 104,4 mmol/L (12/11/2025) | Transposición de dígitos confirmada | Deja de ser un valor atípico (z = −0,71) |
| 7 | Calidad | Conservar Cl 91,0 mmol/L (15/07/2025, pool 5) | Medición real, previa a una intervención sobre los electrodos | Se interpreta en la revisión del pool 5 (8.3.5) |
| 8 | Calidad | No imputar resultados en fechas o períodos sin registro | Los períodos sin registros corresponden a cierres por vacaciones, según la dirección técnica; el intervalo del 21/09/2024 al 30/10/2024 queda entre los pools 1 y 2. Imputar inventaría controles no realizados | Las secuencias se evalúan sobre las mediciones disponibles (6.3) |
| 9 | Calidad | Unificar la escritura de los comentarios de eventos | Variantes de un mismo evento («cambio de electrodos») | Permite contar eventos de forma consistente (6.6) |
| 10 | Preparación | Definir un pool nuevo solo con el comentario «Pool nuevo» | Es el único evento que cambia el material físico | «Calibro equipo» y «Kit nuevo diestro» no reinician la referencia; 11 pools (7) |
| 11 | Preparación | Línea de base = primeras 20 mediciones de cada analito y pool | Práctica habitual de control de calidad interno | 9 pools con línea de base; pools 6 y 11 excluidos por tener menos de 20 mediciones (7.1) |
| 12 | Preparación | Conservar el pool 1 solo para la caracterización descriptiva | No tiene mediciones posteriores a la línea de base | 8 pools × 3 analitos = 24 series comparadas |
| 13 | Preparación | No excluir valores de la línea de base por su distancia a la media | Excluirlos reduciría artificialmente la DE y aumentaría las señales posteriores | Los cambios de nivel se documentan como limitación (8.4) |
| 14 | Validación | Separación cronológica y no aleatoria: referencia (1-20) y seguimiento (21 en adelante) | El control de calidad es secuencial y cada pool se usa en orden; un split aleatorio filtraría información futura | 480 mediciones de referencia y 592 de seguimiento |
| 15 | Modelado | Excluir R₄s de Westgard | Requiere dos niveles de control en la misma corrida y se dispone de uno | Se usan 1₃s, 2₂s, 4₁s y 10x; 1₂s solo como advertencia (9.1) |
| 16 | Modelado | Un modelo de Isolation Forest por analito y pool, con el valor como única variable | Cada pool es un material distinto; no hay otras variables por medición | 24 modelos, 200 árboles, `random_state=42` (10) |
| 17 | Modelado | Mantener `contamination="auto"` en la comparación principal y evaluar también el ajuste al 5 % | Evitar ajustar el umbral con los mismos datos evaluados | La simulación de 10.5 muestra la sensibilidad al umbral |
| 18 | Modelado | EWMA con λ = 0,20 y L = 3; CUSUM con k = 0,5 y h = 5, sin reinicio | Parámetros de referencia habituales (NIST/SEMATECH) | Sugeridos en la devolución de la preentrega (12 y 13) |
| 19 | Modelado | No usar modelos supervisados (Random Forest, XGBoost) | No existen mediciones clasificadas como normales o anómalas | Se justifica en 2.5 |
| 20 | Evaluación | Usar Westgard como comparador y no como verdad de referencia | Usarlo como etiqueta favorecería a los métodos parecidos a él | En los datos reales se reportan frecuencias y coincidencias (11 y 14) |
| 21 | Evaluación | Estimar recall, precisión, F1, AUC y falsas alarmas con errores simulados | Es el método habitual para evaluar reglas de control de calidad; la verdad de referencia se conoce por construcción | 24 series reales × 50 repeticiones = 1.200 corridas por escenario (16) |
| 22 | Evaluación | Simular con la media, la DE y la resolución de cada serie real, y reutilizar el mismo ruido en todos los escenarios | Realismo de escala y comparación justa entre métodos sobre los mismos datos | `default_rng(2026)`; resultados reproducibles |
| 23 | Evaluación | Calcular precisión y F1 con una prevalencia fija del 33 % | La precisión depende de la prevalencia, que en una simulación se elige por diseño | Se informa junto con la tasa de falsas alarmas, que no depende de la prevalencia |
| 24 | Evaluación | Incluir la combinación Westgard + CUSUM | Es la forma práctica de incorporar CUSUM sin abandonar la regla vigente | Recall 0,78 frente a 0,49 ante sesgo de 1,5 DE; 5,4 % de falsas alarmas (16.5) |
| 25 | Interpretación | Explicar la baja detección de Isolation Forest al 5 % | Con una única variable, un valor fuera del rango de entrenamiento sigue el mismo camino que el extremo de la línea de base y recibe su mismo puntaje | Demostración en 16.7: +2 DE y +10 DE tienen igual puntaje |
| 26 | Selección | Mantener Westgard; descartar Isolation Forest; proponer CUSUM como complemento candidato | Criterios fijados en 2.3: detección y falsas alarmas | Ningún método cumple plenamente ambos criterios; la elección es provisional (16.9) |
| 27 | Negocio | Expresar la carga operativa como mediciones estables señaladas por mes bajo la simulación | No se dispone del costo de cada repetición ni de episodios observados | No equivale a episodios independientes ni a repeticiones realizadas; multiplicado por el costo de una repetición da una cota superior (16.9) |
| 28 | Interpretación | No equiparar una medición simulada no señalada con una corrida aceptada | En la práctica intervienen la revisión bioquímica y el contexto de cada corrida | El 51 % → 22 % se informa como detección simulada, no como reducción demostrada del riesgo (16.5 y 16.9) |
| 29 | Ética | Mantener la decisión de liberar resultados en el bioquímico | Ningún método reemplaza la validación profesional; riesgo de fatiga de alertas | Se recomienda una evaluación prospectiva documentada (17 y 18.2) |
