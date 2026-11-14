# Baseline Model Evaluation Summary

This document summarizes the performance of the original skill-tagging model before any improvements.

## 1. Dataset Overview

- **Job Descriptions**: 50 job descriptions across 10 job roles and 10 companies.
- **Official Skill List**: 54 standardized skills.
- **True Skills to Find (Ground Truth)**: 227 total skill instances (177 Required skills, 50 Preferred skills).
- **Model Predictions**: 200 total skill tags produced by the original model.

## 2. Overall Performance Results

- **Precision (Accuracy of Predictions)**:
  - Micro Precision: 85.50% (171 correct out of 200 predicted tags)
  - Macro Precision: 87.31%
- **Recall (Coverage of Real Skills)**:
  - Micro Recall: 75.33% (171 found out of 227 ground-truth skills)
  - Macro Recall: 74.53%
- **F1-Score (Overall Balance)**:
  - Micro F1-Score: 80.09%
  - Macro F1-Score: 78.71%
- **Tag Breakdown**:
  - True Positives (Correct tags): 171
  - False Positives (Wrong tags): 29
  - False Negatives (Missed skills): 56

## 3. Required vs. Preferred Skills

There is a large difference in how well the model finds required skills versus optional bonus skills:

- **Required Skills Found**: 85.31% recall (151 found out of 177)
- **Preferred Skills Found**: 40.00% recall (20 found out of 50)
- **Performance Gap**: 45.31 percentage points.

The model is good at finding mandatory skills, but misses more than half of the nice-to-have skills.

## 4. Main Model Errors

The error analysis found three main causes for wrong or missed tags:

1. **Missed Preferred Skills (30 missed instances, 53.57% of all false negatives)**:
   The model often skips sections that start with words like "Nice to have:", "Bonus:", or "Good to have".
2. **Predicting Soft Skills Not on the Official List (16 wrong tags, 55.17% of all false positives)**:
   The model predicted general traits (such as Leadership, Teamwork, Problem Solving, Communication, Time Management) that are not in the official 54-skill list.
3. **Guessing Skills Based on Company Domain (12 wrong tags, 41.38% of all false positives)**:
   The model guessed technical skills that match the company business (for example, guessing SAP for Orbit Logistics or Kubernetes for Lumen Cloud) even when the job description never mentioned them.

## 5. Saved Files

- Per-Job Description Results: `outputs/per_jd_metrics.csv`
- Overall Performance Table: `outputs/overall_metrics.csv`
- Error Breakdown: `outputs/error_analysis.csv`
- List of Wrong Tags: `outputs/false_positive_analysis.csv`
- List of Missed Skills: `outputs/false_negative_analysis.csv`
- Charts and Visualizations: `outputs/baseline_figures/`
