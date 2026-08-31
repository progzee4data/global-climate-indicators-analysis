# Global Climate Indicators Analysis (2000–2020)

An empirical data analysis project evaluating non-linear trends, polynomial regression performance, and bias-variance tradeoffs across global climate indicators using Google Sheets and quantitative accuracy metrics.

---

## Project Overview

This project analyzes key global climate metrics—specifically **Atmospheric CO₂ Concentration (ppm)** and **Sea Level Rise (mm)**—over a 20-year period (2000–2020). By fitting linear and higher-degree polynomial trendlines, calculating explicit residuals, and measuring precision metrics ($R^2$, MAE, MSE, RMSE), this analysis explores model selection and the physical dynamics of climate systems.

---

## Key Findings & Model Comparison

### 1. Atmospheric CO₂ Concentration over Time

Linear models underfit atmospheric CO₂ data due to direct, accelerating human carbon emissions. Moving to a **Quadratic (Degree 2)** fit eliminates structural U-shaped residual patterns and reduces prediction error by over 50%.

| Degree | Fit Type | Trendline Equation ($x = \text{Year}$) | $R^2$ Value | RMSE (ppm) | Model Evaluation |
| :---: | :--- | :--- | :---: | :---: | :--- |
| **1** | Linear | $y = 2.235x - 4101$ | `0.997` | `0.753` | Underfit (High Bias) |
| **2** | **Quadratic** | $y = 0.0271x^2 - 106.9x + 1.05\times 10^5$ | **`0.999`** | **`0.364`** | **Optimal Fit** |
| **3** | Cubic | $y = 0.000353x^3 - 2.1x^2 + 4165x - 2.76\times 10^6$ | `1.000` | `0.278` | Overfit / Unnecessary Complexity |
| **4** | Quartic | $y = 1.34\times 10^{-5}x^4 - 0.108x^3 + 325x^2 - 4.35\times 10^5x + 2.19\times 10^8$ | `1.000` | `0.263` | Overfit (High Variance) |

---

### 2. Sea Level Rise over Time

Sea level rise exhibits high linearity over short observation windows due to ocean thermal inertia, but quadratic models better capture the physical acceleration driven by thermal expansion and ice sheet melt.

| Degree | Fit Type | Trendline Equation ($x = \text{Year}$) | $R^2$ Value | RMSE (mm) | Model Evaluation |
| :---: | :--- | :--- | :---: | :---: | :--- |
| **1** | Linear | $y = 3.807x - 7618$ | `0.991` | `2.254` | Underfit (High Bias) |
| **2** | **Quadratic** | $y = 0.0668x^2 - 264.8x + 2.62\times 10^5$ | **`0.999`** | **`0.557`** | **Optimal Fit** |
| **3** | Cubic | $y = 0.00184x^3 - 11.05x^2 + 22070x - 1.47\times 10^7$ | `1.000` | `0.458` | Overfit |
| **4** | Quartic | $y = -0.000415x^4 + 3.34x^3 - 10066x^2 + 1.35\times 10^7x - 6.79\times 10^9$ | `1.000` | `0.268` | Overfit |

---

## Analytical Takeaways

* **Underfitting (Degree 1)**: Linear trendlines yield high $R^2$ values (>0.99) but fail to capture non-linear acceleration, leaving systematic U-shaped residual patterns in both datasets.
* **The Sweet Spot (Degree 2)**: Quadratic polynomials provide the ideal balance. They eliminate structural residual bias, reduce RMSE dramatically (75% drop in Sea Level Rise error; >50% drop in CO₂ error), and reflect real-world climate physics without introducing artificial inflection points.
* **Overfitting (Degrees 3 & 4)**: Higher-degree models yield negligible improvements in $R^2$ while fitting short-term observational noise, making them unreliable for long-term forecasting.
* **Physical System Dynamics**: While CO₂ shows immediate non-linear acceleration due to direct industrial output, sea level rise appears more linear over short 20-year windows because the ocean's thermal inertia buffers rapid change.

---

## Google Sheets Formula Implementation

All statistical indicators and prediction errors were computed dynamically in Google Sheets using the exact setup below:

### 1. Atmospheric CO₂ Concentration (`B2:B22` vs. `A2:A22`)

* **R² Values (Coefficient of Determination)**:
  * Degree 1 (Linear): `=RSQ(B2:B22, A2:A22)`
  * Degree 2 (Quadratic): `=INDEX(LINEST(B2:B22, A2:A22^{1,2}, TRUE, TRUE), 3, 1)`
  * Degree 3 (Cubic): `=INDEX(LINEST(B2:B22, A2:A22^{1,2,3}, TRUE, TRUE), 3, 1)`
  * Degree 4 (Quartic): `=INDEX(LINEST(B2:B22, A2:A22^{1,2,3,4}, TRUE, TRUE), 3, 1)`

* **Root Mean Squared Error (RMSE)**:
  * Degree 1 (Linear via Prediction Column `G2:G22`): `=SQRT(AVERAGE(ARRAYFORMULA((B2:B22 - G2:G22)^2)))`
  * Degree 2 (Quadratic): `=SQRT(AVERAGE(ARRAYFORMULA((B2:B22 - TREND(B2:B22, A2:A22^{1,2}))^2)))`
  * Degree 3 (Cubic): `=SQRT(AVERAGE(ARRAYFORMULA((B2:B22 - TREND(B2:B22, A2:A22^{1,2,3}))^2)))`
  * Degree 4 (Quartic): `=SQRT(AVERAGE(ARRAYFORMULA((B2:B22 - TREND(B2:B22, A2:A22^{1,2,3,4}))^2)))`

---

### 2. Sea Level Rise (`D2:D22` vs. `A2:A22`)

* **R² Values (Coefficient of Determination)**:
  * Degree 1 (Linear): `=RSQ(D2:D22, A2:A22)`
  * Degree 2 (Quadratic): `=INDEX(LINEST(D2:D22, A2:A22^{1,2}, TRUE, TRUE), 3, 1)`
  * Degree 3 (Cubic): `=INDEX(LINEST(D2:D22, A2:A22^{1,2,3}, TRUE, TRUE), 3, 1)`
  * Degree 4 (Quartic): `=INDEX(LINEST(D2:D22, A2:A22^{1,2,3,4}, TRUE, TRUE), 3, 1)`

* **Root Mean Squared Error (RMSE)**:
  * Degree 1 (Linear): `=SQRT(AVERAGE(ARRAYFORMULA((D2:D22 - TREND(D2:D22, A2:A22))^2)))`
  * Degree 2 (Quadratic): `=SQRT(AVERAGE(ARRAYFORMULA((D2:D22 - TREND(D2:D22, A2:A22^{1,2}))^2)))`
  * Degree 3 (Cubic): `=SQRT(AVERAGE(ARRAYFORMULA((D2:D22 - TREND(D2:D22, A2:A22^{1,2,3}))^2)))`
  * Degree 4 (Quartic): `=SQRT(AVERAGE(ARRAYFORMULA((D2:D22 - TREND(D2:D22, A2:A22^{1,2,3,4}))^2)))`
