# 01 Data Understanding — Research Notes and Discussion

## Dataset Understanding

The Dry Bean dataset is suitable for this project because it is a supervised multi-class classification dataset with interpretable numerical features. Each sample represents one dry bean, and the target variable is the bean variety.

The dataset contains 13,611 samples, 16 numerical input features, and 7 target classes. The classes are BARBUNYA, BOMBAY, CALI, DERMASON, HOROZ, SEKER, and SIRA.

The features describe physical and morphological properties of beans, such as size, perimeter, axis length, compactness, roundness, and shape factors. This makes the dataset appropriate for explainable AI because the explanations can be connected to real, understandable bean characteristics.

This project is not only about predicting the correct bean class. The main focus is whether similar bean samples receive similar explanations from SHAP and LIME.

---

## Basic Data Quality

The dataset has no missing values. This is useful because no imputation is needed before model training.

However, 68 duplicate rows were found, which is about 0.50% of the dataset. The percentage is small, but duplicates can still affect evaluation. If duplicate samples appear in both training and test sets, the model may look slightly better than it really is.

For this reason, exact duplicate rows should be removed before train-test splitting. This will make the experiment cleaner and reduce possible data leakage.

---

## Class Distribution

The class distribution is moderately imbalanced.

| Class    | Count | Percentage |
| -------- | ----: | ---------: |
| BARBUNYA | 1,322 |      9.71% |
| BOMBAY   |   522 |      3.84% |
| CALI     | 1,630 |     11.98% |
| DERMASON | 3,546 |     26.05% |
| HOROZ    | 1,928 |     14.17% |
| SEKER    | 2,027 |     14.89% |
| SIRA     | 2,636 |     19.37% |

DERMASON is the largest class, while BOMBAY is the smallest class. The largest class is around 6.8 times larger than the smallest class.

This imbalance is important for evaluation. Accuracy alone may hide weak performance on smaller classes. For example, a model can achieve good accuracy by performing well on DERMASON and SIRA, while still making mistakes on BOMBAY.

Therefore, the evaluation should include macro precision, macro recall, macro F1-score, weighted F1-score, and a per-class confusion matrix. Macro F1-score is especially important because it gives equal importance to every class.

The train-test split should be stratified so that the class proportions remain similar in both training and testing data.

---

## Feature Distribution

The feature histograms show that the numerical features have different value ranges and different distribution shapes.

Size-related features such as Area, ConvexArea, Perimeter, MajorAxisLength, MinorAxisLength, and EquivDiameter have much larger numerical values. Shape-related features such as Solidity, ShapeFactor1, ShapeFactor2, ShapeFactor3, and ShapeFactor4 have much smaller decimal values.

Several features are skewed or multi-modal. This is expected because the dataset contains multiple bean types. Different bean classes naturally form different value groups.

This matters for the project because Logistic Regression, MLP, LIME, and nearest-neighbor-based stability analysis are sensitive to feature scale. Without scaling, large-value features such as Area and ConvexArea may dominate the distance calculations and explanation behavior.

StandardScaler should be applied after train-test splitting. The scaler should be fitted only on the training data and then applied to both training and test data.

---

## Correlation Analysis

The correlation heatmap shows that many features are strongly related to each other. This is expected because several features describe connected geometric properties of the same bean.

The strongest correlations are:

| Feature 1    | Feature 2       | Correlation |
| ------------ | --------------- | ----------: |
| Area         | ConvexArea      |      0.9999 |
| Compactness  | ShapeFactor3    |      0.9987 |
| Perimeter    | EquivDiameter   |      0.9914 |
| AspectRation | Compactness     |     -0.9877 |
| ConvexArea   | EquivDiameter   |      0.9852 |
| Area         | EquivDiameter   |      0.9850 |
| Eccentricity | ShapeFactor3    |     -0.9811 |
| AspectRation | ShapeFactor3    |     -0.9786 |
| Perimeter    | MajorAxisLength |      0.9773 |
| Eccentricity | Compactness     |     -0.9703 |

This is one of the most important findings for the XAI part of the project.

When two features are highly correlated, explanation methods may distribute importance between them differently. For example, one explanation may highlight Area, while another explanation may highlight ConvexArea. This does not always mean the model is wrong. It may happen because both features contain almost the same information.

This makes explanation stability more challenging. If similar samples receive explanations that switch between correlated features, the explanations may look unstable even when the predictions are reasonable.

For the baseline experiment, all features should be kept. Removing correlated features at the beginning may make the project less realistic. A later optional experiment can test whether removing one feature from highly correlated pairs improves explanation stability.

---

## Class-wise Feature Patterns

The boxplots show that some classes have clear differences in size-related features.

BOMBAY has much larger values for Area, Perimeter, MajorAxisLength, and MinorAxisLength compared with the other classes. This suggests that BOMBAY may be easier to separate using size-based features, even though it is the smallest class.

