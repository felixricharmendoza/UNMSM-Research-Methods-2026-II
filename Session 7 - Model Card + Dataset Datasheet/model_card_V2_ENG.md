# Model Card — Aegis Content-Safety Input Guardrail (educational use)

## Model details
- **Name / version:** AegisSafety-TFIDF-LR v0.1
- **Owner / contact:** FELIX RICHAR MENDOZA PUMACAHUA <felix.mendozap@unmsm.edu.pe>
- **Date:** 2026-10-30
- **Architecture:** TF-IDF (word 1–2-grams, min_df=2, max 50,000 features, sublinear tf) + L2 logistic regression (C=1.0); binary output safe/unsafe with 0.5 probability threshold; scikit-learn 1.6.1, Python 3.13.18
- **License:** Code: MIT (proposed by the author). Data: CC BY 4.0 per the Hugging Face dataset card (verify); access is gated behind NVIDIA's terms.

## Intended use
- **Primary intended uses:** Course exercise (Research Methods, UNMSM) to practise documentation of a content-safety classifier. Research baseline for the fast first layer of a two-layer input guardrail (Moraes et al., 2026) in Big Data QA systems.
- **Out-of-scope / misuse:** NOT a production safety filter and NOT for moderating real users or making decisions about people. NOT a jailbreak detector: Aegis labels harmful CONTENT, not jailbreak TECHNIQUES (role-play, obfuscation, prompt injection). English only. A bag-of-words model is trivially evaded by paraphrasing, misspelling or encoding.
- **Intended users:** Doctoral students in this course; reviewers of this exercise.

## Factors
- **Relevant groups / conditions:** Risk category of the text (Aegis taxonomy: 13 critical categories plus 'Needs Caution'); text type (user prompt vs LLM response); language (English only); origin (red-team prompts, adversarial by construction); annotator disagreement (variable number of annotations per item). Identity groups mentioned in the text are NOT annotated, so identity-term bias cannot be measured with this dataset (UNKNOWN — to investigate).
- **Evaluation factors:** Reported overall, per class, per risk category (recall of 'unsafe') and per text type.

## Metrics
- **Performance measures:** Accuracy, macro-F1 and F1/precision of the 'unsafe' class (classes are imbalanced, so accuracy alone misleads); AUPRC of the 'unsafe' probability (threshold-free); per-class recall. Recall of 'unsafe' = share of harmful texts that the guardrail stops.
- **Decision thresholds:** 0.5 on the predicted probability (no threshold tuning).
- **Variation approach:** Mean ± std (ddof=1) over 5 random seeds; each seed changes the stratified 80/20 split of Aegis 'train' and the model's random_state.

## Evaluation data
- **Datasets:** (a) 20% stratified held-out split of Aegis 'train', re-drawn for each seed; (b) the official Aegis 'test' split (1199 items; unsafe: 792 (66.1%), safe: 407 (33.9%)), fixed across seeds.
- **Preprocessing:** Binary labels derived from annotator votes with the repo rule (safe if >= 2 'Safe' votes). TF-IDF vocabulary fit on the training split only (no leakage into test).

## Training data
- **Datasets:** 80% of Aegis 1.0 'train' (10798 items; classes: unsafe: 7248 (67.1%), safe: 3550 (32.9%)).
- **Known limitations:** Labels depend on the binarisation rule: repo rule (>=2 'Safe' votes -> safe) vs strict majority of present annotations disagree on 13 of 10798 train items (0.1%); 0 items have a single annotation that says 'Safe' but the repo rule labels them 'unsafe'; 0 items are 'unsafe' under the repo rule although no annotator assigned any risk category. Annotations per item: 2 annotation(s): 12, 3 annotation(s): 9751, 4 annotation(s): 1031, 5 annotation(s): 4. Prompts and LLM responses are mixed in one pool (combined: 3147, llm_response: 3131, user_message: 3099, multi_turn: 1421). Risk categories are highly imbalanced (Criminal Planning/Confessions: 2482, Needs Caution: 2098, Hate/Identity Hate: 1253, Harassment: 1034, Violence: 958, PII/Privacy: 757, Sexual: 632, Profanity: 550, Controlled/Regulated Substances: 506, Guns and Illegal Weapons: 399, Other: 188, Suicide and Self Harm: 115, Threat: 101, Sexual (minor): 63, Deception/Fraud: 2, Fraud/Deception: 1, Graphic: 1, Fraud/deception: 1, acting without consent (possible harassment): 1).

