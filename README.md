# What Is the Minimum Number of Variables Needed to Predict Divorce?

**Course:** Data Science Lab for Economists (ECB3DSL), Utrecht University School of Economics
**Academic year:** 2023/2024

## Project overview

Marriage counselling questionnaires are often long, which slows down the assessment process for therapists and clients. This project asks a simple question: how few variables do we actually need to predict whether a couple has a high probability of divorce?

We use the [Divorce Predictors Scale dataset](https://www.kaggle.com/csafrit2/predicting-divorce), based on Gottman couples therapy, which contains 54 Likert-scale statements (0 = never, 4 = always) answered by 170 couples (84 divorced, 86 married), plus a binary divorce outcome. We compare five feature selection approaches to find the smallest, most predictive subset of questions.

## Research question

> What is the minimum number of variables needed to predict whether the respondent has a high probability of divorce?

## Methods

We applied and compared the following feature selection techniques:

1. **Correlation-Based Feature Selection (CBFS)** — selects features with low feature-feature correlation and high feature-class correlation.
2. **Lasso regression (L1 regularization)** — shrinks irrelevant coefficients to zero for a sparse feature set.
3. **Backward selection** — iteratively removes the least important features to minimize AIC.
4. **Principal Component Analysis (PCA)** — used to inspect the underlying variance structure and highest-loading variables (not itself a feature selection method).
5. **Random Forest** — ensemble model used to rank variable importance via Mean Decrease Gini.

## Key results

| Selection Method | Variables Selected |
|---|---|
| CBFS | Q6, Q16, Q27, Q20 |
| Lasso | Q1, Q16, Q18, Q19, Q26, Q28, Q39, Q40 |
| Backward selection | Q14, Q17, Q24, Q40 |
| **Random Forest (best model)** | **Q16, Q18, Q19** |

Random Forest was the most accurate model, with an Out-of-Bag error of 0.84% (99.16% accuracy). It identified three variables as sufficient to predict divorce probability:

- **Q16** — "We're compatible with my spouse about what love should be."
- **Q18** — "My spouse and I have similar ideas about how marriage should be."
- **Q19** — "My spouse and I have similar ideas about how roles should be in marriage."

These findings suggest that compatibility in defining love, alignment in views about marriage, and agreement on marital roles are the strongest predictors of divorce risk, reducing a 54-item questionnaire to just 3 essential questions.

## Data

- **Source:** [Divorce Predictors Scale dataset, Kaggle](https://www.kaggle.com/csafrit2/predicting-divorce)
- **Observations:** 170 respondents (54 predictor variables + 1 binary divorce outcome)
- **Scale:** 0 (Never) to 4 (Always) for all predictor items
- No missing values; statements were recoded for consistent directionality before analysis

## Repository structure

```
├── divorce.csv            # Divorce Predictors Scale dataset
├── ECB3DSL_code.Rmd        # Full analysis: descriptives, CBFS, Lasso, Backward Selection, PCA, Random Forest
├── report.pdf              # Final written paper
└── README.md
```

## Requirements

The analysis is written in R (R Markdown). It uses the following packages:

```r
install.packages(c(
  "readr", "dplyr", "ggplot2", "ggthemes",
  "corrplot", "factoextra", "glmnet", "caret",
  "magrittr", "lattice", "randomForest",
  "leaps", "MASS", "likert", "xtable"
))
```

## How to run

1. Place `divorce.csv` in the same directory as `ECB3DSL_code.Rmd`.
2. Open `ECB3DSL_code.Rmd` in RStudio.
3. Run the chunks in order — each section (logistic regression, correlation plot, PCA, Lasso, Random Forest, backward selection, descriptive analysis, visualizations) is self-contained but some later chunks (e.g. Random Forest) reuse data objects created earlier in the file, so running top to bottom is recommended.

## Limitations

- The dataset may not capture the full range of variables influencing marital outcomes, and results may not generalize across demographic or cultural contexts.
- High multicollinearity among variables makes causal interpretation difficult.
- Findings are based on a single cultural context (Turkey) and cross-sectional data; longitudinal validation is needed.

## Disclosure

Generative AI (ChatGPT) was used during this project to support clear and engaging communication throughout the write-up.

## License

This project is licensed under the [MIT License](LICENSE).
