# Doctoral Program in Deep Tech, Artificial Intelligence & Emerging Technologies

## Research Methods and Scientific Integrity in AI and Advanced Technologies

**DELIVERABLE No. 7: Model Card & Datasheet**

**Course Instructor:** Dr. Loveleen Gaur

**Student:** Félix Richar Mendoza Pumacahua

---

## Descripción

### English

Documents a content-safety input guardrail on the Aegis AI Content Safety Dataset 1.0 (NVIDIA; 10,798 training texts and 1,199 test texts). The classifier is a lightweight TF-IDF + logistic regression model that labels each text as *safe* or *unsafe*, equivalent to the "fast first layer" of a two-layer input guardrail (Moraes et al., 2026). It follows the Model Card (Mitchell et al., 2019) and Datasheet for Datasets (Gebru et al., 2021) formats. It is a documentation exercise, not intended for real-world use.

- **Results (5 seeds, official test split, n = 1,199):**
  - Accuracy: 0.773 ± 0.004 (majority-class baseline: 0.661).
  - Macro-F1: 0.698 ± 0.008.
  - F1 of the *unsafe* class: 0.849 ± 0.002; AUPRC: 0.912 ± 0.002.
  - Recall per class: unsafe 0.962, safe 0.406.
  - Lowest recall of *unsafe* by risk category: Needs Caution 0.924 and Profanity 0.936.
- **Limitations:**
  - In-distribution evaluation only: English text, red-teaming data.
  - Not directly comparable with the zero-shot guardrails of the `big-qa-architecture` repository (best: BERT → Llama-Guard, F1 *unsafe* 0.6287).
  - Aegis labels harmful content, not jailbreak techniques, so this is not a jailbreak detector.
  - No identity-term bias audit and no threshold tuning.
- **Datasheet:** marks unanswered points as "UNKNOWN", such as ethics approval and the annotation period. It detects a discrepancy between the code comment of the repository (`labels_0..labels_3`) and the dataset (5 annotation columns).
- **What changed in version 2:** the documented project is now the Aegis guardrail (the first version documented a student-dropout classifier on UCI #697). The experiment notebook, the Model Card, and the Datasheet are available in English and Spanish.

Files: `model_card_V2_ENG.md`, `datasheet_V2_ENG.md`, and the notebook `Ejercicios_4_5_Guardrail_Aegis_ENG_v2.ipynb`. The Spanish versions are `model_card_V2_ESP.md`, `datasheet_V2_ESP.md`, and `Ejercicios_4_5_Guardrail_Aegis_ESP_v2.ipynb`. The written deliverable is `Deliverable_07_Model_Card_Datasheet_EN_v2.docx` (Spanish: `..._ES_v2.docx`).

### Español

Documenta un guardrail de entrada de seguridad de contenido sobre el Aegis AI Content Safety Dataset 1.0 (NVIDIA; 10 798 textos de entrenamiento y 1 199 de prueba). El clasificador es un modelo ligero TF-IDF + regresión logística que etiqueta cada texto como *seguro* o *inseguro*, equivalente a la "primera capa rápida" de un guardrail de entrada de dos capas (Moraes et al., 2026). Sigue los formatos Model Card (Mitchell et al., 2019) y Datasheet for Datasets (Gebru et al., 2021). Es un ejercicio de documentación, no está pensado para uso real.

- **Resultados (5 semillas, split de prueba oficial, n = 1 199):**
  - Accuracy: 0.773 ± 0.004 (la clase mayoritaria da 0.661).
  - Macro-F1: 0.698 ± 0.008.
  - F1 de la clase *unsafe*: 0.849 ± 0.002; AUPRC: 0.912 ± 0.002.
  - Recall por clase: unsafe 0.962, safe 0.406.
  - Menor recall de *unsafe* por categoría de riesgo: Needs Caution 0.924 y Profanity 0.936.
- **Limitaciones:**
  - Solo se evalúa dentro de la distribución: texto en inglés, datos de red-teaming.
  - No es directamente comparable con los guardrails zero-shot del repositorio `big-qa-architecture` (el mejor: BERT → Llama-Guard, F1 *unsafe* 0.6287).
  - Aegis etiqueta contenido dañino, no técnicas de jailbreak, así que no es un detector de jailbreaks.
  - No tiene auditoría de sesgo por términos de identidad ni ajuste de umbral.
- **Datasheet:** marca como "DESCONOCIDO" los puntos sin respuesta, como la aprobación ética y el periodo de anotación. Detecta una discrepancia entre el comentario del código del repositorio (`labels_0..labels_3`) y el dataset (5 columnas de anotación).
- **Qué cambió en la versión 2:** el proyecto documentado ahora es el guardrail sobre Aegis (la primera versión documentaba un clasificador de abandono estudiantil sobre UCI #697). El notebook del experimento, el Model Card y el Datasheet están disponibles en español y en inglés.

Archivos: `model_card_V2_ESP.md`, `datasheet_V2_ESP.md` y el notebook `Ejercicios_4_5_Guardrail_Aegis_ESP_v2.ipynb`. Las versiones en inglés son `model_card_V2_ENG.md`, `datasheet_V2_ENG.md` y `Ejercicios_4_5_Guardrail_Aegis_ENG_v2.ipynb`. El documento escrito es `Deliverable_07_Model_Card_Datasheet_ES_v2.docx` (inglés: `..._EN_v2.docx`).