## Quantitative analyses
- **Overall results:** Held-out: accuracy=0.772 ± 0.005, macro-F1=0.687 ± 0.007, F1 unsafe=0.851 ± 0.003, AUPRC=0.920 ± 0.004 (majority-class accuracy for reference: 0.671 ± 0.000). Official test: accuracy=0.773 ± 0.004, macro-F1=0.698 ± 0.008, F1 unsafe=0.849 ± 0.002, AUPRC=0.912 ± 0.002 (majority-class accuracy: 0.661 ± 0.000). Best guardrail in the big-qa-architecture repo on Aegis: Chain BERT → Llama-Guard (paper headline) (F1 unsafe 0.6287, zero-shot on Aegis train); this model: F1 unsafe 0.849 ± 0.002 on the official test split. Different evaluation sets — not a like-for-like comparison.
- **Disaggregated results:** per-class recall (held-out): unsafe: 0.965 ± 0.003, safe: 0.380 ± 0.009; per-class recall (official test): unsafe: 0.962 ± 0.004, safe: 0.406 ± 0.015. Recall of 'unsafe' by risk category (official test): Needs Caution (n=194): 0.924 ± 0.007; Profanity (n=50): 0.936 ± 0.022; Hate/Identity Hate (n=137): 0.953 ± 0.004; PII/Privacy (n=73): 0.964 ± 0.008; Controlled/Regulated Substances (n=66): 0.970 ± 0.011; Criminal Planning/Confessions (n=298): 0.970 ± 0.004; Violence (n=104): 0.983 ± 0.008; Harassment (n=116): 0.986 ± 0.008; Sexual (n=53): 0.992 ± 0.010; Guns and Illegal Weapons (n=45): 1.000 ± 0.000. Categories with fewer than 20 unsafe test items were not reported: Other (n=13), Sexual (minor) (n=5), Suicide and Self Harm (n=10), Threat (n=12), (no category; unsafe only by the repo rule) (n=0). By text type: llm_response: macro-F1 0.641 ± 0.015, unsafe recall 0.973 ± 0.007 (n=330); user_message: macro-F1 0.708 ± 0.021, unsafe recall 0.943 ± 0.002 (n=359); combined: macro-F1 0.713 ± 0.005, unsafe recall 0.954 ± 0.004 (n=332); multi_turn: macro-F1 0.725 ± 0.029, unsafe recall 0.985 ± 0.009 (n=178).

## Ethical considerations
- **Risks:** False negatives let harmful requests reach the LLM and the knowledge base; false positives over-block legitimate questions and may fall disproportionately on texts that mention identity groups or sensitive topics (common in toxicity classifiers; not measured here). The training data contain offensive content. A transparent linear model can be probed to find evasions (dual use).
- **Mitigations:** Educational use only; no deployment; examples are never printed. Any real use would need a second, stronger layer (e.g., an LLM guardrail), an identity-term bias audit, adversarial testing and human review (UNKNOWN — to investigate).

## Caveats and recommendations
- **Caveats:** In-distribution evaluation only (same source and taxonomy as training). Results will NOT transfer to real QA traffic, other languages or jailbreak-style attacks. No hyper-parameter tuning; the comparison with the repo's guardrails uses different evaluation sets.
- **Recommendations:** Evaluate the repo's guardrails on the same official test split; test out-of-distribution on TrustAIRLab in-the-wild jailbreak prompts; check calibration and tune the threshold for the first-layer role (high precision); report results under the strict-majority label rule; run an identity-term bias audit before any claim of usefulness.
