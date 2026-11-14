# Job Description Skill-Tagging: Model Evaluation and Improvement

This repository contains the complete evaluation and improvement study for the Priberry Machine Learning Intern assignment. The project evaluates an existing skill-tagging system across 50 job descriptions and 54 canonical skills, diagnoses systematic errors, and tests a controlled post-processing guardrail.

---

## 1. Project Overview

Automated recruitment systems rely on accurate skill extraction to match candidates with open roles. This project provides:

- An independent, verbatim-grounded benchmark of 227 skill targets across 50 job descriptions.
- A quantitative baseline evaluation of pre-computed model predictions.
- A detailed error breakdown classifying why predictions fail.
- A controlled post-processing experiment (M1 Canonical Guardrail) that increases Precision from 85.50% to 93.44% without any loss in recall.

---

## 2. Problem Statement

Extracting skills from unstructured job descriptions is challenging because text contains mandatory requirements, optional bonus skills, company background context, and negated tools. 

The goal of this assignment is to:
1. Establish a verified ground truth with clear separation between required and preferred qualifications.
2. Evaluate original model predictions using Micro and Macro Precision, Recall, and F1-Scores.
3. Identify systematic error patterns.
4. Implement and evaluate an evidence-based improvement.

---

## 3. Dataset and Vocabulary

The study uses two original datasets provided in the `data/` directory:

- `data/job-descriptions-tagged.csv`: 50 job descriptions covering 10 roles across 10 companies, with 200 pre-computed skill predictions in the `model_tags` column.
- `data/skill-list.csv`: The official canonical vocabulary of 54 standardized technical and professional skills.

---

## 4. Project Workflow

The project follows a 7-stage sequential pipeline:

1. **Data Collection & Audit**: Verify raw files and compute cryptographic checksums.
2. **Data Cleaning & Normalization**: Parse prediction strings, establish synonym mappings, and audit vocabulary coverage.
3. **Exploratory Data Analysis (EDA)**: Inspect text length distributions, prediction frequencies, and qualification wording.
4. **Feature Engineering & Ground Truth**: Construct independent ground truth with verbatim sentence evidence for Required and Preferred skills.
5. **Baseline Model Evaluation**: Compute Micro/Macro Precision, Recall, and F1-Scores against ground truth; diagnose failure modes.
6. **Controlled Model Improvement**: Implement the M1 Canonical Guardrail filter to remove out-of-vocabulary tags.
7. **Final Evaluation & Reporting**: Consolidate findings, compare baseline vs. improved system, and produce the final report.

---

## 5. Notebook Descriptions

All analysis is organized into 7 sequential notebooks in the `notebooks/` directory:

- `notebooks/01_data_collection.ipynb`: Loads source files, verifies schemas, inspects sample records, and checks SHA-256 data hashes.
- `notebooks/02_data_cleaning.ipynb`: Cleans description text, parses model predictions into structured lists, audits out-of-vocabulary tags, and formalizes synonym mappings.
- `notebooks/03_exploratory_data_analysis.ipynb`: Analyzes text lengths, prediction density per job, most and least frequent skills, and qualification cues.
- `notebooks/04_feature_engineering.ipynb`: Implements the ground-truth rubric, labels 227 target instances with verbatim text evidence, and validates data integrity.
- `notebooks/05_baseline_model_evaluation.ipynb`: Evaluates original predictions against ground truth, measures the required vs. preferred performance gap, and exports baseline diagnostics.
- `notebooks/06_model_improvement_experiment.ipynb`: Audits the black-box setup, applies the M1 Canonical Guardrail filter, and evaluates metric changes.
- `notebooks/07_final_model_evaluation.ipynb`: Synthesizes final comparisons, visualizes metrics, and generates the final submission report.

---

## 6. Evaluation Methodology

The evaluation uses standard information retrieval and multi-label classification metrics:

- **Target Tiers**:
  - Required Skills: 177 instances (mandatory core qualifications).
  - Preferred Skills: 50 instances (bonus or optional qualifications).
  - Total Targets: 227 instances.
- **Metrics**:
  - **Micro Precision, Recall, F1**: Global aggregation across all prediction instances.
  - **Macro Precision, Recall, F1**: Unweighted mean of per-job-description scores.
  - **Tiered Recall**: Separate recall measurements for Required vs. Preferred qualifications.

---

## 7. Baseline Results

The baseline evaluation of the original 200 model predictions against the 227 ground-truth targets revealed:

- **Micro Precision**: 85.50% (171 / 200)
- **Micro Recall**: 75.33% (171 / 227)
- **Micro F1-Score**: 80.09%
- **Macro Precision**: 87.31%
- **Macro Recall**: 74.53%
- **Macro F1-Score**: 78.71%
- **Tier Disparity**: Required skill recall was 85.31%, while preferred skill recall dropped to 40.00% (a 45.31 percentage point gap).

---

## 8. Controlled M1 Improvement Results

