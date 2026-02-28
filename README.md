<!-- ================= HEADER SECTION ================= -->

<div style="
    text-align:center;
    padding:40px 20px;
    background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);
    color:white;
    border-radius:12px;
">

<h1 style="margin-bottom:10px;font-size:40px;">
CSIRO Biomass Competition
</h1>

<h3 style="font-weight:300;margin-top:0;">
Advanced Structural EDA & Hierarchical Modeling Strategy
</h3>

<p style="max-width:700px;margin:20px auto;font-size:16px;line-height:1.6;">
Multi-Output Regression · Hierarchical Target Decomposition ·  
Leakage-Safe Validation · Distribution-Aware Optimization
</p>

</div>

<br/>

<!-- ================= HIGHLIGHT CARDS ================= -->

<div style="display:flex;flex-wrap:wrap;justify-content:center;gap:20px;">

<div style="
    width:260px;
    padding:20px;
    background:#f4f6f8;
    border-radius:10px;
    box-shadow:0 4px 10px rgba(0,0,0,0.1);
">
<h3>Problem Type</h3>
<p>Multi-Output Regression (Long Format)</p>
</div>

<div style="
    width:260px;
    padding:20px;
    background:#f4f6f8;
    border-radius:10px;
    box-shadow:0 4px 10px rgba(0,0,0,0.1);
">
<h3>Metric</h3>
<p>Globally Weighted R²</p>
</div>

<div style="
    width:260px;
    padding:20px;
    background:#f4f6f8;
    border-radius:10px;
    box-shadow:0 4px 10px rgba(0,0,0,0.1);
">
<h3>Core Insight</h3>
<p>Dry_Total_g drives 50% of score</p>
</div>

</div>

<br/>

<!-- ================= STRUCTURAL MODEL ================= -->

<div style="
    background:#ffffff;
    padding:30px;
    border-radius:12px;
    box-shadow:0 6px 15px rgba(0,0,0,0.08);
    max-width:900px;
    margin:auto;
">

<h2 style="text-align:center;">Hierarchical Target Structure</h2>

<pre style="
background:#0f2027;
color:#00ffcc;
padding:20px;
border-radius:8px;
font-size:16px;
overflow-x:auto;
">
Dry_Green_g + Dry_Clover_g  =  GDM_g
GDM_g + Dry_Dead_g          =  Dry_Total_g
</pre>

<p style="text-align:center;">
Exact structural identities verified across all 357 unique samples.
</p>

</div>

<br/>

<!-- ================= STRATEGY SUMMARY ================= -->

<div style="
    background:#f8f9fa;
    padding:30px;
    border-radius:12px;
    max-width:900px;
    margin:auto;
">

<h2 style="text-align:center;">Modeling Blueprint</h2>

<ol style="line-height:1.8;font-size:15px;">
<li>Use GroupKFold by sample_id to prevent leakage</li>
<li>Predict only base targets (Green, Clover, Dead)</li>
<li>Derive GDM and Total via structural constraints</li>
<li>Train on log1p(target) to stabilize skew</li>
<li>Evaluate using weighted R² on original scale</li>
</ol>

</div>

<br/>

<!-- ================= OPTIONAL JS (Works on GitHub Pages) ================= -->

<script>
function highlight() {
    alert("Hierarchical modeling ensures structural consistency and score stability.");
}
</script>

## Problem Summary

This project performs advanced exploratory data analysis and modeling design for the CSIRO Biomass challenge.

**Task Type:** Multi-Output Regression (Long Format)  
**Evaluation Metric:** Globally Weighted R²  

### Targets

- Dry_Green_g
- Dry_Dead_g
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
