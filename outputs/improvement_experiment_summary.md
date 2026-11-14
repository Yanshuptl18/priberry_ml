# Model Improvement Experiment Summary

This document describes the controlled experiment carried out to improve the skill-tagging system.

## 1. What We Found in the Original Setup

- **Model Code Availability**: The original model code and training scripts were not provided with the dataset. We only had the text and the pre-computed predictions.
- **Model Type**: This is a black-box system where we cannot retrain the internal model weights directly.
- **Improvement Strategy**: Because we cannot retrain the model, the best scientific method is to build a post-processing filter (called the M1 Canonical Guardrail).

## 2. The M1 Canonical Guardrail Experiment

- **The Idea**: Check every predicted tag against the official 54-skill list and approved synonyms. If a predicted tag is not in the official list, filter it out.
- **Goal**: Stop the model from predicting soft skills and unapproved terms (like Leadership, Teamwork, and generic "API") that always count as errors.
- **Safety**: This filter only removes tags that are outside the official vocabulary, so it cannot hurt recall on official skills.

## 3. Results: Baseline vs. M1 Improved

Filtering out non-official skills produced clear improvements across all main metrics:

- **Precision (Accuracy of Predictions)**:
  - Micro Precision improved from 85.50% to **93.44%** (+7.94 percentage points).
  - Macro Precision improved from 87.31% to **94.73%** (+7.42 percentage points).
- **Recall (Coverage of Real Skills)**:
  - Micro Recall stayed at **75.33%** (zero drop in recall).
  - Macro Recall stayed at **74.53%** (zero drop in recall).
- **F1-Score (Overall Balance)**:
  - Micro F1-Score increased from 80.09% to **83.41%** (+3.32 percentage points).
  - Macro F1-Score increased from 78.71% to **81.67%** (+2.96 percentage points).
- **Wrong Tags (False Positives)**:
  - Dropped from 29 down to **12** (a 58.62% reduction in errors).

## 4. Remaining Errors

After the M1 guardrail, two types of errors still remain:

1. **Remaining Wrong Tags (12 tags)**:
   These are all valid technical skills that match the company's business domain (like SAP or Kubernetes) but were not actually requested in the text. Because they are on the official skill list, a vocabulary filter cannot detect that they were guessed.
2. **Remaining Missed Skills (56 skills)**:
   These include 30 missed preferred skills and 26 missed required skills. A filtering guardrail can only remove bad predictions; it cannot find skills that the model never predicted in the first place. Finding these will require modifying the text extractor itself.

## 5. Saved Files

- Job-by-Job Improvement Metrics: `outputs/improvement_per_jd_metrics.csv`
- Overall Metrics Table: `outputs/improvement_overall_metrics.csv`
- Error Breakdown: `outputs/improvement_error_analysis.csv`
- Baseline vs. Improved Comparison: `outputs/improvement_comparison.csv`
- Charts and Visualizations: `outputs/improvement_figures/`
