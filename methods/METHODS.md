# Methods

## Problem Definition

The task is multi-class classification: predict a broad food classification group
from numerical nutrient values in the Australian Food Composition Database.

The target is derived from the first two digits of the food classification code.
Food names, detailed descriptions, classification codes, and administrative or
derivation fields are excluded from the predictors to reduce label leakage.

## Final Dataset

- 1,253 food items
- 90 numerical nutrient features
- 8 broad food groups
- Groups with fewer than 50 rows removed

## Preprocessing

1. Load the nutrient profile table for solids and liquids per 100 g.
2. Construct the broad target from the classification code.
3. Remove rows where the target cannot be created.
4. Remove food groups with fewer than 50 samples.
5. Exclude food names, label-related information, and non-nutrient administrative fields.
6. Keep numerical nutrient columns as predictors.
7. Use a stratified train/test split.
8. Apply median imputation to missing nutrient values.
9. Apply standard scaling to scale-sensitive models such as kNN and logistic regression.

Median imputation was selected because nutrient variables may contain strong
outliers, making the median less sensitive than the mean.

## Models Compared

- Dummy classifier (most frequent class)
- k-nearest neighbours, k = 7
- Logistic regression
- Decision tree
- Random forest

The Random Forest configuration reported in the appendix uses 300 trees,
`random_state=42`, and `class_weight="balanced"`.

## Evaluation

The analysis reports:

- Accuracy
- Balanced accuracy
- Macro F1
- 5-fold cross-validation on the training data
- Final evaluation on a held-out test set

Balanced accuracy is emphasised because the retained food groups are not equally represented.

## Feature Analysis

Random Forest feature importance is used as an algorithmic check of which
nutrient variables contribute most strongly to predictions.

The most important reported features include:

- Total dietary fibre
- Starch
- C22:6w3
- Saturated fatty-acid measures
- Available carbohydrate
- Long-chain omega-3 fatty acids
- Nitrogen
- Selenium
- Monounsaturated fatty acids
- Iron
- Protein
- Trans fatty acids
- Iodine

The report explicitly treats feature importance as predictive evidence, not causal evidence.

## PCA Decision

PCA was considered but not used in the final models because:

- the 90-feature dataset was computationally manageable,
- PCA components would be harder to interpret nutritionally,
- Random Forest can model non-linear feature interactions without requiring uncorrelated inputs.

Interpretability was therefore prioritised over dimensionality reduction.
