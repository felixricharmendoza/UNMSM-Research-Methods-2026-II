# Model Card — Student Dropout Classifier (educational use)

## Model details
- **Name / version:** StudentDropout-GBM v0.1
- **Owner / contact:** FELIX RICHAR MENDOZA PUMACAHUA <felix.mendozap@unmsm.edu.pe>
- **Date:** 2026-10-23
- **Architecture:** Gradient Boosting (200 trees, depth 3) on standardised features; scikit-learn 1.6.1, Python 3.13.16
- **License:** Code: MIT (proposed by the author). Data: CC BY 4.0 (UCI).

## Intended use
- **Primary intended uses:** Course exercise (Research Methods, UNMSM) to practise documentation of a student-dropout classifier. Research on early-warning methods.
- **Out-of-scope / misuse:** NOT for decisions about real students (admission, sanctions, funding, support allocation). Trained on one institution's historical records; no fairness audit and no external validation.
- **Intended users:** Doctoral students in this course; reviewers of this exercise.

## Factors
- **Relevant groups / conditions:** Dataset includes demographic and socioeconomic attributes (e.g., marital status, nationality, parents' qualifications, gender) and three macroeconomic indicators (unemployment, inflation, GDP). Degree programme (17 degrees) and cohort (2008/09–2018/19) are also predictors, so programme and cohort effects are entangled with the other factors.
- **Evaluation factors:** Reported overall, per class and by Gender (Dropout recall).

## Metrics
- **Performance measures:** Accuracy and macro-F1 (classes are imbalanced, so accuracy alone misleads); per-class recall.
- **Decision thresholds:** argmax of predicted class probabilities (no threshold tuning).
- **Variation approach:** Mean ± std (ddof=1) over 5 random seeds; each seed changes both the stratified split and the model's random_state.

## Evaluation data
- **Datasets:** 20% stratified held-out split of UCI #697 (4424 students), re-drawn for each seed.
- **Preprocessing:** StandardScaler fit on the training split only (no leakage into test).

## Training data
- **Datasets:** 80% of UCI #697 (4424 students, 36 features, classes: Graduate: 2209 (49.9%), Dropout: 1421 (32.1%), Enrolled: 794 (17.9%)).
- **Known limitations:** Single institution (Polytechnic Institute of Portalegre), historical, class imbalance (Graduate 50% / Dropout 32% / Enrolled 18% per Realinho et al., 2022). 12 features describe curricular units of the 1st and 2nd semesters, so the model is NOT an at-enrolment predictor: it can only be applied after those semesters. The dataset authors also report pairs of predictors with Pearson r > 0.7 (multicollinearity), which complicates interpretation of feature importance.

## Quantitative analyses
- **Overall results:** accuracy=0.777 ± 0.010, macro-F1=0.709 ± 0.015 (majority-class accuracy for reference: 0.499 ± 0.000)
- **Disaggregated results:** per-class recall: Dropout: 0.763 ± 0.023, Enrolled: 0.423 ± 0.029, Graduate: 0.914 ± 0.008. Dropout recall for Gender=0: 0.737 ± 0.014 (n_seeds=5); Dropout recall for Gender=1: 0.791 ± 0.047 (n_seeds=5)

## Ethical considerations
- **Risks:** Labelling students as 'likely to drop out' can become self-fulfilling or stigmatising; protected/sensitive attributes may encode structural inequality; false negatives leave at-risk students unsupported.
- **Mitigations:** Educational use only; no deployment; any real use would need a fairness audit, human review and consent/legal review (UNKNOWN — to investigate).

## Caveats and recommendations
- **Caveats:** Results will NOT transfer to other institutions, countries or cohorts. Single train/test protocol, no hyper-parameter tuning, no baseline beyond majority class.
- **Recommendations:** External validation, calibration check, subgroup fairness audit and a tuned baseline (e.g., regularised logistic regression) before any claim of superiority. Dataset authors suggest imbalance-aware methods (e.g., SMOTE, ADASYN, balanced bagging); not applied here.
