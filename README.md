## Traffic Congestion Forecasting - Junction wise ML Pipeline
### Uber Menternship Program (UpGrad)· Nov 2025 – Jan 2026

#### Problem Statement
Urban traffic congestion is highly dynamic and varies significantly by location, time of day, and external conditions such as weather and events. A single city-level forecasting model fails to capture this variation. This project builds a junction-wise machine learning pipeline to forecast hourly traffic volume across 4 urban junctions, integrating traffic, weather, and event data to enable more reliable, location-specific congestion predictions.

#### Data
Three datasets were integrated into a unified hourly record per junction:
- Traffic volume data:- Hourly vehicle counts across 4 junctions
- Weather data:- Conditions, temperature, precipitation
- Event data:- Local events with category and estimated impact level

After integration, cleaning, and type correction, the dataset was sorted by junction and timestamp to preserve time-series integrity.

#### Methodology
**Feature Engineering**
Time-based features extracted from datetime index:
- Hour of day, day of week, is_weekend flag
- Weather condition and event impact encoded as categorical variables

**Models Trained**
Three models were trained and compared per junction:
- ARIMA:- Univariate time-series (baseline) model. Only works on historical traffic data, no exogenous variables.
- Random Forest:- Multivariate ensemble. Robust on stable, lower-volume junctions.
- Gradien Boosting:- Multivariate ensemble. Works best on high-variability junctions.

**Validation Strategy**
- Train/Test split preserving temporal order.
- TimeSeriesSplit cross-validation (rolling origin) to prevent data leakage.
- GridSearchCV used for hyperparameter tuning for high-variability junctions (1 & 3).
- Residual diagnostics to confirm errors are random, and not structurally biased.

#### Results
**Model Performance by Junction (Test Set)**
| **Junction** | **Best Model** | **R²** | **RMSE Reductions vs ARIMA**|
|-------------|----------------|--------|-----------------------------|
| Junction 1(high variability) | Gradient Boosting | ~0.93 | ~80% |
| Junction 2 | Random Forest | ~0.93 | ~55% |
| Junction 3 | Random Forest (tuned) | ~0.91 | ~45% |
| Junction 4 (stable) | Random Forest | ~0.93 | ~60% |

**Cross-validation (Best Model per Junction)**
- Stable junctions (2 & 4):- CV RMSE as low as ~ 2.6 vehicles/hour, low-variance across folds.
- High-variability junctions (1 & 3):- Higher CV RMSE reflecting intrinsic traffic volatility, not model failure
- Consistent CV vs test performance confirms models generalise well to unseen future data.

**Key finding**
ARIMA recorded negative R² across all junctions, performing worse than a simple mean predictor. This confirms that traffic forecasting in complex urban environments requires multivariate models that can incorporate weather, time, adn event signals, not just historical traffic alone.

#### Conclusions:
Junction-wise forecasting is necessary because a single city-level models alone cannot capture location-specific congestion dynamics. Pratically:
- Junction 1 (highest volume, most volatile): Needs dynamic signal timing and rapid-response monitoring during events and adverse weather.
- Junctions 2 & 3 (moderate traffic): Well-suited to scheduled interventions and planned signal adjustments.
- Junction 4 (stable, low volume): Routine monitoring is sufficient.

#### Tech Stack
Python · pandas · NumPy · scikit-learn · statsmodels · Matplotlib · Seaborn · Google Colab