Because upstream training code was not provided, the intervention was designed as an external **Canonical Vocabulary Post-Processing Guardrail (M1)** that filters out predictions absent from the 54 official skills and approved synonyms:

| Metric | Original Baseline | M1 Guardrail | Change | Gain |
| :--- | :---: | :---: | :---: | :---: |
| **Micro Precision** | 85.50% | **93.44%** | **+7.94 pp** | **+9.29%** |
| **Micro Recall** | 75.33% | **75.33%** | **+0.00 pp** | 0.00% |
| **Micro F1-Score** | 80.09% | **83.41%** | **+3.32 pp** | **+4.15%** |
| **Macro Precision** | 87.31% | **94.73%** | **+7.42 pp** | **+8.50%** |
| **Macro Recall** | 74.53% | **74.53%** | **+0.00 pp** | 0.00% |
| **Macro F1-Score** | 78.71% | **81.67%** | **+2.96 pp** | **+3.77%** |
| **Required Recall** | 85.31% | **85.31%** | **+0.00 pp** | 0.00% |
| **Preferred Recall** | 40.00% | **40.00%** | **+0.00 pp** | 0.00% |
| **Correct Tags (TP)** | 171 | **171** | **0** | Kept 100% |
| **Wrong Tags (FP)** | 29 | **12** | **-17** | **-58.62%** |
| **Missed Skills (FN)** | 56 | **56** | **0** | Zero loss |

---

## 9. Key Findings and Error Patterns

Detailed error analysis itemized all 29 baseline false positives and 56 false negatives into three primary patterns:

1. **Missed Preferred Skills (30 instances, 53.57% of misses)**: The model fails to extract skills placed under secondary headings like "Nice to have:" or "Bonus:".
2. **Non-Canonical Soft Skills (16 instances, 55.17% of wrong tags)**: The model extracted general traits (Leadership, Teamwork, Problem Solving, Communication, Time Management) not present in the 54-skill list. The M1 guardrail eliminated 100% of these errors.
3. **Company Context Guesses (12 instances, 41.38% of wrong tags)**: The model guessed valid technical tools (e.g. SAP for Orbit Logistics, Kubernetes for Lumen Cloud) based on company domain rather than text evidence.

---

## 10. Limitations

- **Black-Box Environment**: Training code and model weights were not supplied, precluding internal fine-tuning.
- **Filter Scope**: The M1 guardrail filters invalid predictions; recovering the 56 missed skills requires updating the upstream text extraction model.
- **Single-Reviewer Ground Truth**: Ground truth was established by one expert annotator.
- **Low Preferred Recall**: A 40.00% preferred recall reduces effectiveness in candidate ranking scenarios.

---

## 11. Repository Structure

```text
skill-eval/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── job-descriptions-tagged.csv
│   └── skill-list.csv
├── notebooks/
│   ├── 01_data_collection.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_exploratory_data_analysis.ipynb
│   ├── 04_feature_engineering.ipynb
│   ├── 05_baseline_model_evaluation.ipynb
│   ├── 06_model_improvement_experiment.ipynb
│   └── 07_final_model_evaluation.ipynb
└── outputs/
    ├── final_submission_report.md
    ├── annotation_rubric.md
    ├── baseline_evaluation_summary.md
    ├── improvement_experiment_summary.md
    ├── evaluation_labels.csv
    ├── annotation_audit.csv
    ├── synonym_mapping.csv
    ├── per_jd_metrics.csv
    ├── overall_metrics.csv
    ├── error_analysis.csv
    ├── false_positive_analysis.csv
    ├── false_negative_analysis.csv
    ├── improvement_per_jd_metrics.csv
    ├── improvement_overall_metrics.csv
    ├── improvement_comparison.csv
    ├── improvement_error_analysis.csv
    ├── eda_dataset_summary.csv
    ├── eda_jd_statistics.csv
    ├── eda_oov_analysis.csv
    ├── eda_prediction_frequency.csv
    ├── eda_vocabulary_coverage.csv
    ├── eda_figures/
    ├── baseline_figures/
    ├── improvement_figures/
    └── final_figures/
```

---

## 12. Instructions to Run

### Setup Environment
```bash
# Clone the repository
git clone https://github.com/Yanshuptl18/priberry_ml.git
cd priberry_ml

# Install required dependencies
pip install -r requirements.txt
```

### Run Notebooks
Execute the notebooks in sequence inside Jupyter Lab or Jupyter Notebook:
1. `notebooks/01_data_collection.ipynb`
2. `notebooks/02_data_cleaning.ipynb`
3. `notebooks/03_exploratory_data_analysis.ipynb`
4. `notebooks/04_feature_engineering.ipynb`
5. `notebooks/05_baseline_model_evaluation.ipynb`
6. `notebooks/06_model_improvement_experiment.ipynb`
7. `notebooks/07_final_model_evaluation.ipynb`

All results and figures will be reproduced directly into the `outputs/` directory.
