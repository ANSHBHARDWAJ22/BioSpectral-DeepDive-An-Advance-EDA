
<!-- Animated Gradient Header -->
<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=gradient&height=180&section=header&text=CSIRO%20Biomass%20Analysis&fontSize=35&fontColor=ffffff&animation=twinkling" />
</p>

<!-- Typing Animation -->
<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=500&size=20&duration=2500&pause=800&color=2E8B57&center=true&vCenter=true&width=700&lines=Multi-Output+Regression;Hierarchical+Target+Decomposition;Constraint-Aware+Modeling;Leakage-Safe+Validation;Weighted+R2+Optimization" />
</p>

<!-- Animated Divider -->
<p align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%">
</p>

<!-- Dynamic Badges -->
<p align="center">
  <img src="https://img.shields.io/badge/Task-Multi--Output%20Regression-1f77b4?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Metric-Weighted%20R2-2ca02c?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Targets-5-ff7f0e?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Validation-GroupKFold-d62728?style=for-the-badge" />
</p>

<!-- Animated Divider -->
<p align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%">
</p>

## Project Overview

This repository presents an advanced exploratory data analysis and modeling strategy for the **CSIRO Biomass Challenge**.

### Core Characteristics

- Multi-output regression (long format)
- Exact hierarchical target identities
- Distribution stabilization using log transformation
- Leakage-safe validation design
- Competition-aligned modeling strategy

---

## Problem Summary

This project performs advanced exploratory data analysis and modeling design for the CSIRO Biomass challenge.

**Task Type:** Multi-Output Regression (Long Format)  
**Evaluation Metric:** Globally Weighted R²  

### Targets

- Dry_Green_g
-<div align="center">

# CSIRO Biomass Competition

### Advanced Structural EDA & Hierarchical Modeling Strategy

Multi-Output Regression · Constraint-Aware Modeling · Leakage-Safe Validation · Distribution Stabilization

</div>

---

## Problem Overview

**Task Type:** Multi-Output Regression (Long Format)  
**Metric:** Globally Weighted R²  

### Target Weights

| Target         | Weight |
|---------------|--------|
| Dry_Green_g   | 0.10   |
| Dry_Dead_g    | 0.10   |
| Dry_Clover_g  | 0.10   |
| GDM_g         | 0.20   |
| Dry_Total_g   | 0.50   |

Dry_Total_g alone drives 50% of the competition score.

---

## Structural Target Relationships
 Dry_Dead_g
- Dry_Clover_g
- GDM_g
- Dry_Total_g

### Score Weight Distribution

| Target        | Weight |
|--------------|--------|
| Dry_Green_g  | 0.10   |
| Dry_Dead_g   | 0.10   |
| Dry_Clover_g | 0.10   |
| GDM_g        | 0.20   |
| Dry_Total_g  | 0.50   |

> Dry_Total_g alone contributes 50% of the total score, making structural modeling decisions critical.

---

## Structural Insight (Core Discovery)

Exact mathematical identities verified in training data:

# CSIRO Biomass Competition — Advanced EDA & Modeling Strategy

## Overview

This notebook presents a structured, competition-grade exploratory data analysis (EDA) for the CSIRO Biomass challenge.

The task is a **multi-output regression problem (long format)** predicting:

- `Dry_Green_g`
- `Dry_Dead_g`
- `Dry_Clover_g`
- `GDM_g`
- `Dry_Total_g`

Evaluation Metric: **Globally Weighted R²**

Target Weights:

| Target         | Weight |
|---------------|--------|
| Dry_Green_g   | 0.10   |
| Dry_Dead_g    | 0.10   |
| Dry_Clover_g  | 0.10   |
| GDM_g         | 0.20   |
| Dry_Total_g   | 0.50   |

`Dry_Total_g` alone drives 50% of the score, making structural modeling critical.

---

## Dataset Structure

- 1,785 training rows
- 357 unique images (each appears 5 times — long format)
- All five targets available per image
- Numeric Features:
  - `Pre_GSHH_NDVI`
  - `Height_Ave_cm`
- Categorical Features:
  - `State`
  - `Species`
  - `Sampling_Date`

### Critical Validation Strategy

Since each image appears multiple times:

- Row-level splitting causes leakage.
- Validation must use `GroupKFold(sample_id)`.

---

## Structural Identity Verification

Two exact structural identities were verified:

1. `GDM_g = Dry_Green_g + Dry_Clover_g`
2. `Dry_Total_g = GDM_g + Dry_Dead_g`

Verification results:

- Mean absolute residual: 0.00 g
- Median % error: 0.00%
- No row violates identity

These are exact mathematical relationships.

### Modeling Implication

Instead of predicting 5 targets independently:

Predict only:
- `Dry_Green_g`
- `Dry_Clover_g`
- `Dry_Dead_g`

Then derive:

This guarantees:
- Structural consistency
- Zero logical violations
- Stability on the 50% weighted target

---

## Target Distribution Analysis

All biomass targets are right-skewed.

| Target         | Skew (raw) |
|---------------|------------|
| Dry_Green_g   | 1.74       |
| Dry_Dead_g    | 1.75       |
| Dry_Clover_g  | 2.83       |
| GDM_g         | 1.55       |
| Dry_Total_g   | 1.42       |

After `log1p` transformation:

- Skew reduces significantly
- Distribution becomes near symmetric
- Extreme values naturally compressed

### Final Decision

- Train using `log1p(target)`
- Predict on log scale
- Convert back using `expm1(pred)`
- Use standard MSE loss

Outliers are real agricultural measurements and must not be removed.

---

## Feature-Level Observations

### Height_Ave_cm
- Strongly correlated with biomass
- Not leakage (available at inference)
- Monitor feature dominance
- Log-transform if necessary

### Pre_GSHH_NDVI
- Strong agronomic signal
- Safe (measured before sampling)

### Species
- 13 categories
- Some rare classes (<5 samples)
- Risk of overfitting with One-Hot Encoding

Mitigation:
- Group rare categories
- Use CatBoost
- Or apply target encoding with CV

### Sampling_Date
- Full growth season coverage (Jan–Nov 2015)
- Temporal features likely important

Feature Engineering:
- Month
- Season
- Day-of-year

---

## Loss Function Decision

Based on skewness and outlier severity:

- Raw MSE not ideal
- Huber not required
- Best choice: `log1p + MSE`

---

## Final Modeling Blueprint

1. Use `GroupKFold` by `sample_id`
2. Train on `log1p(Dry_Green_g, Dry_Clover_g, Dry_Dead_g)`
3. Derive:
   - `GDM_g`
   - `Dry_Total_g`
4. Evaluate using weighted R² on original scale
5. Use tree-based models (LightGBM / CatBoost)

---

## Strategic Insight

This is not five independent regression problems.

This is a hierarchical biomass decomposition problem with exact structural constraints.

Exploiting these constraints provides:
- Logical consistency
- Reduced variance
- Direct optimization of the 50% weighted target

The notebook focuses on extracting modeling decisions from EDA rather than performing surface-level visualization.

---

## Conclusion

This EDA establishes:

- No hard data leakage
- Exact structural target relationships
- Correct transformation strategy
- Proper validation design
- Controlled outlier handling
- Clear modeling blueprint

The next step is disciplined baseline modeling and controlled experimentation.  
