# Discussion and Analysis: Notebook 06 — LIME Explanations

## Purpose of This Notebook

This notebook was created to generate LIME explanations for the same selected Dry Bean test samples that were explained with SHAP in Notebook 05. The main purpose was to add a second explainability method so that the project can later compare explanations across XAI techniques.

The project is not only about achieving high classification accuracy. The main research objective is to study explanation stability: whether similar dry bean samples receive similar explanations across models and XAI methods. Because of this objective, LIME was applied after SHAP to check whether another local explanation method identifies similar important features for the same samples.

This notebook reused the 40 selected samples from Notebook 05 instead of selecting new samples. This decision is important because SHAP and LIME must explain the same test cases for a fair comparison. If different samples were used, the later explanation stability analysis would not be reliable.

## What LIME Is

LIME stands for Local Interpretable Model-Agnostic Explanations. It is an Explainable AI method that explains individual predictions.

LIME does not try to explain the whole model directly. Instead, it explains one prediction at a time by creating small perturbations around the selected sample. These perturbed samples are passed through the trained model, and LIME observes how the model prediction changes. Then LIME fits a simple interpretable local model around that sample. The weights from this local model are used as the explanation.

In simple terms, LIME answers the question:

> For this one sample, which features most influenced the model prediction?

This makes LIME useful for this project because the research focus is local explanation stability. The project needs to know whether similar samples receive similar local explanations, especially around difficult class boundaries such as DERMASON and SIRA.

## Why LIME Was Used

LIME was used because it provides local explanations from a different perspective than SHAP.

SHAP explains feature contributions using Shapley-value-based logic. LIME explains predictions by perturbing the input locally and fitting a simple surrogate model. Because these methods work differently, comparing them can reveal whether an explanation is robust or method-dependent.

If SHAP and LIME identify similar features for the same model and same sample, the explanation can be considered more consistent. If SHAP and LIME give very different explanations, then the model prediction may be less explanation-stable, even if the classification result is correct.

This is especially important in this project because the previous evaluation showed that MLP, Logistic Regression, and Random Forest had close classification performance. However, similar performance does not necessarily mean similar explanation behavior. LIME helps test this idea more deeply.

## How LIME Was Applied in This Notebook

LIME explanations were generated for all four trained models:

- Logistic Regression
- Decision Tree
- Random Forest
- MLP Neural Network

No model was retrained in this notebook. All models were loaded from the saved model files.

The correct input type was preserved for each model:

- Logistic Regression was explained using scaled data.
- MLP was explained using scaled data.
- Decision Tree was explained using unscaled data.
- Random Forest was explained using unscaled data.

This was necessary because the explanation method must use the same input representation that the model received during training.

The notebook used the 40 selected samples from the SHAP notebook. These samples included important groups such as correctly classified DERMASON and SIRA samples, misclassified DERMASON/SIRA samples, correctly classified BOMBAY samples, and BARBUNYA/CALI confusion cases.

For each selected sample, LIME generated local explanations using 5000 perturbation samples. The notebook saved both full local explanations and top-5 local explanations. The top-5 explanation output is especially important because it will later be compared directly with the top-5 SHAP explanation output.

## Main Outputs Generated

This notebook generated and saved LIME explanation outputs for all four models.

The full local LIME explanation table contains explanations for every model, every selected sample, and every feature. Since there are 4 models, 40 selected samples, and 16 features, the combined local explanation file contains 2560 rows.

The top-5 local LIME explanation table contains the top five local features for each model and selected sample. Since there are 4 models, 40 selected samples, and 5 top features, the top-5 explanation file contains 800 rows.

The global LIME feature importance table contains feature importance values for each model and feature. Since there are 4 models and 16 features, the combined global LIME importance file contains 64 rows.

The main saved outputs include:

- `local_lime_explanations_all_models.csv`
- `local_lime_top_5_explanations_all_models.csv`
- `lime_feature_importance_all_models.csv`
- `lime_summary.json`
- `lime_raw_explanations_logistic_regression.pkl`
- `lime_raw_explanations_decision_tree.pkl`
- `lime_raw_explanations_random_forest.pkl`
- `lime_raw_explanations_mlp.pkl`
- LIME top-feature plots for all four models

