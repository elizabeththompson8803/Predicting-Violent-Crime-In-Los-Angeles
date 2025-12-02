# Los Angeles Violent Crime Analysis

This repository contains a data-driven analysis of violent and non-violent crime incidents in Los Angeles, focusing on spatial and temporal patterns. The project applies logistic regression and Random Forest models to classify violent incidents and explore the predictive value of various features.

## Project Overview

- **Objective:** Predict violent vs. non-violent crimes using spatial and temporal data from Los Angeles police reports.  
- **Data:** Administrative crime data, including victim demographics, incident location (latitude/longitude), and date/time.  
- **Methods:**  
  - Logistic Regression (baseline and with spatial-temporal features)  
  - Random Forest Classification (baseline and with spatial-temporal features)  
- **Key Findings:**  
  - Models with spatial and temporal features slightly improve prediction performance.  
  - Random Forest models outperform logistic regression, capturing nonlinear interactions.  
  - Certain victim demographics (age, sex, descent) and geographic coordinates are strong predictors.  
  - Crime hotspots remain geographically stable across years, particularly in Central and South Los Angeles.  

## Repository Structure
  - Notebooks/ Notebooks for EDA, methodology and results, and statistical inference results
  - README.md Project overview
  - refs.bib Bibliography for the report


## Usage

1. Clone the repository:
git clone https://github.com/elizabeththompson8803/Predicting-Violent-Crime-In-Los-Angeles
2. Install dependencies (Python, scikit-learn, pandas, matplotlib, seaborn, etc.).
3. Run notebooks in `Notebooks/` for EDA, modeling, and visualization.

## Results

- ROC and PR curves, confusion matrices, and classification reports for all models are available in the `Notebooks' file.
- Key performance metrics:
- Logistic Regression ROC AUC: ~0.72–0.73  
- Random Forest ROC AUC: ~0.75  
- High recall for violent crimes in all models, with trade-offs in precision for non-violent crimes.  

## References

See `refs.bib` for all citations used in the analysis.

## Authors

Elizabeth Thompson, Rylenn Berry, Juliana Perez Romero
