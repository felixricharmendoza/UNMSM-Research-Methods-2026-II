# Datasheet — UCI #697 Predict Students' Dropout and Academic Success (educational use)

## Motivation
- **Purpose:** Built to train machine-learning models that estimate each student's risk of dropping out or failing; the models feed a Learning Analytics tool that helps tutors target support (Realinho et al., 2022; Martins et al., 2021).
- **Created by / funded by:** Valentim Realinho, Jorge Machado, Luís Baptista, Mónica V. Martins — Polytechnic Institute of Portalegre (Portugal). Funding (per UCI): programme SATDAP (Capacitação da Administração Pública), grant POCI-05-5762-FSE-000191.

## Composition
- **What instances represent:** One row per student: 4424 students, 36 predictors in the downloaded table. Predictor groups per the paper: demographic, socioeconomic, macroeconomic (unemployment, inflation, GDP), academic at enrolment, and academic performance at the end of the 1st and 2nd semesters. Attribute count: the paper reports 35 attributes (34 predictors + target) while UCI lists 36 features (UNKNOWN — to investigate the difference). Cross-check run: records 4424 vs paper 4424: OK; class counts OK; attributes incl. target 37 vs paper 35: MISMATCH (UCI lists 36 features, paper 34 predictors + target).
- **Labels:** Dropout / Enrolled / Graduate, defined by the student's status at the end of the normal course duration (3 years; 4 for Nursing). Dropout is defined broadly: a change of course or institution counts as dropout. Counts in the downloaded table: Graduate: 2209 (49.9%), Dropout: 1421 (32.1%), Enrolled: 794 (17.9%) (imbalanced).
- **Sample vs population:** Students of 17 undergraduate degrees at ONE polytechnic institute, enrolled 2008/09–2018/19; NOT representative of other institutions or countries. Outcome rates vary strongly by degree.
- **Missing data:** None in the downloaded table (0 missing values). The paper reports 'rigorous preprocessing' of missing values; the method used (e.g., imputation) is UNKNOWN — to investigate.

## Collection process
- **How collected:** Merged from four sources: the institution's Academic Management System (raw export of 13,992 rows × 398 columns), its Support System for the Teaching Activity (PAE), national admission data (DGES, via CNAES), and macroeconomic data from PORDATA entered manually (Realinho et al., 2022).
- **Timeframe:** Academic years 2008/2009 to 2018/2019 (after the Bologna process); donated to UCI on 2021-12-12.
- **Consent / ethics:** Data are anonymised; privacy aspects were validated by the institution's Data Protection Officer under GDPR. The paper lists informed consent as 'not applicable'. Ethics-committee approval: UNKNOWN — to investigate.

## Preprocessing / cleaning / labeling
- **Steps:** Creators removed records from discontinued courses and Erasmus enrolments, removed duplicates, and handled anomalies, outliers and missing values (details UNKNOWN — to investigate). Labelling per the outcome rule above. We additionally apply StandardScaler at train time (fit on the train split only).
- **Raw data available:** Only the processed table is public (UCI, Zenodo). Availability of the raw AMS/PAE/DGES extracts: UNKNOWN — to investigate (the paper does not say they are released).

## Uses
- **Appropriate uses:** Comparative studies of student academic performance, benchmarking and teaching of machine learning (uses suggested by the dataset authors), and early-warning research with a fairness review.
- **Should NOT be used for:** Decisions about individual students (admission, sanctions, funding) without a fairness audit and human review; generalising to other institutions or countries; prediction at enrolment time (12 predictors only exist after the 1st/2nd semesters).

## Distribution
- **How distributed:** Public via the UCI Machine Learning Repository (ID 697, DOI 10.24432/C5MC89) and Zenodo (doi 10.5281/zenodo.5777339). Dataset paper: Realinho et al., Data 7(11):146, DOI 10.3390/data7110146.
- **License / restrictions:** Creative Commons Attribution 4.0 (CC BY 4.0): attribution required.

## Maintenance
- **Maintainer:** Hosted by UCI and Zenodo; the creators' own maintenance commitment is UNKNOWN — to investigate. The institution uses the derived models in an internal Learning Analytics tool.
- **Update plan:** None stated on the dataset page or in the paper.
