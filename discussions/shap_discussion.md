# Discussion and Analysis: Notebook 05 — SHAP Explanations

## Purpose of This Notebook

This notebook was created to generate SHAP explanations for the trained Dry Bean classification models. In the previous notebooks, the dataset was explored, preprocessed, models were trained, and model performance was evaluated. Those earlier steps showed how well each model classified the bean samples, but they did not explain why each model made its predictions.

The purpose of this notebook was therefore to move from model performance analysis to explainability analysis. Since the main research objective of this project is explanation stability, it is not enough to know which model has the highest accuracy or macro F1-score. The project must also investigate whether the models produce similar explanations for similar samples, especially around difficult classification boundaries.

SHAP was used in this notebook as the first explainability method. The SHAP explanations generated here will later be compared with LIME explanations and then used in the explanation stability notebook.

## What SHAP Is

SHAP stands for SHapley Additive exPlanations. It is an Explainable AI method used to estimate how much each input feature contributes to a model prediction.

For a single prediction, SHAP explains how the model moves from a baseline prediction to the final prediction. Each feature receives a SHAP value. A positive SHAP value means the feature pushes the model output toward the explained class. A negative SHAP value means the feature pushes the model output away from the explained class.

In simple terms, SHAP answers the question:

> Which features were most responsible for this model prediction, and in what direction did they influence the prediction?

This is useful for the Dry Bean dataset because the features are interpretable morphological measurements, such as `Area`, `Perimeter`, `MajorAxisLength`, `MinorAxisLength`, `ShapeFactor1`, `roundness`, and `Compactness`. These features describe the shape and size of bean samples, so SHAP can help explain which physical properties influence the model decisions.

## Why SHAP Was Used

SHAP was used because it supports both global and local explanation analysis.

Global SHAP analysis shows which features are generally important for a model across the selected samples. This helps identify the main morphological features used by each classifier.

Local SHAP analysis explains individual predictions. This is important because the main project question is about explanation stability for similar samples. For example, if two SIRA samples are morphologically similar, their local explanations should ideally also be similar. If their explanations are very different, then the model may be accurate but not explanation-stable.

SHAP was also useful because this project includes different model families:

- Logistic Regression
- Decision Tree
- Random Forest
- MLP Neural Network

These models learn decision patterns differently. Logistic Regression is linear, Decision Tree is rule-based, Random Forest is an ensemble of trees, and MLP is a neural network. SHAP allows explanation outputs to be created for all of them, making it possible to compare how different model types use the same input features.

## How SHAP Was Applied in This Notebook

SHAP explanations were generated for all four trained models. The models were loaded from disk, and no model was retrained in this notebook.

The correct input type was preserved for each model:

- Logistic Regression was explained using scaled data.
- MLP was explained using scaled data.
- Decision Tree was explained using unscaled data.
- Random Forest was explained using unscaled data.

Different SHAP explainers were used depending on the model type. Tree-based SHAP was used for the Decision Tree and Random Forest models. Linear SHAP was used for Logistic Regression. Kernel SHAP was used for the MLP model because the MLP is a neural network and requires a model-agnostic explanation approach.

The notebook selected 40 meaningful test samples for explanation. The selected samples focused mainly on the most important confusion patterns found in the evaluation stage:

- Correctly classified DERMASON samples
- Correctly classified SIRA samples
- Misclassified DERMASON as SIRA samples
- Misclassified SIRA as DERMASON samples
- Correctly classified BOMBAY samples
- BARBUNYA samples misclassified as CALI

DERMASON and SIRA were prioritized because they formed the strongest confusion pair in the model evaluation notebook. BOMBAY was included as an easier contrast class because it was classified very well by all models.

These selected samples were saved so that the same samples can be reused in the LIME notebook. This is important because SHAP and LIME should explain the same test cases if they are going to be compared fairly later.

## Main Outputs Generated

This notebook produced both global and local SHAP outputs.

The global SHAP feature importance table contains feature importance scores for each model. Since there are four models and sixteen features, the combined global feature importance file contains 64 rows.

The local SHAP explanation table contains the top five explanation features for each selected sample and model. Since there are four models, 40 selected samples, and five features per local explanation, the combined local explanation file contains 800 rows.

