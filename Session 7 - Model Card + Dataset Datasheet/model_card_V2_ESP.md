# Model Card — Guardrail de entrada de seguridad de contenido sobre Aegis (uso educativo)

## Detalles del modelo
- **Nombre / versión:** AegisSafety-TFIDF-LR v0.1
- **Responsable / contacto:** FELIX RICHAR MENDOZA PUMACAHUA <felix.mendozap@unmsm.edu.pe>
- **Fecha:** 2026-10-30
- **Arquitectura:** TF-IDF (1–2-gramas de palabras, min_df=2, máximo 50,000 atributos, tf sublineal) + regresión logística con regularización L2 (C=1.0); salida binaria seguro/inseguro con umbral de probabilidad 0.5; scikit-learn 1.6.1, Python 3.13.18
- **Licencia:** Código: MIT (propuesta por el autor). Datos: CC BY 4.0 según la tarjeta del dataset en Hugging Face (verificar); el acceso está restringido por los términos de NVIDIA.

## Uso previsto
- **Usos previstos principales:** Ejercicio del curso (Métodos de Investigación, UNMSM) para practicar la documentación de un clasificador de seguridad de contenido. Línea base de investigación para la primera capa rápida de un guardrail de entrada de dos capas (Moraes et al., 2026) en sistemas de preguntas y respuestas sobre Big Data.
- **Fuera de alcance / mal uso:** NO es un filtro de seguridad para producción y NO debe usarse para moderar a usuarios reales ni para tomar decisiones sobre personas. NO es un detector de jailbreaks: Aegis etiqueta CONTENIDO dañino, no TÉCNICAS de jailbreak (juego de roles, ofuscación, inyección de prompts). Solo inglés. Un modelo de bolsa de palabras se evade trivialmente con paráfrasis, errores ortográficos o codificación.
- **Usuarios previstos:** Estudiantes de doctorado de este curso; revisores de este ejercicio.

## Factores
- **Grupos / condiciones relevantes:** Categoría de riesgo del texto (taxonomía de Aegis: 13 categorías críticas más 'Needs Caution'); tipo de texto (prompt de usuario vs respuesta del LLM); idioma (solo inglés); origen (prompts de red-teaming, adversarios por construcción); desacuerdo entre anotadores (número variable de anotaciones por texto). Los grupos identitarios mencionados en el texto NO están anotados, por lo que el sesgo hacia términos de identidad no se puede medir con este dataset (DESCONOCIDO — por investigar).
- **Factores de evaluación:** Se reporta en total, por clase, por categoría de riesgo (recall de 'unsafe') y por tipo de texto.

## Métricas
- **Medidas de desempeño:** Exactitud (accuracy), macro-F1 y F1/precisión de la clase 'unsafe' (las clases están desbalanceadas, así que la exactitud sola engaña); AUPRC de la probabilidad de 'unsafe' (no depende del umbral); recall por clase. Recall de 'unsafe' = proporción de textos dañinos que el guardrail detiene.
- **Umbrales de decisión:** 0.5 sobre la probabilidad predicha (sin ajuste de umbral).
- **Enfoque de variación:** Media ± desviación estándar (ddof=1) sobre 5 semillas aleatorias; cada semilla cambia la partición estratificada 80/20 de 'train' de Aegis y el random_state del modelo.

## Datos de evaluación
- **Datasets:** (a) 20% reservado y estratificado del 'train' de Aegis, redibujado en cada semilla; (b) el split 'test' oficial de Aegis (1199 textos; inseguro: 792 (66.1%), seguro: 407 (33.9%)), fijo para todas las semillas.
- **Preprocesamiento:** Etiquetas binarias derivadas de los votos de los anotadores con la regla del repo (seguro si >= 2 votos 'Safe'). El vocabulario TF-IDF se ajusta solo con el split de entrenamiento (sin fuga de datos hacia el test).

## Datos de entrenamiento
- **Datasets:** 80% del 'train' de Aegis 1.0 (10798 textos en total; clases: inseguro: 7248 (67.1%), seguro: 3550 (32.9%)).
- **Limitaciones conocidas:** Las etiquetas dependen de la regla de binarización: la regla del repo (>=2 votos 'Safe' -> seguro) y la mayoría estricta de las anotaciones presentes discrepan en 13 de 10798 textos de train (0.1%); 0 textos tienen una única anotación 'Safe' pero la regla del repo los etiqueta 'unsafe'; 0 textos son 'unsafe' según la regla del repo aunque ningún anotador les asignó una categoría de riesgo. Anotaciones por texto: 2 anotación(es): 12, 3 anotación(es): 9751, 4 anotación(es): 1031, 5 anotación(es): 4. Los prompts y las respuestas del LLM están mezclados en un mismo conjunto (combined: 3147, llm_response: 3131, user_message: 3099, multi_turn: 1421). Las categorías de riesgo están muy desbalanceadas (Criminal Planning/Confessions: 2482, Needs Caution: 2098, Hate/Identity Hate: 1253, Harassment: 1034, Violence: 958, PII/Privacy: 757, Sexual: 632, Profanity: 550, Controlled/Regulated Substances: 506, Guns and Illegal Weapons: 399, Other: 188, Suicide and Self Harm: 115, Threat: 101, Sexual (minor): 63, Deception/Fraud: 2, Fraud/Deception: 1, Graphic: 1, Fraud/deception: 1, acting without consent (possible harassment): 1).

