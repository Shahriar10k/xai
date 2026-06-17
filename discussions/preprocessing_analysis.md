# 02 Preprocessing — Research Notes and Discussion

## Preprocessing Outcome

The preprocessing stage successfully prepared the Dry Bean dataset for model training, explainable AI analysis, and explanation stability testing.

The original dataset contained 13,611 samples. After removing 68 exact duplicate rows, the cleaned dataset contains 13,543 samples. This duplicate removal is useful because exact repeated samples could create a small risk of data leakage if the same observation appeared in both training and test sets.

After preprocessing, the dataset still contains all 7 bean classes and all 16 numerical morphology-based input features.

The target column is `Class`, and the class labels were encoded numerically as follows:

| Class | Encoded Label |
|---|---:|
| BARBUNYA | 0 |
| BOMBAY | 1 |
| CALI | 2 |
| DERMASON | 3 |
| HOROZ | 4 |
| SEKER | 5 |
| SIRA | 6 |

This label encoding is necessary for model training, but the original class names are still preserved in the saved target files. This is important because later evaluation tables, confusion matrices, SHAP plots, and LIME explanations should use readable class names.

---

## Duplicate Removal

Exact duplicate rows were removed before train-test splitting.

This is an important methodological choice. If duplicates are removed after splitting, duplicate samples may already exist across train and test sets. That would make the test set less independent and could slightly inflate model performance.

The number of removed duplicates is small, so this step should not negatively affect the dataset. Instead, it makes the experiment cleaner and more reliable.

| Stage | Samples |
|---|---:|
| Before duplicate removal | 13,611 |
| Duplicates removed | 68 |
| After duplicate removal | 13,543 |

This decision should be mentioned in the report because it shows that the preprocessing stage considered evaluation fairness, not only technical cleaning.

---

## Train-Test Split

The cleaned dataset was split into training and test sets using an 80/20 split.

| Split | Samples |
|---|---:|
| Training set | 10,834 |
| Test set | 2,709 |

A stratified split was used. This was the correct choice because the class distribution is imbalanced. Stratification keeps the class proportions almost identical in the full cleaned dataset, training set, and test set.

The train-test class distribution plot confirms that the class proportions were preserved after splitting. DERMASON remains the largest class in both sets, and BOMBAY remains the smallest class in both sets.

This is important because the later model comparison will not be affected by a badly distributed test set.

---

## Feature Scaling

StandardScaler was applied to the input features.

The scaler was fitted only on the training data and then applied to both training and test features. This is the correct method because the test set must remain unseen during preprocessing.

Before scaling, features had very different numerical ranges. For example, `Area` and `ConvexArea` have very large values, while shape factor features have very small decimal values. Without scaling, models and distance-based methods could be dominated by large-scale features.

After scaling, the training features have means close to 0 and standard deviations close to 1. This confirms that scaling worked correctly.

The scaling plot for `Area` shows that scaling changes the numerical range but does not change the shape of the distribution. The feature is still skewed after scaling, but it is now centered and standardized. This is expected because StandardScaler standardizes magnitude; it does not make a skewed feature normally distributed.

---

## Why Scaling Matters for This Project

Scaling is necessary for Logistic Regression and MLP because these models are sensitive to feature magnitude.

Scaling is also important for LIME because LIME creates local perturbations around a sample. If feature scales are very different, the perturbation space may become biased.

Scaling is especially important for explanation stability because similar samples will be found using nearest-neighbor search. Without scaling, features such as `Area`, `ConvexArea`, or `Perimeter` could dominate the distance calculation, while smaller-scale shape features would have less influence.

Therefore, the scaled dataset should be used for:

- Logistic Regression
- MLP
- LIME
- nearest-neighbor search
- explanation stability metrics

Tree-based models such as Decision Tree and Random Forest do not strictly need scaling. However, using the same scaled feature set for all models can keep the pipeline consistent. If needed, an additional comparison can later check tree models on unscaled data.

---

## Methodological Decisions

The preprocessing choices are appropriate for the project.

Duplicate removal improves evaluation fairness.

Stratified train-test splitting preserves class balance.

Label encoding makes the target usable for machine learning while preserving the class mapping.

StandardScaler makes the features suitable for Logistic Regression, MLP, LIME, and nearest-neighbor-based stability analysis.

Saving both scaled and unscaled features is useful. The scaled data supports training and stability calculations, while the unscaled data remains useful for interpretation and checking original feature values.

---

## Research Implications

The preprocessing stage supports the main research question by preparing the data in a fair and reproducible way.

The stratified split ensures that the model evaluation will reflect the original class distribution.

The scaling step ensures that explanation stability analysis will not be unfairly dominated by large-scale features.

The preserved class labels and saved label mapping will make final results easier to explain in the report and presentation.

This preprocessing stage is especially important because the project is not only comparing classification accuracy. It is also comparing how stable and trustworthy the explanations are for similar samples.

---

## Final Notes for Report and Presentation

The dataset was cleaned by removing 68 exact duplicate rows before train-test splitting.

The final cleaned dataset contains 13,543 samples.

The data was split into 10,834 training samples and 2,709 test samples using an 80/20 stratified split.

The stratified split preserved the original class distribution very well.

The target labels were encoded numerically, but the original class names were preserved for interpretation.

StandardScaler was fitted only on the training data and then applied to both training and test data.

The scaling process standardized feature magnitudes while preserving the original distribution shape.

The preprocessing output is now ready for model training, model evaluation, SHAP analysis, LIME analysis, and explanation stability testing.

---

## Next Stage

The next notebook should train the four planned models:

- Logistic Regression
- Decision Tree
- Random Forest
- MLP

The next stage should save trained models, predictions, training time, and model configurations.

Model evaluation should include accuracy, macro precision, macro recall, macro F1-score, weighted F1-score, and confusion matrices.

The main concern in the next stage is not only which model is most accurate, but also which model provides a strong base for stable and trustworthy explanations.
