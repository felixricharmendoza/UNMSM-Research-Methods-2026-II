# Doctoral Program in Deep Tech, Artificial Intelligence & Emerging Technologies

## Research Methods and Scientific Integrity in AI and Advanced Technologies

**DELIVERABLE No. 7: Model Card & Datasheet**

**Course Instructor:** Dr. Loveleen Gaur

**Student:** Félix Richar Mendoza Pumacahua

---

## Descripción

### English

Documents a student-dropout classifier (gradient boosting) on the UCI #697 dataset, with 4,424 students from a Portuguese polytechnic institute. It follows the Model Card (Mitchell et al., 2019) and Datasheet for Datasets (Gebru et al., 2021) formats. It is a documentation exercise, not intended for real-world use.

- **Results (5 seeds):**
  - Accuracy: 0.777 ± 0.010 (majority-class baseline: 0.499).
  - Macro-F1: 0.709 ± 0.015.
  - Recall per class: Graduate 0.914, Dropout 0.763, Enrolled 0.423.
  - Dropout recall by gender: 0.737 and 0.791.
- **Limitations:**
  - Single institution.
  - Only applicable after the 2nd semester.
  - No fairness audit and no external validation.
- **Datasheet:** marks unanswered points as "UNKNOWN", such as missing-data handling and ethics approval. It detects a discrepancy between the 35 variables in the paper and the 36 in UCI.

Files: `model_card.md`, `datasheet.md` and the experiment notebook `Ejercicios_4_5_Dropout_UCI697.ipynb`. Available in English and Spanish.

### Español

Documenta un clasificador de abandono estudiantil (gradient boosting) sobre el dataset UCI #697, con 4 424 estudiantes de un instituto politécnico portugués. Sigue los formatos Model Card (Mitchell et al., 2019) y Datasheet for Datasets (Gebru et al., 2021). Es un ejercicio de documentación, no está pensado para uso real.

- **Resultados (5 semillas):**
  - Accuracy: 0.777 ± 0.010 (la clase mayoritaria da 0.499).
  - Macro-F1: 0.709 ± 0.015.
  - Recall por clase: Graduate 0.914, Dropout 0.763, Enrolled 0.423.
  - Recall de Dropout por género: 0.737 y 0.791.
- **Limitaciones:**
  - Es de una sola institución.
  - Solo se aplica después del 2.º semestre.
  - No tiene auditoría de equidad ni validación externa.
- **Datasheet:** marca como "UNKNOWN" los puntos sin respuesta, como el tratamiento de datos faltantes y la aprobación ética. Detecta una discrepancia entre las 35 variables del paper y las 36 de UCI.

Archivos: `model_card.md`, `datasheet.md` y el notebook del experimento `Ejercicios_4_5_Dropout_UCI697.ipynb`. Disponible en inglés y español.
