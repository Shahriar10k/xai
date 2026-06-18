# Notebook 04 Discussion: Model Evaluation

## Purpose of This Stage

The purpose of this notebook was to evaluate the classification performance of the four comparative models trained in Notebook 03.

The evaluated models were:

- Logistic Regression
- Decision Tree
- Random Forest
- MLP Neural Network

The goal was not only to identify the most accurate model, but also to understand how different model types behave across the seven dry bean classes.

This evaluation stage is important because the later explainable AI analysis will depend on the trained models. Before studying explanation stability, it is necessary to know which models perform well, which models perform weakly, and which bean classes are difficult to classify.

---

## Evaluation Metrics Used

The models were evaluated using:

- Accuracy
- Macro Precision
- Macro Recall
- Macro F1-score
- Weighted F1-score
- Class-wise precision, recall, and F1-score
- Confusion matrices

Accuracy gives the overall proportion of correct predictions. However, because the dataset is imbalanced, macro F1-score is especially important. Macro F1 gives equal importance to each class, regardless of how many samples that class has.

This is useful because the Dry Bean dataset contains both large and small classes.

---

## Overall Model Performance

The MLP model achieved the best overall performance.

The overall ranking based on macro F1-score was:

1. MLP
2. Logistic Regression
3. Random Forest
4. Decision Tree

The MLP achieved the highest accuracy and macro F1-score, which suggests that the neural network was able to capture useful non-linear relationships in the dry bean morphology features.

However, the improvement of MLP over Logistic Regression and Random Forest was relatively small. This is an important finding because it shows that the Dry Bean dataset has strong feature patterns that can already be learned well by simpler models.

Logistic Regression performed surprisingly well, despite being a linear model. This suggests that some class boundaries in the feature space may be close to linearly separable.

Random Forest also performed strongly, but it did not clearly outperform Logistic Regression. This shows that a more complex ensemble model does not always guarantee a large performance improvement.

Decision Tree had the weakest overall performance. This is expected because a single decision tree can be sensitive to small changes in the data and can overfit more easily than an ensemble model.

---

## Class-wise Performance

The class-wise results showed that not all bean classes were equally difficult to classify.

BOMBAY was the easiest class. It was classified perfectly by all four models. This suggests that BOMBAY has very distinctive morphological characteristics compared with the other bean types.

SIRA was the most difficult class. It had the lowest F1-score across all models. This indicates that SIRA has overlapping feature patterns with at least one other class.

The most important confusion pattern was between DERMASON and SIRA. Many SIRA samples were predicted as DERMASON, and many DERMASON samples were predicted as SIRA. This suggests that these two classes are morphologically similar in the available feature space.

A secondary confusion pattern was observed between BARBUNYA and CALI. Several BARBUNYA samples were predicted as CALI across the models, which suggests another area of class overlap.

---

## Interpretation of Confusion Matrices

The confusion matrices confirmed the metric results.

The MLP confusion matrix showed fewer errors than the other models, especially for the difficult SIRA class.

The Decision Tree confusion matrix showed more misclassifications, especially between SIRA and DERMASON. This supports the conclusion that the Decision Tree is less robust than the other models.

The confusion matrices also showed that some classification errors are shared by multiple models. This means the errors are not only caused by a specific model. Instead, they likely come from real overlap in the feature distributions of certain bean classes.

---

## Relevance to Explainable AI Analysis

The evaluation results are important for the next stages of the project.

Since MLP achieved the best performance, it should be included in the SHAP and LIME explanation analysis.

Random Forest should also be included because it represents a strong tree-based ensemble model.

Logistic Regression is also useful as a simple and interpretable reference model because its performance was very close to the more complex models.

The DERMASON-SIRA confusion pair should be prioritized in the local explanation analysis. These classes are the most difficult to separate, so they are useful for studying whether explanation methods can reveal why the models confuse them.

The BARBUNYA-CALI confusion pair can also be used as a secondary case study.

---

## Main Findings

The main findings from this notebook are:

- MLP achieved the highest accuracy and macro F1-score.
- Logistic Regression and Random Forest performed very close to MLP.
- Decision Tree had the weakest overall performance.
- BOMBAY was classified perfectly by all models.
- SIRA was the hardest class to classify.
- DERMASON and SIRA were the most frequently confused classes.
- BARBUNYA and CALI showed a secondary confusion pattern.

---

## Conclusion

This notebook established the predictive performance foundation for the XAI part of the project.

The results show that model accuracy alone is not enough for the final analysis. Although MLP performed best, the difference between MLP, Logistic Regression, and Random Forest was relatively small.

The next step is to investigate whether these models also provide stable and consistent explanations. This will be done using SHAP, LIME, and explanation stability metrics in the following notebooks.