DERMASON appears smaller in several size-related features. SEKER shows relatively high compactness. HOROZ appears lower in compactness compared with SEKER.

The boxplots also show overlap between some classes. This means some bean varieties may be harder to separate, especially when their morphology is similar.

This should be checked later using the confusion matrix. If two classes overlap strongly in the feature plots, they may also be confused by the trained models.

---

## Importance for Explainable AI

This dataset is useful for explanation stability research because the features are meaningful. SHAP and LIME explanations can be interpreted using real bean properties.

For example:

| Feature              | Meaning                        |
| -------------------- | ------------------------------ |
| Area                 | Bean size                      |
| Perimeter            | Boundary length                |
| MajorAxisLength      | Long axis of the bean          |
| MinorAxisLength      | Short axis of the bean         |
| roundness            | How round the bean is          |
| Compactness          | How compact the bean shape is  |
| ShapeFactor features | Mathematical shape descriptors |

The strong correlations between features make the project more challenging and more research-oriented. Explanation instability may not only come from model weakness. It may also come from overlapping feature information.

This supports the main project idea: high prediction accuracy does not automatically mean that the explanations are stable or trustworthy.

---

## Main Findings

The first data analysis confirms that the dataset is appropriate for the project.

The dataset has no missing values, so preprocessing does not need imputation.

The dataset has a small number of duplicate rows. These should be removed before splitting the data.

The class distribution is imbalanced, so accuracy alone is not enough. Macro F1-score and per-class evaluation are necessary.

The features have very different scales, so standardization is necessary, especially for Logistic Regression, MLP, LIME, and nearest-neighbor stability testing.

Several features are highly correlated. This is important for SHAP and LIME because correlated features can cause explanations to shift between similar variables.

The boxplots show that some classes have clear feature differences, while others overlap. This means confusion matrix analysis will be important after model training.

---

## Decisions for the Next Stage

The preprocessing stage will remove exact duplicate rows before train-test splitting.

The target labels will be encoded numerically.

The train-test split will be stratified to preserve the class distribution.

StandardScaler will be fitted only on the training data and then applied to the training and test data.

Both scaled and unscaled datasets should be saved. The scaled version will be used for Logistic Regression, MLP, LIME, and nearest-neighbor stability analysis. The unscaled version can still be useful for interpretation and comparison.

The scaler and label encoder should be saved so that later notebooks can reproduce the same preprocessing.

---

## Model Evaluation Direction

The planned models are Logistic Regression, Decision Tree, Random Forest, and MLP.

The model comparison should not rely only on accuracy. The evaluation should include accuracy, macro precision, macro recall, macro F1-score, weighted F1-score, and confusion matrices.

The confusion matrix will be especially important for checking whether minority classes or visually overlapping classes are misclassified more often.

Training time and prediction time can also be recorded as secondary practical metrics.

---

## XAI Direction

SHAP and LIME should be applied to the same selected test samples.

The explanation comparison should include both individual explanation examples and aggregated stability scores.

The project should compare whether SHAP and LIME highlight similar features for the same sample and whether their explanations remain stable for similar samples.

Because many features are highly correlated, explanation differences should be interpreted carefully. If SHAP highlights Area and LIME highlights ConvexArea, this may reflect correlated feature behavior rather than a complete disagreement.

---

## Explanation Stability Direction

The stability experiment should use similar samples found through nearest-neighbor search on scaled features.

For each selected test sample, its nearest neighbors should be found. Then SHAP and LIME explanations should be generated for the original sample and its neighbors.

The explanation vectors can be compared using cosine similarity, Spearman rank correlation, top-k feature overlap, and explanation variance.

This will allow the project to answer the main research question:

Do similar dry bean samples receive stable and consistent local explanations across different classification models and XAI methods?

---

## Report and Presentation Notes

The dataset contains 13,611 samples, 16 numerical morphology-based features, and 7 bean classes.

The data has no missing values, but 68 duplicate rows were found.

The class distribution is imbalanced. DERMASON is the largest class, and BOMBAY is the smallest class.

The imbalance makes macro F1-score and per-class metrics necessary.

The feature distributions show different numerical scales, so feature scaling is required.

The correlation analysis shows strong relationships between several features, especially Area and ConvexArea, and Compactness and ShapeFactor3.

The high feature correlation makes the explanation stability problem more challenging.

The boxplots show that BOMBAY is visually different in size-related features, while some other classes have overlapping feature ranges.

The main research value of this dataset is that it allows both classification performance and explanation stability to be studied together.

---

## Final Interpretation

Notebook 01 supports the main direction of the project. The Dry Bean dataset is not only suitable for classification but also suitable for studying explanation stability. The data is clean, interpretable, and large enough for model comparison. At the same time, class imbalance, feature scaling issues, feature correlation, and overlapping class patterns make the problem realistic and challenging.

The next stage should prepare the dataset carefully so that the later model training, XAI explanation, and stability comparison are fair and reproducible.

*Generated using chatgpt*
