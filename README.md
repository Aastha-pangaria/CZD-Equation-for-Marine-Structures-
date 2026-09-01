# CZD Prediction Framework for Marine Structures

A physically interpretable data-driven framework for predicting **Convection Zone Depth (CZD)** in marine concrete structures subjected to cyclic wetting–drying exposure.

## Overview

Marine concrete structures experience repeated wetting and drying in tidal and splash zones. These cyclic conditions influence moisture transport and can contribute to chloride ingress, reinforcement corrosion, and deterioration.

This project investigates a data-driven approach for predicting **Convection Zone Depth (CZD)** while retaining physical interpretability.

The framework combines:

- **Symbolic Regression (PySR)** for mathematical equation discovery
- **Reduced-order modelling** for a compact CZD formulation
- **Drying-to-Wetting Time Ratio (DWTR)** analysis for transport-regime characterization
- **Coefficient analysis** for understanding drying- and wetting-controlled behaviour
- **Random Forest validation** for independent assessment of dominant variables

The study is based on **FEM-generated data from 36 coastal stations** covering multiple marine exposure conditions.

---

## Objectives

The main objectives of this work are to:

- Identify interpretable mathematical relationships governing CZD.
- Determine the dominant variables controlling cyclic moisture transport.
- Develop a reduced-order CZD formulation using wetting and drying durations.
- Characterize drying- and wetting-controlled transport using DWTR.
- Develop generalized coefficient functions.
- Independently validate the dominant transport variables using Random Forest analysis.

---

## Methodology

The overall workflow followed in the study is:

```text
FEM-Generated Dataset
        ↓
Data Preprocessing
        ↓
Symbolic Regression (PySR)
        ↓
Identification of Recurring Relationships
        ↓
Reduced-Order Equation
        ↓
Station-Wise Coefficient Extraction
        ↓
DWTR-Based Coefficient Analysis
        ↓
Generalized Coefficient Functions
        ↓
Random Forest Validation
        ↓
Validation on Unseen Stations
```

---

## Dataset

The dataset is generated using **Finite Element Modelling (FEM)** for marine moisture transport under cyclic wetting–drying exposure.

The study considers:

- **36 coastal stations**
- Multiple tidal exposure levels
- Wetting duration, `t_wet`
- Drying duration, `t_dry`
- Drying flux, `q_dry`
- Equilibrium moisture content, `θ_r,eq`
- Convection Zone Depth, `CZD`

The FEM-derived moisture profiles are used to identify the CZD and subsequently develop the data-driven predictive framework.

> **Note:** The complete FEM dataset may not be included in this repository depending on data size, ownership, or redistribution restrictions.

---

## Symbolic Regression

**Symbolic Regression (SR)** was performed using **PySR** to discover mathematical relationships between the input variables and CZD.

Unlike conventional regression, Symbolic Regression does not require a fixed mathematical form to be specified in advance. It searches over mathematical expressions to identify relationships that provide a balance between predictive performance and interpretability.

Recurring symbolic patterns were identified across the exposure levels, with square-root terms involving the wetting and drying durations appearing repeatedly.

These observations motivated the development of a reduced-order formulation.

---

## Reduced-Order CZD Formulation

The recurring symbolic relationships were represented using the following reduced-order form:

$$
CZD = k_1\sqrt{t_{dry}} + k_2\sqrt{t_{wet}}
$$

where:

- $k_1$ represents the drying-controlled transport contribution.
- $k_2$ represents the wetting-controlled transport contribution.
- $t_{dry}$ is the drying duration.
- $t_{wet}$ is the wetting duration.

Representative values of the environmental variables were used during coefficient isolation to investigate the dominant temporal behaviour.

---

## DWTR Analysis

The **Drying-to-Wetting Time Ratio (DWTR)** was introduced to characterize the relative dominance of the drying and wetting phases:

$$
DWTR = \frac{t_{dry}}{t_{wet}}
$$

The DWTR provides a dimensionless measure of the relative duration of drying and wetting.

Broadly:

- Higher DWTR corresponds to relatively stronger drying influence.
- Lower DWTR corresponds to relatively stronger wetting influence.
- Intermediate DWTR represents a transition region between the two regimes.

The station-wise coefficients $k_1$ and $k_2$ were analyzed as functions of DWTR.

---

## Generalized Coefficient Functions

The coefficient analysis resulted in generalized nonlinear relationships for the two transport contributions.

### Drying-Controlled Coefficient

$$
k_1(DWTR)=
\frac{16.63}
{1+19.4(DWTR)^{1.27}}
$$

The coefficient $k_1$ decreases monotonically with increasing DWTR.

### Wetting-Controlled Coefficient

$$
k_2(DWTR)=
4.07\exp
\left[
-\frac{(\ln(DWTR)-2.37)^2}
{2(3.2)^2}
\right]
$$

The coefficient $k_2$ exhibits a peak-type dependence on DWTR, representing stronger wetting influence within an intermediate DWTR range.

Together, these functions provide a continuous representation of the coefficient behaviour observed across the investigated exposure conditions.

---

## Random Forest Validation

A **Random Forest Regressor** was used as an independent machine-learning approach to assess the relative importance of the input variables.

The analysis indicated that:

- Drying duration was the most influential variable.
- Wetting duration was the second most influential variable.
- The two temporal variables together contributed approximately **90%** of the dominant transport behaviour.
- Drying flux and equilibrium moisture content had comparatively smaller contributions within the investigated dataset.

