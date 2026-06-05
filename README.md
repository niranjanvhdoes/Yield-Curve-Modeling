# Quantitative Yield Curve Modeling: CIR++ Implementation

This repository contains a quantitative finance pipeline designed to reconstruct a full 8-tenor yield curve (6M to 30Y) using only a 3-Month (3M) proxy rate. 

The project overcomes the structural limitations of standard single-factor interest rate models by utilizing cross-sectional risk-neutral calibration and extending the base Cox-Ingersoll-Ross (CIR) model with a deterministic Brigo-Mercurio (CIR++) shift.

## Core Features
* **Data Engineering:** Rebuilds continuous time-steps ($dt$) by forward-filling non-trading days to maintain the mathematical integrity of the Stochastic Differential Equations (SDEs), alongside rolling Z-score outlier normalization.
* **Risk-Neutral Calibration:** Rejects time-series OLS in favor of cross-sectional optimization via `scipy.optimize.minimize`, successfully extracting the $\mathbb{Q}$-measure parameters ($\kappa, \theta, \sigma$) that reflect embedded market risk premiums.
* **CIR++ Extension (EMA Anchor):** Calculates a deterministic shift vector to absorb single-factor structural bias. To prevent overfitting to historical data, the shift vector is anchored to $t=0$ using an Exponential Moving Average (EMA) on the final 30 days of the training regime.

## Results
The model successfully extrapolates the full yield curve from the 3M proxy, achieving an **Out-of-Sample $R^2$ score of > 0.85** on the blind test dataset, demonstrating strong predictive accuracy on the short-to-medium term tenors while maintaining strict Feller condition stability.

## Dependencies
* `pandas`
* `numpy`
* `scipy`
* `scikit-learn`

## Usage
The primary code is contained within the Jupyter Notebook (`Yield_Curve_Model.ipynb`). Because the data is hosted directly in this repository and fetched via raw URLs, the notebook can be executed seamlessly in any local Python IDE or cloud environment (like Google Colab) without requiring manual data uploads.
