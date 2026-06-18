# Notebook 03 Discussion: Model Training

## Purpose of This Stage

The purpose of this notebook is to train a diverse set of machine learning models that will later be evaluated not only for predictive performance but also for explanation stability.

The main research objective of this project is to investigate whether models that achieve high classification performance also provide stable and consistent explanations. Therefore, it is necessary to train multiple models with different learning mechanisms before performing explainability analysis.

The trained models will serve as the foundation for the subsequent evaluation, SHAP analysis, LIME analysis, and explanation stability experiments.

---

## Why Model Training Is Necessary

The previous preprocessing stage prepared the Dry Bean dataset by removing duplicate samples, encoding the target labels, creating a stratified train-test split, and standardizing the feature values.

At this stage, the prepared data is used to train classification models capable of predicting bean varieties from their morphological measurements.

The trained models are required because explanation methods such as SHAP and LIME operate on model predictions. Without trained models, explanation stability cannot be studied.

---

## Why Multiple Models Were Selected

The objective is not to identify a single best-performing classifier. Instead, the objective is to compare models with fundamentally different learning strategies.

Using multiple model families makes it possible to investigate whether explanation stability depends on model architecture.

The selected models represent different levels of complexity and interpretability.

---

## Logistic Regression

Logistic Regression is a linear classification algorithm that models relationships between the input features and class probabilities using linear decision boundaries.

This model was selected because it is one of the most widely used machine learning classifiers and provides a strong linear reference model.

### Advantages

- Simple and computationally efficient.
- Easy to interpret.
- Often produces stable feature importance patterns.
- Useful benchmark for explanation analysis.

### Limitations

- Assumes approximately linear relationships.
- May struggle to capture complex feature interactions.

In this project, Logistic Regression serves as the linear reference model against which more complex models can be compared.

---

## Decision Tree

Decision Trees classify observations through a sequence of hierarchical decision rules.

This model was selected because it is inherently interpretable and provides explicit decision paths.

### Advantages

- Easy to visualize and understand.
- Naturally handles non-linear relationships.
- Produces human-readable decision rules.

### Limitations

- Sensitive to small changes in training data.
- Can easily overfit.
- May produce unstable structures.

Because decision trees are known to be sensitive to data variation, they provide an interesting case for explanation stability analysis.

---

## Random Forest

Random Forest is an ensemble learning method that combines many decision trees through bootstrap aggregation.

This model was selected because it generally achieves strong predictive performance while reducing the instability associated with individual decision trees.

### Advantages

- Strong classification performance.
- Reduced overfitting compared to a single tree.
- Robust to noise and outliers.
- Frequently performs well on tabular datasets.

### Limitations

- More difficult to interpret directly.
- Individual decision paths are less transparent.

Random Forest serves as the ensemble tree-based representative in this study.

---

## Multi-Layer Perceptron (MLP)

The Multi-Layer Perceptron is a feed-forward neural network capable of learning complex non-linear relationships.

This model was selected because neural networks represent a fundamentally different learning paradigm compared to linear models and tree-based models.

### Advantages

- Capable of learning highly complex decision boundaries.
- Can model non-linear interactions between features.
- Often achieves competitive predictive performance.

### Limitations

- Less interpretable than simpler models.
- Internal representations are difficult to explain directly.
- Explanations may be less stable.

The MLP provides a neural-network perspective for the explanation stability comparison.

---

## Expected Outcomes

At the end of this notebook, four trained models should be available for evaluation.

The outputs generated in this stage include:

- Trained model files.
- Prediction outputs.
- Prediction probabilities.
- Training time measurements.
- Model configuration files.
- Training summary reports.

These artifacts will be reused in later notebooks without retraining.

---

## Relation to the Research Objective

The ultimate goal of this project is not merely to compare classification accuracy.

Instead, the project aims to investigate the relationship between predictive performance and explanation stability.

The models trained in this notebook will later be analyzed using SHAP and LIME explanations. The resulting explanations will then be compared using stability metrics to determine whether models that perform well also produce consistent and trustworthy explanations.

This stage therefore establishes the predictive foundation required for all subsequent explainable AI experiments.