This independent analysis supports the emphasis placed on wetting and drying durations in the reduced-order CZD formulation.

---

## Validation

The proposed formulation was evaluated against FEM-derived CZD profiles.

Validation included:

- Agreement between predicted and observed CZD values.
- Comparison of predicted and actual moisture-depth trends.
- Assessment of dominant transport mechanisms.
- Independent validation on previously unseen coastal stations.

The framework reproduced the major CZD trends across the investigated stations, while localized deviations remained in regions affected by abrupt profile transitions and station-specific variability.

---

## Key Findings

The major findings of the study are:

1. **Symbolic Regression identified recurring interpretable relationships** governing CZD.
2. **Square-root temporal terms** repeatedly appeared in the discovered equations.
3. **Drying and wetting durations emerged as the dominant temporal variables.**
4. **DWTR captured the transition between drying- and wetting-controlled behaviour.**
5. The coefficient $k_1$ showed a **monotonic decrease** with DWTR.
6. The coefficient $k_2$ showed a **peak-type dependence** on DWTR.
7. Random Forest analysis independently supported the dominance of the temporal variables.
8. The resulting reduced-order framework reproduced the major FEM-derived CZD trends.
9. The framework showed promising generalization to unseen stations.
10. Remaining deviations highlight the importance of station-specific and secondary environmental effects.

---

## Limitations

The current framework has several limitations:

- Localized predictions can be sensitive to abrupt moisture-profile transitions.
- Station-specific variability is not completely captured.
- The reduced-order formulation focuses primarily on the dominant temporal variables.
- Secondary environmental effects are represented indirectly.
- Further experimental and field validation is required before direct engineering implementation.

---

## Future Work

Future extensions of the study include:

- Extending the analysis to a larger number of stations and marine exposure conditions.
- Investigating alternative nonlinear functional forms for generalized coefficient modelling.
- Incorporating additional environmental descriptors for improved localized prediction.
- Developing a unified station-independent CZD prediction equation.
- Validating the framework against experimental and field data.
- Integrating the model with long-term durability and service-life prediction frameworks.

---

## Repository Structure

```text
CZD-Prediction-Marine-Structures/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── Code/
│   ├── after_taking_J_and_TE_Q_constant.ipynb
│   ├── analysis.ipynb
│   ├── analysis_of_level_8_9_10.ipynb
│   ├── combining_data_for_S1_and_running_PYSR.ipynb
│   ├── end_model_and_analysis.ipynb
│   ├── final.ipynb
│   ├── merged_model.ipynb
│   ├── new_MTP_(1)_(1).ipynb
│   ├── plotting.ipynb
│   ├── runing_level_wise_1_to_5.ipynb
│   └── tryin_that_k_thing.ipynb
│
├── thesis.pdf
```

---

## Notebooks

The `Code/` directory contains the computational work carried out during the project, including data analysis, level-wise Symbolic Regression, parameter isolation, coefficient analysis, model development, and plotting.

The notebooks represent the progression of the analysis from exploratory work to the final modelling framework.

---

## Technologies Used

- **Python**
- **PySR**
- **Scikit-learn**
- **Random Forest**
- **NumPy**
- **Pandas**
- **Matplotlib**
- **Jupyter Notebook**

---

## Installation

Clone the repository:

```bash
git clone https://github.com/Aastha-pangaria/CZD-Equation-for-Marine-Structures-.git
cd CZD-Equation-for-Marine-Structures-
```
Install the required dependencies:

```bash
pip install -r requirements.txt
```

PySR can be installed using:

```bash
pip install pysr
```

---

## Reproducibility

The notebooks contain the computational workflow used for the analysis.

Because the notebooks were developed iteratively during the research process, they are not necessarily intended to be executed as a single linear pipeline.

For understanding the final methodology, the recommended order is:

```text
Data / FEM Outputs
        ↓
Data Analysis
        ↓
Level-Wise Symbolic Regression
        ↓
Parameter Isolation
        ↓
Coefficient Extraction
        ↓
DWTR Analysis
        ↓
Final Model Development
        ↓
Validation
        ↓
Plots and Results
```

The final thesis provides the complete methodology, equations, analysis, and interpretation.

---

## Thesis

The complete thesis documenting the methodology and results is available in:

```text
thesis.pdf
```

The thesis contains the detailed background, literature review, FEM-generated dataset description, Symbolic Regression methodology, coefficient analysis, DWTR analysis, Random Forest validation, results, limitations, and future work.

---

## Research Context

This project was completed as part of a thesis on **data-driven prediction of Convection Zone Depth in marine tidal structures**.

The work combines physics-based simulation data with interpretable machine-learning techniques to develop a reduced-order predictive framework.

The central idea is:

```text
FEM-Based Physics
       ↓
Data-Driven Equation Discovery
       ↓
Physical Interpretation
       ↓
Reduced-Order Model
       ↓
Independent ML Validation
       ↓
Generalized CZD Prediction
```

---

## Author

**Aastha Pangaria**  
Indian Institute of Technology Bhubaneswar

---

## Citation

If you use this work, please cite:

```text
Pangaria, A.
"Prediction of Convection Zone Depth in Marine Tidal Structures using Symbolic Regression."
Thesis, Indian Institute of Technology Bhubaneswar, 2026.
```

---

## License

This repository is intended primarily for academic and research purposes.

Please refer to the repository license for terms regarding use, modification, and redistribution.
