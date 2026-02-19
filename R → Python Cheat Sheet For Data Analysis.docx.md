# R → Python Cheat Sheet for Data Analysis

**Audience:** R users transitioning to Python for data analysis  
**Context:** Aligned with Classes 1–4 (NumPy, pandas, visualization, modeling)

---

## 1\. Core Philosophy Differences

| Concept | R | Python |
| :---- | :---- | :---- |
| Design | Statistics-first | General-purpose |
| Style | Functional, vectorized | Explicit, object-oriented |
| Defaults | Convenient | Explicit |
| Missing values | NA | np.nan, None |

**Key mindset shift:** Python requires you to say *what library* does *what task*.

---

## 2\. Objects and Assignment

| Task | R | Python |
| :---- | :---- | :---- |
| Assignment | x \<- 5 | x \= 5 |
| Check type | class(x) | type(x) |
| Help | ?mean | help(np.mean) |

---

## 3\. Vectors, Lists, and Arrays

| Concept | R | Python |
| :---- | :---- | :---- |
| Numeric vector | c(1,2,3) | np.array(\[1,2,3\]) |
| List | list(a=1,b=2) | \[1,2\] or {} |
| Indexing | x\[1\] | x\[0\] |
| Length | length(x) | len(x) |

⚠️ **Important:** Python lists are *not* vectorized.

---

## 4\. Control Flow

| Task | R | Python |
| :---- | :---- | :---- |
| If statement | if (x\>0) {} | if x \> 0: |
| For loop | for (i in 1:5) | for i in range(5): |
| Conditional vector | ifelse() | np.where() |

---

## 5\. Data Frames

| Task | R | Python (pandas) |
| :---- | :---- | :---- |
| Create | data.frame() | pd.DataFrame() |
| Dimensions | dim(df) | df.shape |
| Structure | str(df) | df.info() |
| Summary | summary(df) | df.describe() |

---

## 6\. dplyr → pandas Verbs

| dplyr | pandas |
| :---- | :---- |
| select() | df\[\[...\]\] |
| filter() | df\[condition\] or .query() |
| mutate() | .assign() |
| arrange() | .sort\_values() |
| group\_by() | .groupby() |
| summarize() | .agg() |

**Method chaining:**

(df  
 .query("price \> 200")  
 .assign(ppsqft\=**lambda** d: d.price/d.size)  
 .groupby("neighborhood")  
 .agg(mean\_ppsqft\=("ppsqft","mean"))  
)

---

## 7\. Missing Data

| Task | R | Python |
| :---- | :---- | :---- |
| Detect | is.na(x) | pd.isna(x) |
| Drop | na.omit() | .dropna() |
| Fill | ifelse(is.na()) | .fillna() |

⚠️ None ≠ np.nan (numeric work uses np.nan).

---

## 8\. Visualization

| Task | ggplot2 | Python |
| :---- | :---- | :---- |
| Base | ggplot() | matplotlib |
| Grammar-like | ggplot2 | seaborn |
| Scatter | geom\_point() | sns.scatterplot() |
| Histogram | geom\_histogram() | sns.histplot() |
| Faceting | facet\_wrap() | sns.relplot(col=...) |

⚠️ Python plots often require plt.show().

---

## 9\. Modeling: Inference vs Prediction

### Linear Regression

| R | Python |
| :---- | :---- |
| lm(y \~ x) | smf.ols("y \~ x", data=df).fit() |

### Logistic Regression

| R | Python |
| :---- | :---- |
| glm(..., binomial) | smf.logit("y \~ x", data=df).fit() |

### Prediction-focused (ML)

| Task | Python |
| :---- | :---- |
| Fit | LinearRegression().fit() |
| Predict | .predict() |
| Evaluate | mean\_squared\_error() |

---

## 10\. statsmodels vs scikit-learn

| statsmodels | scikit-learn |
| :---- | :---- |
| Inference | Prediction |
| Coefficients \+ SEs | Pipelines |
| Hypothesis tests | Cross-validation |
| R-like output | ML workflows |

---

## 11\. Common Pitfalls for R Users

* Forgetting parentheses in boolean filters

* Expecting recycling rules

* Confusing Series vs DataFrame

* Modifying data without .assign()

* Assuming plots auto-render

---

## 12\. When to Use What

* **Exploratory analysis:** pandas \+ seaborn

* **Inference / reporting:** statsmodels

* **Prediction / ML:** scikit-learn

---

## Final Advice

*Write Python like a statistician at first — clarity beats cleverness.*

This cheat sheet is designed to be used **alongside the course notebooks**.