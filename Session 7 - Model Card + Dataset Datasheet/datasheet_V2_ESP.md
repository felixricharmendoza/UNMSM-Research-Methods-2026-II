# Datasheet — NVIDIA Aegis AI Content Safety Dataset 1.0 (uso educativo)

## Motivación
- **Propósito:** Creado para entrenar y evaluar modelos de moderación de seguridad de contenido de LLM frente a una taxonomía amplia de riesgos, incluido el ensamble AEGIS de expertos LLM de seguridad (Ghosh et al., 2024).
- **Creado por / financiado por:** NVIDIA (Ghosh, Varshney, Galinkin, Parisien — verificar la lista de autores en arXiv:2404.05993). Financiamiento: interno de NVIDIA; subvención o financiamiento externo DESCONOCIDO — por investigar.

## Composición
- **Qué representan las instancias:** Una fila por texto, que es un prompt de usuario o una respuesta de LLM (combined: 3147, llm_response: 3131, user_message: 3099, multi_turn: 1421). Tamaño de los splits: 10798 train / 1199 test. Cada fila tiene hasta 5 columnas de anotación (labels_0, labels_1, labels_2, labels_3, labels_4); cada anotación es 'Safe' o una o más categorías de riesgo. Anotaciones por texto: 2 anotación(es): 12, 3 anotación(es): 9751, 4 anotación(es): 1031, 5 anotación(es): 4. Contraste con fuentes en esta corrida: train 10798 vs tarjeta 10798: COINCIDE; test 1199 vs tarjeta 1199: COINCIDE; seguro/inseguro 3550/7248 vs salida del repo 3550/7248: COINCIDE; columnas de anotación 5 vs comentario del código del repo 4: NO COINCIDE (el propio código lee labels_0..labels_4).
- **Etiquetas:** Etiquetas por anotador de la taxonomía de Aegis (13 categorías de riesgo críticas más 'Needs Caution'; verificar la lista en la tarjeta del dataset). Frecuencia de categorías en train (asignadas por >= 1 anotador): Criminal Planning/Confessions: 2482, Needs Caution: 2098, Hate/Identity Hate: 1253, Harassment: 1034, Violence: 958, PII/Privacy: 757, Sexual: 632, Profanity: 550, Controlled/Regulated Substances: 506, Guns and Illegal Weapons: 399, Other: 188, Suicide and Self Harm: 115, Threat: 101, Sexual (minor): 63, Deception/Fraud: 2, Fraud/Deception: 1, Graphic: 1, Fraud/deception: 1, acting without consent (possible harassment): 1. Etiqueta binaria usada aquí = regla del repo (seguro si >= 2 votos 'Safe'): inseguro: 7248 (67.1%), seguro: 3550 (32.9%) (desbalanceada). Los creadores no proporcionan una etiqueta binaria oficial.
- **Muestra vs población:** Prompts en inglés tomados de los datos de red-teaming de inocuidad HH-RLHF de Anthropic, más respuestas generadas por un LLM (Mistral-7B-v0.1 según la tarjeta del dataset — verificar). Adversarial por construcción; NO es representativo del tráfico real de usuarios, de otros idiomas ni de preguntas de QA sobre documentos empresariales.
- **Datos faltantes:** 4 textos vacíos en train. Es esperable que haya espacios de anotación vacíos porque los textos tienen un número variable de anotadores (2 anotación(es): 12, 3 anotación(es): 9751, 4 anotación(es): 1031, 5 anotación(es): 4); el motivo por el que algunos textos tienen menos anotaciones es DESCONOCIDO — por investigar.

## Proceso de recolección
- **Cómo se recolectó:** Prompts muestreados de Anthropic HH-RLHF (harmless-base, conversaciones de red-teaming); respuestas de LLM generadas por los creadores; cada texto fue etiquetado por anotadores humanos capacitados siguiendo la taxonomía y las guías de Aegis (Ghosh et al., 2024 — verificar el número de anotadores y el procedimiento de control de calidad).
- **Periodo:** Publicado en 2024 junto con arXiv:2404.05993 (abril de 2024). Periodo de anotación DESCONOCIDO — por investigar.
- **Consentimiento / ética:** Los prompts de HH-RLHF fueron escritos por trabajadores colaborativos (crowdworkers) contratados por Anthropic; sus términos de consentimiento son los de la recolección original (DESCONOCIDO — por investigar). Los anotadores estuvieron expuestos a contenido dañino; las medidas de bienestar se describen en el artículo (verificar). Aprobación de un comité de ética: DESCONOCIDO — por investigar.

## Preprocesamiento / limpieza / etiquetado
- **Pasos:** Filtrado, deduplicación y tratamiento de PII por parte de los creadores: DESCONOCIDO — por investigar. En este proyecto: (1) etiqueta binaria derivada con la regla de src/dataset.py de big-qa-architecture; (2) auditoría de la regla de etiquetado: la regla del repo (>=2 votos 'Safe' -> seguro) y la mayoría estricta de las anotaciones presentes discrepan en 13 de 10798 textos de train (0.1%); 0 textos tienen una única anotación 'Safe' pero la regla del repo los etiqueta 'unsafe'; 0 textos son 'unsafe' según la regla del repo aunque ningún anotador les asignó una categoría de riesgo; (3) el comentario del código del repo menciona labels_0..labels_3 pero el código lee labels_0..labels_4; (4) atributos TF-IDF al momento del entrenamiento (ajustados solo con train).
- **Datos crudos disponibles:** Se publican las etiquetas por anotador (columnas labels_*), de modo que se pueden aplicar otras reglas de agregación. Las conversaciones completas de HH-RLHF son públicas por separado (Anthropic, licencia MIT — verificar). Guías completas para anotadores: DESCONOCIDO — por investigar.

## Usos
- **Usos apropiados:** Entrenar y comparar clasificadores de seguridad de contenido y guardrails de LLM; investigación sobre el desacuerdo entre anotadores y la agregación de etiquetas; enseñanza de documentación de IA responsable.
- **No debe usarse para:** Entrenar modelos que GENEREN contenido dañino; afirmar desempeño en detección de jailbreaks sin pruebas fuera de distribución (las etiquetas describen categorías de daño, no técnicas de ataque); perfilar o tomar decisiones sobre personas reales; generalizar a texto que no esté en inglés.

## Distribución
- **Cómo se distribuye:** Hugging Face Hub, con acceso restringido (los usuarios deben aceptar los términos de NVIDIA): https://huggingface.co/datasets/nvidia/Aegis-AI-Content-Safety-Dataset-1.0. Artículo del dataset: Ghosh et al. (2024), arXiv:2404.05993.
- **Licencia / restricciones:** CC BY 4.0 según la tarjeta del dataset (verificar): se requiere atribución; acceso restringido; la tarjeta advierte que los datos contienen contenido ofensivo.

## Mantenimiento
- **Responsable:** NVIDIA, mediante la página del dataset en Hugging Face; existe un dataset sucesor (Aegis 2.0) (verificar). Compromiso formal de mantenimiento de la v1.0: DESCONOCIDO — por investigar.
- **Plan de actualización:** Ninguno declarado para la v1.0 en la página del dataset ni en el artículo (verificar).