The notebook saved the following main outputs:

- `selected_samples.csv`
- `shap_values_logistic_regression.pkl`
- `shap_values_decision_tree.pkl`
- `shap_values_random_forest.pkl`
- `shap_values_mlp.pkl`
- `shap_feature_importance_all_models.csv`
- `local_shap_explanations_all_models.csv`
- `shap_summary.json`
- SHAP bar plots for all four models
- SHAP summary plots for all four models

## Global SHAP Findings

The global SHAP results show that all models rely on morphological bean features, but they do not rank the same features in the same order.

This is one of the most important findings of the notebook. The models have similar classification goals and are trained on the same dataset, but their explanations are not identical. This supports the main idea of the project: model accuracy alone does not guarantee explanation consistency.

Across the models, the most frequently important features include:

- `ShapeFactor1`
- `Perimeter`
- `MajorAxisLength`
- `MinorAxisLength`
- `ConvexArea`
- `EquivDiameter`
- `roundness`
- `Compactness`
- `AspectRation`
- `ShapeFactor4`

These features are all related to bean size and shape. This makes sense because the Dry Bean dataset is based on morphological measurements.

## Model-Specific SHAP Findings

### Logistic Regression

For Logistic Regression, the top features were:

1. `ShapeFactor1`
2. `MinorAxisLength`
3. `EquivDiameter`
4. `Perimeter`
5. `ConvexArea`

Logistic Regression relies strongly on size and shape measurements. Since it is a linear model, its explanations are more directly connected to weighted feature contributions. However, its raw SHAP values should not be directly compared with the raw SHAP values from other models because the model uses scaled input data and a different SHAP explainer.

### Decision Tree

For Decision Tree, the top features were:

1. `MajorAxisLength`
2. `Perimeter`
3. `ShapeFactor1`
4. `Compactness`
5. `roundness`

The Decision Tree explanation is concentrated around a smaller number of features. This is expected because a single tree makes predictions through feature-based splits. The model depends strongly on `MajorAxisLength` and `Perimeter`, which may make its explanations more sensitive to small changes in the data.

### Random Forest

For Random Forest, the top features were:

1. `Perimeter`
2. `ShapeFactor1`
3. `ConvexArea`
4. `MajorAxisLength`
5. `MinorAxisLength`

Random Forest uses a more balanced combination of size-based and shape-based features. Compared with a single Decision Tree, it is expected to be more robust because it averages many trees.

### MLP Neural Network

For MLP, the top features were:

1. `ShapeFactor1`
2. `AspectRation`
3. `roundness`
4. `ShapeFactor4`
5. `MajorAxisLength`

The MLP gives strong importance to shape-ratio features. Compared with Random Forest, which emphasizes `Perimeter`, `ConvexArea`, and axis-length features, MLP places more emphasis on `ShapeFactor1`, `AspectRation`, `roundness`, and `ShapeFactor4`. This suggests that the neural network may be learning nonlinear shape relationships.

## Cross-Model Comparison

The SHAP results show both agreement and disagreement across models.

There is agreement that morphological features are important. `ShapeFactor1` appears as a highly important feature in all four models. `Perimeter`, `MajorAxisLength`, `MinorAxisLength`, and `ConvexArea` also appear repeatedly among the top features.

However, the models disagree on the exact ranking of these features. Random Forest gives highest importance to `Perimeter`, MLP gives highest importance to `ShapeFactor1`, Decision Tree gives highest importance to `MajorAxisLength`, and Logistic Regression gives highest importance to `ShapeFactor1`.

This is important because MLP, Logistic Regression, and Random Forest had close predictive performance in the previous evaluation notebook, but their explanation patterns are not identical. Therefore, similar accuracy does not necessarily mean similar explanation behavior.

This finding supports the central motivation of the project: explanation stability should be evaluated separately from classification performance.

## Interpretation of SHAP Bar Plots

The SHAP bar plots summarize the most important features for each model using mean absolute SHAP values.

The plots show that:

- `ShapeFactor1` is consistently important across models.
- `Perimeter` is especially important for Random Forest and Decision Tree.
- `MajorAxisLength` is especially important for Decision Tree.
- `AspectRation`, `roundness`, and `ShapeFactor4` are especially important for MLP.
- Logistic Regression gives strong importance to several size-related features, including `MinorAxisLength`, `EquivDiameter`, `Perimeter`, `ConvexArea`, and `Area`.

These plots are useful for comparing feature rankings. However, raw SHAP magnitudes should not be directly compared across models because different explainers and input scales were used.

## Interpretation of SHAP Summary Plots

The SHAP summary plots provide more detailed explanation behavior than the bar plots. Each point represents one selected sample. The x-axis shows the SHAP value, and the color represents the feature value.

The Random Forest summary plot shows that `Perimeter`, `ConvexArea`, `EquivDiameter`, `ShapeFactor1`, and axis-length features have strong influence. This suggests that Random Forest uses a broad combination of size and shape information.

The MLP summary plot shows strong influence from `ShapeFactor1`, `roundness`, `MajorAxisLength`, `AspectRation`, and `ShapeFactor4`. This suggests that MLP uses shape-ratio information strongly.

The Logistic Regression summary plot shows larger SHAP values for some features. This should be interpreted carefully because Logistic Regression uses scaled data and Linear SHAP.

The Decision Tree summary plot shows strong influence from `MajorAxisLength`, `Perimeter`, and `ShapeFactor1`, confirming that the tree structure depends heavily on a few split-driving features.

## Connection to DERMASON and SIRA Confusion

The previous evaluation notebook found that DERMASON and SIRA were the strongest confusion pair. This notebook selected several samples from this difficult boundary, including correctly classified and misclassified DERMASON/SIRA cases.

This is important because explanation stability should be tested in difficult regions of the feature space. If explanations are unstable around the DERMASON/SIRA boundary, then a model may be accurate overall but unreliable in its reasoning for borderline cases.

The SHAP results suggest that different models use different combinations of shape and size features for classification. For DERMASON and SIRA, small differences in morphology may change not only the predicted class but also the explanation.

## Limitations

The SHAP results in this notebook should be interpreted with some caution.

First, the global SHAP results are based on the selected 40 samples, not the full test set. This was intentional because the project focuses on meaningful local explanation cases, but the results should not be treated as full-dataset global explanations.

Second, the selected sample groups are not perfectly balanced because duplicate sample IDs were removed after selection from multiple models.

Third, raw SHAP values should not be compared directly across model types. The correct comparison should use feature rankings, top-k feature overlap, rank correlation, and local explanation similarity.

Fourth, Kernel SHAP for MLP is an approximation and depends on the selected background samples.

Fifth, the Dry Bean dataset contains highly correlated features. Correlated features can cause explanation importance to shift between related variables, such as `Area`, `ConvexArea`, `EquivDiameter`, and `Perimeter`, or between `Compactness`, `ShapeFactor3`, `AspectRation`, and `Eccentricity`.

## Importance for the Main Research Objective

This notebook is an important step toward the main explanation stability objective.

It shows that all models use meaningful morphological features, but they do not produce identical feature rankings. This means that model performance and explanation behavior are different aspects of trustworthiness.

The SHAP explanations generated here will be reused later when comparing SHAP with LIME and when measuring explanation stability. The next notebook should generate LIME explanations for the same selected samples so that both XAI methods can be compared fairly.

## Conclusion

Notebook 05 successfully generated SHAP explanations for all four trained models.

The results show that the models rely on meaningful bean morphology features, especially shape and size measurements. However, the exact feature rankings differ across models.

`ShapeFactor1` is one of the most consistently important features. Random Forest emphasizes `Perimeter`, `ConvexArea`, and axis-length features. MLP emphasizes shape-ratio features such as `ShapeFactor1`, `AspectRation`, `roundness`, and `ShapeFactor4`. Decision Tree relies strongly on `MajorAxisLength` and `Perimeter`, while Logistic Regression uses a combination of shape factor and size-related variables.

The main conclusion is that similar model performance does not guarantee similar explanations. This supports the need for the next stages of the project, where LIME explanations and formal stability metrics will be used to study explanation consistency more deeply.