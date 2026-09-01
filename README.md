# CZD Prediction Framework for Marine Structures

A physically interpretable data-driven framework for predicting **Convection Zone Depth (CZD)** in marine concrete structures subjected to cyclic wetting–drying exposure.

---

## Overview

Marine reinforced concrete structures are exposed to aggressive environmental conditions, particularly in tidal and splash zones where repeated wetting and drying influence moisture transport and chloride ingress.

This project investigates an interpretable predictive framework for **Convection Zone Depth (CZD)** using a hybrid approach combining:

- **Symbolic Regression (PySR)**
- **Reduced-order modelling**
- **DWTR-based coefficient analysis**
- **Random Forest validation**

The study uses FEM-generated marine transport data covering **36 coastal stations** and multiple tidal exposure levels.

The primary objective is not only to predict CZD, but also to identify and interpret the dominant transport mechanisms governing its evolution.

---

## Objectives

The main objectives of the study are:

- Identify interpretable mathematical relationships governing CZD using Symbolic Regression.
- Determine the dominant variables controlling cyclic moisture transport.
- Develop a reduced-order formulation based on wetting and drying durations.
- Characterize drying- and wetting-controlled transport using the Drying-to-Wetting Time Ratio (DWTR).
- Develop generalized coefficient relationships for CZD prediction.
- Independently validate the dominant governing variables using Random Forest.

---

## Methodology

The overall analytical workflow consists of the following phases:

```text
FEM-Generated Dataset
        ↓
Data Preprocessing
        ↓
Symbolic Regression (PySR)
        ↓
Recurring Symbolic Patterns
        ↓
Reduced-Order Formulation
        ↓
Coefficient Extraction
        ↓
DWTR-Based Analysis
        ↓
Generalized Coefficient Functions
        ↓
Random Forest Validation
        ↓
CZD Prediction Framework
```

### 1. FEM-Generated Dataset

The study uses data generated through FEM-based marine transport simulations across:

- **36 coastal stations**
- **10 tidal exposure levels per station**
- Approximately **360 exposure-condition datasets**

The primary variables considered are:

| Variable | Description |
|---|---|
| `t_wet` | Wetting duration |
| `t_dry` | Drying duration |
| `q_dry` | Drying flux |
| `θ_r,eq` | Equilibrium moisture content |
| `CZD` | Convection Zone Depth |

The FEM simulations provide transport profiles and corresponding CZD values for data-driven analysis.

---

## Symbolic Regression

**Symbolic Regression (SR)** was performed using the **PySR** framework to discover mathematical relationships between exposure variables and CZD.

Unlike conventional regression, which requires a predefined functional form, Symbolic Regression searches for both:

1. The mathematical structure
2. The corresponding coefficients

The initial equations varied across exposure levels and stations. However, recurring mathematical patterns were observed, particularly involving nonlinear temporal terms.

The repeated appearance of square-root relationships involving wetting and drying durations motivated the development of a reduced-order formulation.

---

## Reduced-Order Formulation

The recurring symbolic patterns were simplified into the following reduced-order representation:

$$
CZD = k_1\sqrt{t_{dry}} + k_2\sqrt{t_{wet}}
$$

where:

- $k_1$ represents the **drying-related transport contribution**.
- $k_2$ represents the **wetting-related transport contribution**.
- $t_{dry}$ is the drying duration.
- $t_{wet}$ is the wetting duration.

During coefficient extraction, representative **median values** of the drying flux ($q_{dry}$) and equilibrium moisture content ($\theta_{r,eq}$) were adopted to isolate the dominant temporal transport behaviour.

---

## DWTR-Based Transport Analysis

The **Drying-to-Wetting Time Ratio (DWTR)** was introduced to characterize the relative influence of drying and wetting phases:

$$
DWTR = \frac{t_{dry}}{t_{wet}}
$$

DWTR provides a normalized descriptor of cyclic exposure conditions.

- **Higher DWTR** → relatively stronger drying influence
- **Lower DWTR** → relatively stronger wetting influence
- **Intermediate DWTR** → stronger interaction between wetting and drying phases

The extracted coefficients $k_1$ and $k_2$ were subsequently analyzed as functions of DWTR.

---

## Generalized Coefficient Functions

The station-wise coefficient trends were represented using continuous nonlinear functions.

### Drying-Related Coefficient

$$
k_1(DWTR)=
\frac{16.63}
{1+19.4(DWTR)^{1.27}}
$$

The $k_1$ relationship represents a **monotonic nonlinear decay** with DWTR.

### Wetting-Related Coefficient

$$
k_2(DWTR)=
4.07\exp
\left[
-\frac{(\ln(DWTR)-2.37)^2}
{2(3.2)^2}
\right]
$$

The $k_2$ relationship represents a **broad peak-type response** across the DWTR range.

Together, these functions provide generalized coefficient relationships for representing cyclic marine transport behaviour.

---

## Random Forest Validation

A **Random Forest Regressor** was used as an independent Machine Learning validation approach.

The objective was not equation discovery, but to quantify the relative influence of the input variables on CZD prediction.

The analysis showed that:

