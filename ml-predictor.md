# House Price Prediction Pipeline

## Overview
A machine learning regression model built to predict housing prices using the Boston Housing dataset. This project demonstrates end-to-end data pipeline management, feature engineering, and a practical application of foundational statistical learning techniques.

**Algorithm:** Linear Regression
**Dataset:** Boston Housing Dataset

---

## Technical Implementation

* **Data Preprocessing:** Handled missing values and normalized numerical features to ensure gradient descent convergence and prevent features with larger scales from dominating the cost function.
* **Feature Selection:** Analyzed correlation matrices to identify the most statistically significant predictors of housing prices (e.g., crime rate, average number of rooms, pupil-teacher ratio) while checking for multi-collinearity.
* **Model Training:** Implemented a multiple linear regression model to fit a hyperplane to the multidimensional dataset, optimizing for the lowest possible mean squared error.

---

## Evaluation & Metrics

The model's performance was evaluated using standard regression metrics to quantify its predictive accuracy:

* **Mean Absolute Error (MAE):** Measured the average magnitude of the errors in a set of predictions, without considering their direction.
* **Mean Squared Error (MSE) / Root Mean Squared Error (RMSE):** Used to heavily penalize larger prediction errors, providing a clear view of the model's variance.
* **R-squared ($R^2$) Score:** Evaluated the proportion of the variance in the dependent variable that is predictable from the independent variables, ensuring the model provided a strong fit to the training data.

---

## Future Enhancements
* Implementing regularization techniques (Lasso/Ridge) to further reduce model complexity and prevent potential overfitting.
* Transitioning the model deployment into a containerized C++ inference engine to test low-latency serving capabilities.