## Análisis cuantitativos
- **Resultados generales:** Reserva (hold-out): exactitud=0.772 ± 0.005, macro-F1=0.687 ± 0.007, F1 unsafe=0.851 ± 0.003, AUPRC=0.920 ± 0.004 (exactitud de la clase mayoritaria como referencia: 0.671 ± 0.000). Test oficial: exactitud=0.773 ± 0.004, macro-F1=0.698 ± 0.008, F1 unsafe=0.849 ± 0.002, AUPRC=0.912 ± 0.002 (exactitud de la clase mayoritaria: 0.661 ± 0.000). Mejor guardrail del repo big-qa-architecture en Aegis: Cadena BERT → Llama-Guard (resultado principal del paper) (F1 unsafe 0.6287, zero-shot sobre Aegis train); este modelo: F1 unsafe 0.849 ± 0.002 en el split test oficial. Conjuntos de evaluación distintos: no es una comparación equivalente.
- **Resultados desagregados:** recall por clase (reserva): inseguro: 0.965 ± 0.003, seguro: 0.380 ± 0.009; recall por clase (test oficial): inseguro: 0.962 ± 0.004, seguro: 0.406 ± 0.015. Recall de 'unsafe' por categoría de riesgo (test oficial): Needs Caution (n=194): 0.924 ± 0.007; Profanity (n=50): 0.936 ± 0.022; Hate/Identity Hate (n=137): 0.953 ± 0.004; PII/Privacy (n=73): 0.964 ± 0.008; Controlled/Regulated Substances (n=66): 0.970 ± 0.011; Criminal Planning/Confessions (n=298): 0.970 ± 0.004; Violence (n=104): 0.983 ± 0.008; Harassment (n=116): 0.986 ± 0.008; Sexual (n=53): 0.992 ± 0.010; Guns and Illegal Weapons (n=45): 1.000 ± 0.000. No se reportaron las categorías con menos de 20 textos unsafe en el test: Other (n=13), Sexual (minor) (n=5), Suicide and Self Harm (n=10), Threat (n=12), (sin categoría; unsafe solo por la regla del repo) (n=0). Por tipo de texto: llm_response: macro-F1 0.641 ± 0.015, recall de unsafe 0.973 ± 0.007 (n=330); user_message: macro-F1 0.708 ± 0.021, recall de unsafe 0.943 ± 0.002 (n=359); combined: macro-F1 0.713 ± 0.005, recall de unsafe 0.954 ± 0.004 (n=332); multi_turn: macro-F1 0.725 ± 0.029, recall de unsafe 0.985 ± 0.009 (n=178).

## Consideraciones éticas
- **Riesgos:** Los falsos negativos dejan pasar solicitudes dañinas hacia el LLM y la base de conocimiento; los falsos positivos bloquean en exceso preguntas legítimas y pueden recaer desproporcionadamente sobre textos que mencionan grupos identitarios o temas sensibles (algo común en clasificadores de toxicidad; no se midió aquí). Los datos de entrenamiento contienen contenido ofensivo. Un modelo lineal transparente puede analizarse para encontrar formas de evadirlo (doble uso).
- **Mitigaciones:** Uso solo educativo; sin despliegue; nunca se imprimen ejemplos. Cualquier uso real requeriría una segunda capa más robusta (p. ej., un guardrail basado en LLM), una auditoría de sesgo por términos de identidad, pruebas adversariales y revisión humana (DESCONOCIDO — por investigar).

## Salvedades y recomendaciones
- **Salvedades:** Evaluación solo dentro de la distribución (misma fuente y taxonomía que el entrenamiento). Los resultados NO se trasladarán a tráfico real de preguntas y respuestas, a otros idiomas ni a ataques tipo jailbreak. Sin ajuste de hiperparámetros; la comparación con los guardrails del repo usa conjuntos de evaluación distintos.
- **Recomendaciones:** Evaluar los guardrails del repo sobre el mismo split test oficial; probar fuera de distribución con los prompts de jailbreak reales de TrustAIRLab (in-the-wild); revisar la calibración y ajustar el umbral para el rol de primera capa (alta precisión); reportar resultados también con la regla de mayoría estricta; realizar una auditoría de sesgo por términos de identidad antes de afirmar utilidad alguna.