These saved outputs are important because they make the notebook reproducible and allow later stability analysis without rerunning LIME.

## Global LIME Findings

The global LIME results show that all four models rely on meaningful morphological features of the dry bean samples.

Across the models, the most frequently important LIME features include:

- `ShapeFactor1`
- `Perimeter`
- `MajorAxisLength`
- `MinorAxisLength`
- `ConvexArea`
- `EquivDiameter`
- `Area`
- `roundness`

These features are all related to bean size, shape, and geometric structure. This is expected because the Dry Bean dataset is based on morphology-based measurements.

The LIME results are therefore meaningful from a domain perspective. The explanations are not random; they are connected to physical bean properties that can reasonably separate dry bean varieties.

## Decision Tree LIME Findings

For the Decision Tree model, the top LIME features were:

1. `Perimeter`
2. `MajorAxisLength`
3. `MinorAxisLength`
4. `ShapeFactor1`
5. `roundness`

This shows that LIME explains the Decision Tree mainly through size and axis-length features. This is reasonable because a Decision Tree makes predictions through feature-based splits. Features such as `Perimeter`, `MajorAxisLength`, and `MinorAxisLength` can become important if they appear in influential split points.

The Decision Tree was the weakest model in the evaluation notebook. However, it is still useful for this project because weaker or more sensitive models may produce less stable explanations. The LIME results suggest that the Decision Tree explanation is strongly focused on a small group of morphology features.

Compared with SHAP, Decision Tree LIME has partial agreement. Both methods highlight `Perimeter`, `MajorAxisLength`, `ShapeFactor1`, and `roundness`. However, the exact ranking is not identical. This suggests that Decision Tree explanations may be somewhat method-dependent.

## Logistic Regression LIME Findings

For Logistic Regression, the top LIME features were:

1. `ShapeFactor1`
2. `Perimeter`
3. `EquivDiameter`
4. `MinorAxisLength`
5. `MajorAxisLength`

This is a clean and interpretable explanation pattern. Logistic Regression relies strongly on `ShapeFactor1`, followed by size-related features such as `Perimeter`, `EquivDiameter`, `MinorAxisLength`, and `MajorAxisLength`.

This matches the expected behavior of a linear model. Logistic Regression separates the classes using weighted combinations of the input features. LIME identifies the local features that most influence those predictions.

Compared with SHAP, Logistic Regression has good agreement. Both SHAP and LIME identify `ShapeFactor1`, `MinorAxisLength`, `EquivDiameter`, and `Perimeter` as important features. This suggests that Logistic Regression may have relatively stable explanations across SHAP and LIME.

## Random Forest LIME Findings

For Random Forest, the top LIME features were:

1. `Perimeter`
2. `ConvexArea`
3. `EquivDiameter`
4. `MajorAxisLength`
5. `MinorAxisLength`

The Random Forest LIME explanations are strongly based on size and geometry. `Perimeter`, `ConvexArea`, `EquivDiameter`, and axis-length features describe the overall size and shape of the bean.

This is one of the strongest and most consistent explanation patterns in the notebook. The Random Forest LIME results are also very similar to the Random Forest SHAP results. Both explanation methods highlight features such as `Perimeter`, `ConvexArea`, `MajorAxisLength`, `MinorAxisLength`, and related geometry features.

This suggests that Random Forest may be one of the more explanation-stable models in this project. Since Random Forest averages many trees, it may produce more robust explanation patterns than a single Decision Tree.

## MLP LIME Findings

For the MLP model, the top LIME features were:

1. `ShapeFactor1`
2. `MinorAxisLength`
3. `Area`
4. `MajorAxisLength`
5. `ConvexArea`

The MLP LIME explanations show that `ShapeFactor1` is the strongest feature. However, LIME also gives strong importance to size-related features such as `MinorAxisLength`, `Area`, `MajorAxisLength`, and `ConvexArea`.

This is an important result because the MLP was the best-performing model in the evaluation notebook. However, its LIME explanations are not fully aligned with its SHAP explanations.

