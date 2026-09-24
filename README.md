# Predicting Food Groups from Nutrient Profiles

A machine-learning project that predicts broad food classification groups from
numerical nutrient composition data using the **Australian Food Composition Database**.

The project focuses on the full tabular machine-learning workflow:
problem formulation, leakage-aware feature selection, missing-data handling,
class imbalance, model comparison, cross-validation, model interpretation, and
careful evaluation.

## Project Summary

The input to the model is a vector of numerical nutrient values for a food item.
The target is a broad food group derived from the first two digits of the food
classification code.

To keep the experiment focused on nutrient composition rather than obvious text
clues, food names and other label-related fields were excluded from the model inputs.

After preprocessing, the final dataset contained:

- **1,253 food items**
- **90 numerical nutrient features**
- **8 broad food groups**

Food groups with fewer than 50 samples were removed to make evaluation more stable.

## Machine Learning Pipeline

The workflow included:

1. Constructing the broad multi-class target
2. Removing label-leakage and administrative columns
3. Retaining numerical nutrient variables
4. Stratified train/test splitting
5. Median imputation for missing values
6. Standard scaling for scale-sensitive models
7. 5-fold cross-validation on the training set
8. Final evaluation on a held-out test set
9. Random Forest feature-importance analysis

## Models Compared

| Model | CV Balanced Accuracy | Test Accuracy | Test Balanced Accuracy |
|---|---:|---:|---:|
| Dummy baseline | 0.125 | 0.283 | 0.125 |
| kNN (k=7) | 0.820 | 0.889 | 0.837 |
| Logistic Regression | 0.882 | 0.927 | 0.918 |
| Decision Tree | 0.845 | 0.924 | 0.903 |
| **Random Forest** | **0.906** | **0.981** | **0.974** |

The **Random Forest** achieved the strongest final result:

- **Test accuracy: 0.981**
- **Test balanced accuracy: 0.974**
- **Test macro F1: 0.976**

Balanced accuracy was treated as a key metric because the food-group classes are
not equally represented.

## Exploratory Data Analysis

### Class Distribution

The retained food groups remain imbalanced, which motivates the use of balanced
accuracy and macro-level metrics rather than relying only on ordinary accuracy.

![Class distribution](results/class_distribution.png)

### Missing Nutrient Values

Missingness varies substantially across nutrient features, so missing-value
handling is included as part of the modelling pipeline.

![Missing values](results/missing_values.png)

## Model Comparison

The comparison shows that all learned models substantially outperform the dummy
baseline. Logistic Regression performs strongly, suggesting that some food groups
are separable using relatively simple combinations of nutrient values, while the
Random Forest provides the strongest overall performance.

![Model comparison](results/model_comparison.png)

## Random Forest Evaluation

### Confusion Matrix

Most predictions lie on the diagonal of the row-normalised confusion matrix,
showing strong performance across the retained food groups rather than only on
the largest classes.

![Random forest confusion matrix](results/confusion_matrix.png)

### Feature Importance

The Random Forest relies on nutritionally meaningful variables including dietary
fibre, starch, fatty-acid measurements, carbohydrate measures, nitrogen, selenium,
iron, protein, and iodine.

![Random forest feature importance](results/feature_importance.png)

Feature importance is interpreted as evidence of predictive usefulness rather
than a causal relationship between a nutrient and a food category.

## Data Leakage and Model Validity

A key design decision was to exclude food names and detailed food descriptions.
Using names such as *bread*, *milk*, *beef*, or *apple* could allow the classifier
to infer the target from language rather than from nutrient composition.

Removing these fields makes the task better aligned with the research question:
**how much information about food group is contained in the numerical nutrient profile itself?**

## Why PCA Was Not Used

PCA was considered as a dimensionality-reduction method but was not selected for
the final modelling pipeline.

The main reasons were:

- 90 numerical features were still computationally manageable
- PCA components would reduce interpretability because each component mixes many nutrients
- the best-performing Random Forest can model non-linear interactions directly

For this project, retaining interpretable nutrient variables was more useful than
compressing the feature space.

## Limitations

The reported results should be interpreted within the scope of the experiment.

- Rare food groups with fewer than 50 samples were excluded.
- The model predicts broad groups rather than detailed food categories.
- Median imputation is a simple missing-data strategy.
- Only a moderate set of models and hyperparameters was explored.
- High performance is partly expected because food-group labels are naturally related to nutrient composition.

Potential extensions include comparing alternative imputation methods, testing
reduced-feature models, tuning model hyperparameters more extensively, and
evaluating more detailed food categories.

## Technologies

- Python
- pandas
- NumPy
- scikit-learn
- Matplotlib
- Machine Learning
- Multi-class Classification
- Model Evaluation
- Feature Importance

## Repository Structure

```text
food-group-ml-portfolio/
├── README.md
├── PROJECT_CONTEXT.md
├── methods/
│   └── METHODS.md
├── report/
│   └── COMP4702_ML_Assignment_Portfolio.pdf
└── results/
    ├── class_distribution.png
    ├── missing_values.png
    ├── model_comparison.png
    ├── confusion_matrix.png
    ├── feature_importance.png
    └── model_results.csv
```

## Project Context

This project was developed as part of **COMP4702 Machine Learning**
at **The University of Queensland**.

The repository has been reorganised as a technical portfolio so that the
problem definition, modelling decisions, evaluation results, and interpretation
can be reviewed without relying on the original assignment structure.