- **Drying duration** was the most influential variable.
- **Wetting duration** was the second dominant variable.
- Together, wetting and drying durations contributed approximately **90%** of the dominant moisture transport behaviour.
- Drying flux and equilibrium moisture content showed comparatively smaller influence.

These results independently support the reduced-order formulation based primarily on temporal exposure variables.

---

## Key Results

The study demonstrated that:

- Symbolic Regression successfully identified interpretable mathematical relationships for CZD.
- Recurring square-root temporal relationships were observed across multiple exposure conditions.
- Drying and wetting durations emerged as the dominant governing variables.
- DWTR successfully characterized the transition between drying- and wetting-controlled transport behaviour.
- Generalized nonlinear coefficient functions were obtained for $k_1$ and $k_2$.
- The reduced-order framework showed strong agreement with FEM-derived transport profiles.
- Independent validation on unseen coastal stations demonstrated good generalization capability.
- Localized deviations remained in regions with abrupt profile transitions and station-specific variability.

---

## Physical Interpretation

The analysis indicates a systematic transition in transport behaviour across marine exposure conditions:

```text
Drying-Dominated
       ↓
Intermediate Cyclic Interaction
       ↓
Wetting-Dominated
```

The DWTR-based coefficient analysis captures this changing balance between drying and wetting influences.

The results further indicate that **cyclic temporal exposure is the primary driver of CZD evolution**, while the other investigated environmental variables act as comparatively smaller secondary modifiers within the studied dataset.

---

## Generalization

The generalized coefficient framework was evaluated on **unseen coastal stations**.

The framework successfully reproduced major transport trends, including:

- Overall profile evolution
- Dominant peak regions
- Transition between drying- and wetting-controlled behaviour

Localized deviations remained in certain exposure regions due to station-specific variability and abrupt profile transitions.

---

## Limitations

The current framework has several limitations:

- Localized prediction can be sensitive to abrupt profile transitions.
- Station-specific variability is not completely eliminated.
- The reduced-order formulation focuses primarily on the dominant temporal variables.
- Secondary environmental effects are represented indirectly.
- Further experimental and field validation is required before direct engineering implementation.

---

## Future Work

Future extensions of this work include:

- Extending validation across larger multi-station and multi-regional marine datasets.
- Developing a unified global CZD equation using constrained Symbolic Regression.
- Incorporating additional environmental variables to improve localized prediction.
- Improving representation of station-wise variability and abrupt profile transitions.
- Integrating the framework with long-term durability and service-life models.
- Validating the framework using experimental and real-field marine exposure data.

---

## Repository Structure

```text
CZD-Prediction-Marine-Structures/
│
├── README.md
├── requirements.txt
├── LICENSE
├── .gitignore
│
├── data/
│   ├── README.md
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   ├── 01_data_preprocessing.ipynb
│   ├── 02_pysr_symbolic_regression.ipynb
│   ├── 03_parameter_isolation.ipynb
│   ├── 04_coefficient_extraction.ipynb
│   ├── 05_dwtr_analysis.ipynb
│   ├── 06_coefficient_functions.ipynb
│   └── 07_random_forest_validation.ipynb
│
├── src/
│   ├── preprocessing.py
│   ├── symbolic_regression.py
│   ├── coefficient_analysis.py
│   ├── dwtr_analysis.py
│   └── validation.py
│
├── results/
│   ├── symbolic_regression/
│   ├── coefficients/
│   ├── dwtr/
│   ├── validation/
│   └── figures/
│
├── models/
│   └── README.md
│
└── docs/
    ├── methodology.md
    └── equations.md
```

---

## Technologies

- **Python**
- **PySR**
- **Scikit-learn**
- **Random Forest**
- **NumPy**
- **Pandas**
- **Matplotlib**

---

## Installation

Clone the repository:

```bash
git clone https://github.com/<username>/CZD-Prediction-Marine-Structures.git
cd CZD-Prediction-Marine-Structures
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

The analysis can be reproduced by following the notebooks in sequence:

```text
01_data_preprocessing
        ↓
02_pysr_symbolic_regression
        ↓
03_parameter_isolation
        ↓
04_coefficient_extraction
        ↓
05_dwtr_analysis
        ↓
06_coefficient_functions
        ↓
07_random_forest_validation
```

Where the complete FEM-generated dataset cannot be publicly redistributed due to data ownership, file size, or institutional restrictions, a representative sample dataset and/or processed results may be provided where permissible.

---

## Research Context

This project was undertaken as part of a thesis on data-driven modelling of moisture transport and Convection Zone Depth in marine concrete structures.

The work combines:

**Physics-Based Simulation → Symbolic Regression → Reduced-Order Modelling → DWTR Analysis → Machine Learning Validation**

to bridge the gap between computational transport modelling and interpretable engineering prediction.

---

## Author

**[Your Name]**  
Indian Institute of Technology Bhubaneswar

---

## Citation

If you use this work, please cite:

```text
[Your Name].
"[Full Thesis Title]".
Thesis,
Indian Institute of Technology Bhubaneswar, 2026.
```

---

## License

This repository is intended primarily for academic and research purposes.

Please refer to the accompanying `LICENSE` file for usage and redistribution terms.