In the SHAP notebook, MLP emphasized:

1. `ShapeFactor1`
2. `AspectRation`
3. `roundness`
4. `ShapeFactor4`
5. `MajorAxisLength`

In this LIME notebook, MLP emphasized:

1. `ShapeFactor1`
2. `MinorAxisLength`
3. `Area`
4. `MajorAxisLength`
5. `ConvexArea`

Both SHAP and LIME agree that `ShapeFactor1` and `MajorAxisLength` are important, but they disagree on several other top features. SHAP emphasizes more shape-ratio features, while LIME emphasizes more size-based features.

This is one of the most important findings from Notebook 06. It suggests that the MLP explanation may be more dependent on the explanation method used. The model may be accurate, but its explanation pattern is not necessarily the most stable across XAI methods.

## Cross-Model LIME Comparison

The LIME results show that the models share some important features but differ in their exact rankings.

`ShapeFactor1` is highly important for Logistic Regression and MLP and also appears among the important features for Decision Tree and Random Forest. `Perimeter` is especially important for Decision Tree, Logistic Regression, and Random Forest. `MajorAxisLength` and `MinorAxisLength` appear repeatedly across all models.

This means that the models generally agree that bean morphology matters. However, they do not use the exact same morphology features in the same way.

Random Forest gives the strongest importance to size and geometry features such as `Perimeter`, `ConvexArea`, and `EquivDiameter`. Logistic Regression gives the strongest importance to `ShapeFactor1`, followed by size-related features. Decision Tree focuses strongly on `Perimeter` and axis-length features. MLP combines `ShapeFactor1` with size-related features such as `Area`, `MinorAxisLength`, and `ConvexArea`.

This supports the broader project idea that different models may reach similar classification performance while relying on different explanation patterns.

## SHAP and LIME Comparison

Although formal stability metrics are not calculated in this notebook, the LIME results can already be compared visually and conceptually with the SHAP results from Notebook 05.

Random Forest shows strong agreement between SHAP and LIME. Both methods highlight `Perimeter`, `ConvexArea`, `MajorAxisLength`, `MinorAxisLength`, and related geometry features. This suggests that Random Forest explanations may be relatively stable across XAI methods.

Logistic Regression also shows good agreement. Both SHAP and LIME identify `ShapeFactor1`, `MinorAxisLength`, `EquivDiameter`, and `Perimeter` as important features. This suggests that the linear model may have understandable and consistent explanation behavior.

Decision Tree shows partial agreement. Both SHAP and LIME identify `Perimeter`, `MajorAxisLength`, `ShapeFactor1`, and `roundness` as important, but the exact ordering differs. This may reflect the sensitivity of a single tree model.

MLP shows the most noticeable disagreement between SHAP and LIME. SHAP gives more importance to shape-ratio features such as `AspectRation`, `roundness`, and `ShapeFactor4`, while LIME gives more importance to size-related features such as `Area`, `MinorAxisLength`, and `ConvexArea`.

This is an important research observation. MLP had the highest accuracy and macro F1-score in the evaluation notebook, but its explanation behavior appears less consistent across SHAP and LIME. This supports the claim that predictive performance alone is not enough to judge model trustworthiness.

## Interpretation of the LIME Plots

The LIME bar plots show global feature importance based on the average absolute LIME weight across selected samples.

The plots show that:

- `ShapeFactor1` is highly important for Logistic Regression and MLP.
- `Perimeter` is the most important feature for Decision Tree and Random Forest.
- `MajorAxisLength` and `MinorAxisLength` appear repeatedly across models.
- `ConvexArea`, `EquivDiameter`, and `Area` are especially important for Random Forest, Logistic Regression, and MLP.
- The models mainly depend on morphology features related to size, axis length, and shape factor.

The plots are useful because they give a clear global summary of local LIME explanations. However, they should be interpreted carefully. LIME explanations are based on local perturbations, so the global plots are aggregations of local explanations, not direct full-model feature importance.

## Connection to the DERMASON and SIRA Problem

The previous evaluation notebook showed that DERMASON and SIRA were the most difficult class pair. This notebook reused the selected samples from that difficult region, including correct and misclassified DERMASON/SIRA cases.

