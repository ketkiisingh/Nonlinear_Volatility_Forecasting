# Nonlinear Dynamical Features for Financial Volatility Forecasting

## Research Overview

This project investigates whether nonlinear dynamical features extracted from historical S&P 500 realized variance improve next-session volatility forecasting beyond the conventional Heterogeneous Autoregressive Realized Variance (HAR-RV) model.

The research combines classical financial econometrics with nonlinear time-series feature engineering, drawing on concepts from dynamical systems.

## Research Question

**Do nonlinear dynamical features provide incremental predictive information for next-session S&P 500 realized variance beyond conventional daily, weekly and monthly HAR-RV predictors?**

## Dataset

- **Source:** Oxford-Man Institute Realized Library, accessed through an archived CSV
- **Asset:** S&P 500 index
- **Measure:** Five-minute realized variance (`SPX2.rv`)
- **Coverage:** January 2000 – October 2016
- **Original observations:** 4,380
- **Valid realized-variance observations:** 4,189
- **Final extended-model observations:** 1,104

Missing observations were retained during rolling-feature construction so that forecasting targets would not silently skip missing market sessions. Complete historical windows were required, reducing the effective sample.

## Methodology

### Baseline: HAR-RV

The baseline predicts next-session log realized variance using:

- Current daily log realized variance
- Trailing five-session mean log realized variance
- Trailing 22-session mean log realized variance

Parameters are estimated using ordinary least squares.

### Nonlinear Dynamical Features

Three additional features were investigated:

1. **Volatility acceleration:** Second difference of log realized variance.
2. **Local dispersion:** Rolling standard deviation of log-volatility changes over ten sessions.
3. **Local dependence:** Rolling lag-one correlation over 22 sessions.

The extended model uses standardized Ridge regression to reduce coefficient instability arising from correlated predictors.

### Exploratory Recurrence Analysis

A three-dimensional delay embedding was constructed from current and lagged log realized variance.

A recurrence-rate feature measured the proportion of historical embedded states within a fixed distance of the current state.

Only 536 observations had valid recurrence measurements under the specified 30-state historical-window requirement. The feature was therefore excluded from the primary forecasting comparison.

### Model Validation

Data were divided chronologically:

| Period | Purpose |
|---|---|
| 2000–2010 | Training |
| 2011–2013 | Validation |
| 2014–2016 | Final out-of-sample test |

Ridge regularization was selected using validation RMSE. The selected parameter was α = 0.01.

All final models were refitted on combined training and validation observations before test evaluation.

## Final Out-of-Sample Results

Evaluation was conducted on 206 common test observations.

| Model | MAE | RMSE | QLIKE |
|---|---:|---:|---:|
| HAR-RV OLS | 0.549625 | **0.699932** | 0.420725 |
| HAR-RV Ridge | 0.549624 | **0.699932** | 0.420725 |
| HAR + Nonlinear Ridge | 0.554728 | 0.703942 | **0.397260** |

### Main Finding

The nonlinear extension increased RMSE by approximately 0.57% and MAE by approximately 0.93%, while reducing QLIKE by approximately 5.58%.

Because RMSE was the predefined primary selection metric, the experiment does not demonstrate an improvement in primary out-of-sample forecasting accuracy.

The QLIKE reduction is a secondary, metric-dependent finding; its statistical significance has not been established.

## Limitations

- The dataset ends in October 2016 and does not cover more recent volatility regimes.
- Strict missing-data handling substantially reduces the effective sample.
- The nonlinear feature set is limited and does not establish deterministic chaos.
- The recurrence feature was excluded because of sample attrition.
- No statistical significance test of forecast-loss differences was conducted.
- Results are based on one index and one historical test period.

## Research Conclusion

The experiment finds that a small set of nonlinear dynamical descriptors does not consistently outperform the conventional HAR-RV benchmark for next-session S&P 500 volatility forecasting.

Although the nonlinear extension improves QLIKE, it underperforms under the predefined primary RMSE criterion.

The findings illustrate the importance of rigorous chronological validation, common-sample model comparisons, transparent reporting of negative results, and distinguishing nonlinear feature engineering from evidence of deterministic chaos.

## Tools

Python, NumPy, Pandas, scikit-learn, Matplotlib and Jupyter Notebook.

## Reproducibility

Run the notebook in chronological order, beginning with data ingestion and preprocessing, followed by HAR-RV construction, nonlinear feature engineering, validation, final model fitting and test evaluation.

The notebook uses an archived Oxford-Man realized-volatility CSV. The dataset URL and environment dependencies should be retained for reproducibility.
