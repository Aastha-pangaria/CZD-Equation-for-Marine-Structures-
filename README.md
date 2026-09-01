# CZD Prediction Framework for Marine Structures

A physically interpretable data-driven framework for predicting
Convection Zone Depth (CZD) in marine concrete structures subjected
to cyclic wetting–drying exposure.

## Overview

Marine concrete structures experience repeated wetting and drying,
which influences moisture transport and chloride ingress and can
accelerate reinforcement corrosion.

This project investigates the prediction of Convection Zone Depth
(CZD) using a hybrid data-driven framework combining:

- Symbolic Regression (PySR)
- Reduced-order modelling
- DWTR-based coefficient analysis
- Random Forest validation

The study uses FEM-generated data from 36 coastal stations with
multiple tidal exposure levels.

## Objectives

- Identify interpretable mathematical relationships governing CZD.
- Determine the dominant variables controlling moisture transport.
- Develop a reduced-order formulation based on drying and wetting
  durations.
- Characterize transport behaviour using the Drying-to-Wetting Time
  Ratio (DWTR).
- Independently validate the governing variables using Machine Learning.

## Methodology

The overall workflow is:

FEM Dataset
      ↓
Data Preprocessing
      ↓
Symbolic Regression (PySR)
      ↓
Recurring Equation Patterns
      ↓
Reduced-Order Formulation
      ↓
Coefficient Extraction
      ↓
DWTR Analysis
      ↓
Generalized Coefficient Functions
      ↓
Random Forest Validation
      ↓
CZD Prediction Framework

## Dataset

The dataset consists of FEM-generated marine exposure simulations
covering:

- 36 coastal stations
- Multiple tidal exposure levels
- Wetting duration (t_wet)
- Drying duration (t_dry)
- Drying flux (q_dry)
- Equilibrium moisture content (θ_r,eq)
- Convection Zone Depth (CZD)

The complete dataset is not included in this repository where
restricted by data ownership or file size.

## Reduced-Order Formulation

Recurring symbolic patterns identified using PySR motivated the
following reduced-order representation:

CZD = k₁√t_dry + k₂√t_wet

where:

- k₁ represents the drying-related transport contribution.
- k₂ represents the wetting-related transport contribution.

Representative median values of q_dry and θ_r,eq were used during
coefficient extraction to isolate the dominant temporal transport
behaviour.

## DWTR Analysis

The Drying-to-Wetting Time Ratio is defined as:

DWTR = t_dry / t_wet

DWTR is used to characterize the relative dominance of drying and
wetting phases across different exposure conditions.

The generalized coefficient relationships obtained from the analysis
are:

k₁(DWTR) = 16.63 / [1 + 19.4(DWTR)^1.27]

k₂(DWTR) = 4.07 exp[-(ln(DWTR) - 2.37)² / (2(3.2)²)]

These relationships provide a continuous representation of the
coefficient trends observed across the station-wise datasets.

## Machine Learning Validation

A Random Forest Regressor was used as an independent validation
approach to quantify the relative influence of the input variables.

The analysis showed that drying and wetting durations together
accounted for approximately 90% of the dominant moisture transport
behaviour, supporting the reduced-order formulation.

## Results

The proposed framework:

- Identified recurring symbolic relationships across exposure levels.
- Revealed strong dominance of temporal exposure variables.
- Captured the transition between drying- and wetting-controlled
  transport behaviour.
- Produced generalized coefficient relationships using DWTR.
- Demonstrated strong agreement with FEM-derived transport profiles.
- Showed good generalization on unseen coastal stations.

Localized deviations remain in regions with abrupt profile transitions
and station-specific variability.

## Repository Structure

```text
data/        Dataset and processed data
notebooks/   Analysis and experimentation notebooks
src/         Reusable Python functions
models/      Saved/trained models
results/     Generated results and figures
docs/        Methodology and equation documentation