This is important because explanation stability should be tested where the model decision is difficult. If a model gives unstable explanations around the DERMASON/SIRA boundary, then its predictions may be hard to trust even if its overall accuracy is high.

The LIME results show that important features around the selected samples are mainly related to size and shape. This suggests that DERMASON and SIRA may be difficult to separate because they overlap in some morphology measurements.

The next notebook should examine whether similar DERMASON and SIRA samples receive similar local explanations. This will connect the LIME results directly to the main explanation stability objective.

## Limitations and Cautions

There are several limitations in this notebook.

First, LIME explanations are local approximations. They are not exact explanations of the full model. LIME explains the model behavior around one selected sample by creating perturbed samples and fitting a local surrogate model.

Second, LIME depends on the perturbation process and the number of generated perturbation samples. This notebook used 5000 perturbation samples, which is a reasonable setting, but different settings could slightly change the explanation weights.

Third, LIME discretizes continuous features. This makes explanations more readable, but it can also affect how feature influence is estimated.

Fourth, raw LIME weights should not be directly compared with raw SHAP values. SHAP values and LIME weights are produced by different explanation methods. The correct comparison should use ranking-based and similarity-based metrics such as top-k overlap, Spearman rank correlation, cosine similarity, and explanation variance.

Fifth, the global LIME plots are based on the selected 40 samples, not the full test set. This was intentional because the project focuses on meaningful local explanation cases, but the results should not be interpreted as full-dataset global feature importance.

Sixth, the Dry Bean dataset contains strongly correlated features. Correlated features can cause explanation importance to shift between related variables. For example, `Area`, `ConvexArea`, `EquivDiameter`, and `Perimeter` are related size features. Similarly, `Compactness`, `ShapeFactor3`, `AspectRation`, and `Eccentricity` are related shape features. This must be considered when interpreting both SHAP and LIME explanations.

## Importance for the Main Research Objective

Notebook 06 is important because it provides the LIME side of the explanation analysis.

Notebook 05 showed how SHAP explains the models. Notebook 06 showed how LIME explains the same models and the same selected samples. Together, these two notebooks create the foundation for formal explanation stability analysis.

The results show that SHAP and LIME often agree on broad feature groups, especially morphology-related features. However, they do not always agree on the exact feature ranking. This is especially visible for the MLP model.

This supports the main research argument:

> A model can be accurate but still produce explanations that vary depending on the explanation method.

The next notebook should quantify this more formally using explanation similarity and stability metrics.

## Next Step

The next notebook should be:

`07_explanation_stability.ipynb`

This notebook should load the saved SHAP and LIME explanation files and calculate formal explanation stability metrics.

Important files for the next notebook include:

- `results/05_shap_explanations/local_shap_explanations_all_models.csv`
- `results/06_lime_explanations/local_lime_top_5_explanations_all_models.csv`
- `results/06_lime_explanations/local_lime_explanations_all_models.csv`
- `results/05_shap_explanations/selected_samples.csv`

The main analysis should include:

- SHAP vs LIME top-k feature overlap
- SHAP vs LIME feature rank similarity
- Cross-model explanation similarity
- Stability comparison by model
- Stability comparison by sample group
- Special focus on DERMASON/SIRA explanation stability

## Conclusion

Notebook 06 successfully generated LIME explanations for all four trained models using the same 40 selected samples from the SHAP notebook.

The LIME results show that all models rely on meaningful morphology-based features. The most repeated important features are `ShapeFactor1`, `Perimeter`, `MajorAxisLength`, `MinorAxisLength`, `ConvexArea`, `EquivDiameter`, `Area`, and `roundness`.

Random Forest and Logistic Regression show relatively strong agreement between SHAP and LIME. Decision Tree shows partial agreement. MLP shows the largest difference between SHAP and LIME explanations.

This is an important finding because MLP was the best-performing model, but its explanation behavior may be less consistent across explanation methods. Therefore, model accuracy alone is not enough to judge model trustworthiness.

The outputs from this notebook are ready to be used in the next stage, where explanation stability will be measured using formal similarity metrics.