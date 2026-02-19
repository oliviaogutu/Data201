# housing.csv — Synthetic Housing Dataset (R → Python Bridge)

This dataset is a **synthetic** (simulated) housing-style dataset designed to support an R→Python bridge sequence covering:
- pandas data wrangling (including missing values),
- visualization,
- **linear regression** (Class 4),
- **logistic regression** via a derived binary outcome (Class 5).

## Files
- `housing.csv` — the dataset used throughout the unit
- (Optional) You may derive `high_price` in Class 5 as described below.

---

## Schema (Data Dictionary)

| Column | Type | Missing? | Description |
|---|---:|:---:|---|
| `listing_id` | int | No | Unique identifier for each listing. |
| `price` | int | No | Sale price in USD (rounded). Continuous outcome for linear regression. |
| `size` | float | **Yes** | Home size in square feet. Contains controlled missingness for missing-value lessons. |
| `bedrooms` | float | **Yes** | Number of bedrooms (0–6). Contains controlled missingness. |
| `neighborhood` | category (string) | No | One of: Downtown, Midtown, Uptown, Suburb, Waterfront. |
| `type` | category (string) | No | One of: Condo, Townhouse, SingleFamily, MultiFamily. |

### Derived variable used in Class 5 (not stored by default)
- **`high_price`** (binary 0/1):  
  - **Definition:** `high_price = 1{ price > median(price) }`  
  - In **R**: `high_price <- as.integer(price > median(price))`  
  - In **Python**: `df["high_price"] = (df["price"] > df["price"].median()).astype(int)`

---

## Missingness (Controlled)

Missing values are **MCAR** (missing completely at random) by design:
- `size`: ~8% missing
- `bedrooms`: ~5% missing
- `price`, `neighborhood`, `type`: **never missing**

This supports teaching `.isna()`, `.dropna()`, `.fillna()`, and discussion of preprocessing choices.

---

## Generative Specification (DGP)

The data were generated from the following process (fixed random seed for reproducibility):

### 1) Sample size and categories
- **n = 600**
- `neighborhood` sampled with probabilities:
  - Downtown 0.18, Midtown 0.22, Uptown 0.20, Suburb 0.30, Waterfront 0.10
- `type` sampled with probabilities:
  - Condo 0.30, Townhouse 0.20, SingleFamily 0.40, MultiFamily 0.10

### 2) Size (sqft)
A right-skewed distribution was produced using a log-normal model:
- Base: `log(size) ~ Normal(μ=7.45, σ=0.35)`
- Then small mean shifts were applied by neighborhood and type:
  - Neighborhood shift on `log(size)`:
    - Downtown −0.10, Midtown −0.05, Uptown 0.00, Suburb +0.10, Waterfront +0.05
  - Type shift on `log(size)`:
    - Condo −0.15, Townhouse −0.05, SingleFamily +0.10, MultiFamily −0.08
- `size` was clamped to **[450, 4500]**

### 3) Bedrooms
Bedrooms were generated to correlate with size (with noise):
- `bedrooms = round((size − 400)/600 + Normal(0, 0.6))`
- clamped to **0–6**

### 4) Price (USD)
Price was generated using a log-linear model:
- `log(price) = α + β_size * log(size) + β_bed * bedrooms + neighborhood_effect + type_effect + ε`
- Parameters:
  - `α = 10.2`
  - `β_size = 0.75`
  - `β_bed = 0.06`
  - `ε ~ Normal(0, 0.20)`
- Neighborhood effects (relative to Suburb):
  - Suburb 0.00, Downtown +0.18, Midtown +0.10, Uptown +0.07, Waterfront +0.25
- Type effects (relative to SingleFamily):
  - SingleFamily 0.00, Condo −0.10, Townhouse −0.04, MultiFamily −0.06
- `price` was clamped to **[80,000, 1,500,000]** and rounded to the nearest dollar.

### 5) Missingness applied
After price generation, MCAR missingness was applied to recorded predictors:
- `size`: ~8% set to missing
- `bedrooms`: ~5% set to missing

---

## Notes for Teaching (R → Python Bridge)

- **Inference** workflows:
  - R: `lm()` / `glm()`
  - Python: `statsmodels` formula interface (`smf.ols`, `smf.logit`)
- **Prediction** workflows:
  - R: `tidymodels` / `caret`
  - Python: `scikit-learn` (`LinearRegression`, `LogisticRegression`, train/test split, metrics)

