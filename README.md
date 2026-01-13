# Amazon Household Size Regression

A machine learning regression project that predicts the **number of individuals in a household** based on Amazon purchasing behavior and survey data. The project compares multiple regression models (Lasso, Ridge, Regression Tree, Random Forest, XGBoost, GAM) and evaluates performance using **RMSE and MSE**.

> Slides included: **Final Regression Report.docx**

---

## Features

### Dataset & Problem Setup
- Uses Amazon purchase history combined with survey data
- Each observation represents a **single Amazon household**
- Response variable:
  - **Household size** (integer values from 1 to 4)
- Task:
  - **Regression** (continuous prediction)
- Evaluation strategy:
  - **10-fold cross-validation**
  - Metrics:
    - **RMSE**
    - **MSE**

---

### Exploratory Data Analysis
- Household size distribution shows most households contain **1–2 individuals**
- Larger households:
  - Spend more in total
  - Purchase a greater number of unique items
  - Order more frequently
- Child-related purchases strongly correlate with larger household size
- Purchasing behavior varies geographically by state
- Most purchase-related variables are **right-skewed**, motivating log transformations

---

### Feature Engineering & Preprocessing
- Aggregated raw purchase data by household ID
- Engineered behavioral and temporal predictors including:
  - Total purchase amount
  - Unique items and categories
  - Order frequency and order period length
  - Orders per day (log-transformed)
  - Weekday vs weekend ordering
  - Holiday and back-to-school order proportions
- Created indicator and quantity variables for:
  - Child-related items
  - Pet-related items
  - Grocery and household items
- Generated categorical groupings for:
  - Average quantity (low / medium / high)
- One-hot encoded categorical variables (state, gender, day of week)
- Applied log transformations to reduce skew

---

## Models Implemented

- **Lasso Regression**
  - Automatic feature selection
  - Strong interpretability
- **Ridge Regression**
  - Shrinks correlated predictors without elimination
- **Regression Tree**
  - Captures nonlinear interactions
  - Rule-based interpretability
- **Random Forest (Vanilla & Tuned)**
  - Ensemble of trees
  - Permutation-based variable importance
- **XGBoost (Vanilla & Tuned)**
  - Gradient boosting with regularization
- **Generalized Additive Model (GAM)**
  - Natural cubic splines for nonlinear effects

---

### Model Evaluation & Tuning
- **10-fold cross-validation** used for most models
- RMSE and MSE computed using micro-aggregation
- Hyperparameters tuned via grid search or built-in CV:
  - Lasso & Ridge: lambda
  - Regression Tree: minsplit, maxdepth
  - Random Forest: max.depth, min.node.size
  - XGBoost: eta, max_depth, subsample, regularization
- XGBoost evaluated using a validation split due to computational cost

---

### Performance Summary

| Model | Validation RMSE |
|------|-----------------|
| Lasso | 1.0387 |
| Ridge | 1.0443 |
| GAM | 1.0936 |
| Regression Tree (Tuned) | 1.0557 |
| Random Forest (Vanilla) | 1.0401 |
| **Random Forest (Tuned)** | **1.0341** |
| XGBoost (Tuned, holdout) | 1.0068 |

---

### Key Findings
- **Child-related purchase quantity** is the strongest predictor of household size
- Holiday and back-to-school shopping spikes indicate larger households
- Ordering breadth (unique items & categories) strongly correlates with household size
- Ensemble models outperform linear baselines
- Random Forest captures nonlinear interactions with strong generalization

---

## Final Model

- **Tuned Random Forest** selected as final model
- Chosen over XGBoost due to:
  - Comparable performance
  - Stronger cross-validation reliability
  - Lower complexity and easier tuning
- Balances bias–variance tradeoff effectively

---

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/amazon-household-regression.git
cd amazon-household-regression
```
2. Initialize project environment:
```bash
install.packages("renv")
renv::init()
renv::activate()
```
3. Install required packages:
```bash
install.packages(c("mlr3", "mlr3learners", "glmnet", "rpart", "ranger", "xgboost", "mgcv", "ggplot2", "dplyr", "data.table"))
```

## Usage
### Run model comparison and analysis:
```bash
rmarkdown::render("Model_Comparisons.Rmd")
```
### General Figures
```bash
rmarkdown::render("Figures.Rmd")
```

## Project Structure
```bash
amazon-household-regression/
├── data/
│   ├── amazon-purchases.csv
│   └── fields.csv
│   └── survey_train_test.csv
├── Figures.Rmd
├── Model_Comparisons.Rmd
├── Final_Regression_Report.docx
├── README.md
└── LICENSE
```

## Development

### Reproducibility
- Fixed seeds for cross-validation
- Feature aggregation performed before modeling
- Log transformations applied consistently

### (Optional) Future Refactor
- Modular feature engineering pipeline
- Ensemble averaging across top models
- Incorporation of additional demographic data


## Contributions
To contribute:
1. Fork the repository
2. Create a feature branch (git checkout -b feature/YourFeature)
3. Commit your changes (git commit -m "Add YourFeature")
4. Push to the branch (git push origin feature/YourFeature)
5. Open a Pull Request

## License
This project is licensed under the MIT License.

## Acknowledgments
- mlr3 ecosystem for modeling and evaluation
- glmnet, ranger, xgboost, and mgcv packages
