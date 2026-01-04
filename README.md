# Sleep Score Prediction from Wearable Data
## 1. Project Overview

This project explores the use of machine learning techniques to analyze and predict sleep quality using data exported from the Samsung Health app.
The main objective is not only to achieve good predictive performance, but also to evaluate model validity, focusing on feature leakage, temporal dependencies, and data limitations.

The dataset consists of sleep-related metrics collected via a wearable device over four months.

## 2. Exploring the Data

The initial exploration included:

* Data type validation and cleaning
* Removal of empty or non-informative columns
* Scatter plots and correlation analysis between features and sleep_score

Key observations:

* Several features show strong linear correlation with the target.
* Some features are clearly components used internally to compute the sleep score, indicating potential feature leakage.

## 3. Models Implemented
### 3.1 Linear Regression

The following approaches were tested:
* Single-feature regressions on features that showed stronger relationships during exploration (mental recovery and REM duration).
* Multivariate linear regression using all available features.

Result:
* Excellent performance when leakage exists, but limited generalization.

### 3.2 Neural Network Regression

A Multilayer Perceptron (MLP) was trained using PyTorch with:
* Input scaling using StandardScaler
* Mean Squared Error (MSE) loss
* Adam optimizer

Results:
* With leakage: RMSE ≈ 4.3
* Without leakage: RMSE ≈ 7.2

This confirmed that the neural network was learning meaningful non-linear relationships once leaked features were removed.

### 3.3 Temporal Modeling

To simulate a more realistic prediction scenario, temporal windows were introduced:
* Input: sleep data from a previous number of days
* Output: next-day sleep score

Results:
* Initial tests using 14-day windows and two hidden layers resulted in RMSE values between 16 and 18, with training loss close to zero, indicating overfitting due to: Small dataset size and High feature dimensionality
* Reducing the window size to 5 days and using a single hidden layer improved stability, with RMSE decreasing to approximately 14–16.

This result represents a realistic performance ceiling for the dataset.

## 4. Limitations

* Small dataset (125 samples)
* Single individual data
* Proprietary nature of the sleep score calculation
* Limited external context (stress, activity, caffeine, etc.)

## 5. Conclusion

This project shows that machine learning performance must be interpreted within the context of how the data is generated and what each feature represents.
While simple models can achieve impressive metrics, careful analysis reveals when those results are misleading.

The final models prioritize realistic generalization, making this project a practical case study in applied machine learning rather than pure metric optimization.