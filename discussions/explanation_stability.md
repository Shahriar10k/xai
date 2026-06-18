# Discussion and Analysis: Notebook 07 — Explanation Stability Analysis

## Purpose of This Notebook

This notebook was created to measure explanation stability in the Dry Bean classification project. The previous notebooks generated model predictions, evaluated classification performance, and produced SHAP and LIME explanations. However, those notebooks did not yet answer the main research question of the project:

> Do similar dry bean samples receive similar and stable explanations across models and XAI methods?

This notebook is the main explanation-stability stage of the project. It moves beyond model accuracy and directly compares explanation behavior.

The notebook used the SHAP explanations from Notebook 05 and the LIME explanations from Notebook 06. Both explanation methods were applied to the same 40 selected samples and the same four trained models:

- Logistic Regression
- Decision Tree
- Random Forest
- MLP Neural Network

The analysis focused on top-5 explanation features. This means that for each model, method, and sample, the five most important explanation features were compared.

## Why Explanation Stability Matters

Model accuracy tells us whether a model predicts correctly. However, accuracy does not tell us whether the model explains its predictions consistently.

A model can have high accuracy but still give unstable explanations. For example, two similar bean samples may receive the same predicted class but very different explanation features. In that case, the model may appear accurate, but its reasoning is harder to trust.

This project is based on the idea that trustworthy machine learning should consider both prediction performance and explanation behavior. Therefore, Notebook 07 evaluates whether explanations are consistent across:

- SHAP and LIME
- Different machine learning models
- Similar sample groups
- Nearest-neighbor samples in feature space

This is important because explanation stability provides another dimension of model trustworthiness.

## Explanation Stability Metrics Used

Several similarity metrics were used in this notebook.

The most important metric was **top-5 feature overlap**. This measures how many of the top five explanation features are shared between two explanations. For example, if two explanations share three out of five top features, the top-5 overlap is 0.60.

The notebook also calculated:

- Jaccard similarity
- Spearman rank correlation
- Signed cosine similarity
- Absolute cosine similarity

Top-5 overlap is the easiest metric to interpret because it directly measures whether the same important features appear in two explanations.

Cosine similarity is useful because it compares explanation vectors. Absolute cosine similarity ignores the sign and focuses on strength similarity, while signed cosine similarity considers direction.

Rank correlation is stricter because it checks whether shared features also appear in similar order. In several results, rank correlation was lower than top-5 overlap, which means explanations often agreed on which features mattered but did not always agree on the exact ranking.

## Main Analyses Performed

This notebook performed four main explanation stability analyses.

First, it measured **SHAP vs LIME agreement** for the same model and same sample. This analysis checks whether two different XAI methods explain the same model prediction similarly.

Second, it measured **cross-model stability**. This analysis checks whether different models produce similar explanations for the same sample when using the same XAI method.

Third, it measured **within-group stability**. This analysis checks whether samples from the same selected group receive similar explanations. The groups include correct DERMASON samples, correct SIRA samples, DERMASON predicted as SIRA, SIRA predicted as DERMASON, correct BOMBAY samples, and BARBUNYA predicted as CALI samples.

Fourth, it measured **nearest-neighbor explanation stability**. This analysis checks whether samples that are close to each other in scaled feature space receive similar explanations.

The nearest-neighbor analysis is especially important because it directly tests the central idea of this project: similar samples should ideally receive similar explanations.

## Overall Stability Findings

The overall results show that explanation stability exists, but it is not equally strong in all situations.

The average top-5 overlap scores were:

| Analysis Type | Mean Top-5 Overlap | Interpretation |
|---|---:|---|
| SHAP vs LIME method agreement | 0.584 | Moderate agreement between XAI methods |
| Cross-model stability | 0.461 | Lowest stability; explanations differ across models |
| Within-group stability | 0.598 | Moderate stability within selected sample groups |
| Nearest-neighbor stability | 0.629 | Strongest stability among the analyses |

The strongest overall result was nearest-neighbor stability. This means that samples close to each other in feature space generally received more similar explanations. This supports the idea that the explanations are not random.

The weakest result was cross-model stability. This means that different models often explain the same sample differently, even though they solve the same classification task. This is one of the most important findings of the project.

The main conclusion from the overall results is:

> Similar samples tend to have more similar explanations, but explanations vary noticeably across models and XAI methods.

## SHAP vs LIME Agreement by Model

The SHAP vs LIME agreement analysis compared SHAP and LIME explanations for the same model and same selected sample.

The mean top-5 overlaps were:

| Model | Mean Top-5 SHAP/LIME Overlap |
|---|---:|
| Logistic Regression | 0.695 |
| Random Forest | 0.600 |
| MLP | 0.530 |
| Decision Tree | 0.510 |

Logistic Regression had the highest agreement between SHAP and LIME. This suggests that its explanations are more consistent across XAI methods. This result makes sense because Logistic Regression is a simpler linear model, so its decision behavior is easier for both SHAP and LIME to approximate.

Random Forest had the second highest agreement. This is also meaningful because the previous SHAP and LIME notebooks already showed that Random Forest often relied on similar morphology features such as `Perimeter`, `ConvexArea`, `MajorAxisLength`, `MinorAxisLength`, and `ShapeFactor1`.

MLP had lower SHAP/LIME agreement than Logistic Regression and Random Forest. This is important because MLP was the best-performing model in terms of classification accuracy and macro F1-score. However, its explanations were not the most stable across XAI methods.

Decision Tree had the lowest SHAP/LIME agreement. This may be because a single tree can make sharp split-based decisions, while LIME approximates local behavior using perturbed samples. As a result, the explanation patterns may not match as strongly.

The key finding is:

> The most accurate model was not the most explanation-stable model.

This supports the central argument of the project that predictive performance and explanation stability must be evaluated separately.

## Cross-Model Explanation Stability

Cross-model stability measured whether different models produced similar explanations for the same sample using the same XAI method.

The average cross-model top-5 overlap was 0.461, which was the lowest among the four main stability analyses.

This result shows that different models often rely on different explanation features, even when they are trained on the same dataset and predict the same task.

The average cross-model plot showed that LIME had slightly higher cross-model overlap than SHAP. However, both methods showed only moderate-to-low cross-model stability.

Some model pairs were more similar than others. For LIME, Logistic Regression and MLP had relatively high overlap. For SHAP, Decision Tree and Random Forest had relatively higher overlap than some other pairs. This makes sense because tree-based models may share some structure in the way they use split-related features, while Logistic Regression and MLP both used scaled data and often emphasized shape-related features.

However, the overall result is that cross-model agreement is weaker than method agreement, within-group stability, and nearest-neighbor stability.

This means:

> Different models may produce similar predictive performance while relying on different explanation patterns.

This is an important research finding because it shows why model comparison should not stop at accuracy or macro F1-score.

## Within-Group Explanation Stability

Within-group stability measured whether samples from the same selected group received similar explanations.

The overall within-group top-5 overlap was 0.598, which indicates moderate explanation stability within sample groups.

The plot showed that `correct_bombay` had the highest within-group explanation stability. This fits the earlier evaluation result that BOMBAY was the easiest class and was classified very well by all models. However, this result should be interpreted carefully because the BOMBAY selected group was small.

The `correct_dermason` group also showed high stability. This suggests that correctly classified DERMASON samples often share similar explanation features.

The SIRA-related groups were less stable. In particular, `sira_predicted_as_dermason` had the lowest within-group stability in the plot. This is meaningful because SIRA was already identified as one of the hardest classes in the model evaluation notebook.

The important connection is:

> SIRA was difficult to classify and also showed weaker explanation stability.

This creates a strong link between prediction difficulty and explanation instability. The model confusion between DERMASON and SIRA is not only a classification issue; it also appears in the explanation behavior.

## Nearest-Neighbor Explanation Stability

Nearest-neighbor stability measured whether the most similar selected samples in feature space received similar explanations.

This was the strongest stability result in the notebook, with an average top-5 overlap of 0.629.

The nearest-neighbor analysis is the most direct test of the main research question. If two dry bean samples are close in feature space, their explanations should ideally be similar. The result shows that this is generally true, but not perfectly true.

The strongest nearest-neighbor result was:

- Logistic Regression with SHAP: 0.775 mean top-5 overlap

This suggests that Logistic Regression with SHAP produced highly stable explanations for nearby samples.

Random Forest with LIME also showed strong nearest-neighbor stability:

- Random Forest with LIME: 0.730 mean top-5 overlap

Other model-method combinations had moderate stability. Decision Tree with LIME was the weakest among the nearest-neighbor results, with a mean top-5 overlap of 0.505.

The main interpretation is:

> Similar samples generally receive more similar explanations, but the strength of this stability depends on the model and XAI method.

This result supports the project hypothesis that explanation stability can be measured and that it varies across model-method combinations.

## DERMASON and SIRA Focused Stability

DERMASON and SIRA were the most important confusion pair in the earlier evaluation notebook. Because of this, Notebook 07 included a focused stability summary for DERMASON/SIRA-related samples.

The DERMASON/SIRA focused results were:

| Analysis | Mean Top-5 Overlap | Mean Absolute Cosine Similarity |
|---|---:|---:|
| SHAP vs LIME method agreement | 0.561 | 0.590 |
| Within-group stability | 0.583 | 0.610 |
| Nearest-neighbor stability | 0.607 | 0.637 |

These results show that DERMASON/SIRA explanations are moderately stable. They are not random, because the overlap values are clearly above very low agreement. However, they are not highly stable either.

Nearest-neighbor stability was again the strongest result for the DERMASON/SIRA subset. This means that even in the hardest class boundary, morphologically similar samples tend to receive more similar explanations.

However, the DERMASON/SIRA scores are still only moderate. This suggests that the decision boundary between these two classes is explanation-sensitive. Small morphology differences may affect both the prediction and the explanation.

This is one of the most important findings of the project:

> The hardest classification boundary also shows only moderate explanation stability.

## Interpretation of the Plots

The SHAP vs LIME agreement plot showed that Logistic Regression had the strongest agreement between the two explanation methods, followed by Random Forest. MLP and Decision Tree had lower agreement.

This plot supports the idea that simpler or more structured models can produce more consistent explanations across XAI methods.

The cross-model stability plot showed that cross-model explanation agreement is weaker than other forms of stability. LIME showed slightly higher average cross-model overlap than SHAP, but both methods showed that different models often explain the same samples differently.

The within-group stability plot showed that correct and easier groups tend to have stronger explanation stability. `correct_bombay` and `correct_dermason` had higher stability, while SIRA-related confusion groups had lower stability.

The nearest-neighbor stability plot showed that explanation stability is strongest when comparing nearby samples in feature space. Logistic Regression with SHAP and Random Forest with LIME were the strongest model-method combinations.

Together, these plots provide a clear visual summary of the main result:

> Explanation stability is highest when samples are similar, lower when comparing different models, and weaker around difficult class boundaries.

## Relationship Between Accuracy and Explanation Stability

One of the most important findings from this project is that the best-performing model is not necessarily the most explanation-stable model.

In the model evaluation notebook, MLP achieved the best accuracy and macro F1-score. However, in this stability notebook, MLP did not have the highest SHAP vs LIME agreement or the highest nearest-neighbor explanation stability.

Logistic Regression had the strongest SHAP vs LIME method agreement and the strongest nearest-neighbor SHAP stability. Random Forest also showed strong explanation consistency, especially in the LIME nearest-neighbor analysis.

This means that model selection should not be based only on accuracy. If explanation stability is important, then Logistic Regression or Random Forest may be more trustworthy choices than MLP, even if MLP performs slightly better in prediction metrics.

This does not mean that MLP is a bad model. It means that MLP may need more careful explanation validation because its explanations vary more across XAI methods.

## Main Research Findings

The main findings from this notebook are:

1. Explanation stability is measurable using top-k overlap, Jaccard similarity, rank correlation, and cosine similarity.

2. Nearest-neighbor samples have the strongest explanation stability, which supports the idea that similar samples tend to receive similar explanations.

3. Cross-model explanation stability is the weakest, meaning different models often explain the same samples differently.

4. Logistic Regression has the strongest SHAP vs LIME agreement.

5. Random Forest also produces relatively stable explanations, especially in nearest-neighbor analysis.

6. MLP has the best classification performance but not the best explanation stability.

7. Decision Tree has weaker explanation consistency, especially with LIME.

8. DERMASON/SIRA explanations are moderately stable but not highly stable, which matches the difficulty of that class boundary.

9. Feature overlap is generally stronger than rank correlation, meaning explanations often agree on which features are important but not always on the exact ranking.

10. Explanation stability should be evaluated separately from model performance.

## Limitations and Cautions

There are several limitations in this notebook.

First, the stability analysis was based on 40 selected samples, not the full test set. This was intentional because the project focuses on meaningful local explanations, especially around difficult classes. However, the results should not be interpreted as full-test-set stability results.

Second, the selected sample groups were not perfectly balanced. Some groups had more samples than others. This affects within-group stability comparisons, especially for groups with few samples such as `correct_bombay`.

Third, the analysis used top-5 features. Different values of k, such as top-3 or top-10, could produce slightly different stability scores.

Fourth, SHAP and LIME produce different types of explanation values. Therefore, raw SHAP values and raw LIME weights were not directly compared. The analysis focused on feature overlap, ranking, and vector similarity instead.

Fifth, rank correlation was sometimes low or negative. This does not necessarily mean the explanations were completely different. It means that even when explanations shared some features, the ordering of those features often differed.

Sixth, the Dry Bean dataset has highly correlated features. Correlated features can cause explanation importance to shift between related variables. For example, `Area`, `ConvexArea`, `EquivDiameter`, and `Perimeter` are related size features. Similarly, `Compactness`, `ShapeFactor3`, `AspectRation`, and `Eccentricity` are related shape features. This correlation may reduce explanation stability because different methods may assign importance to different but related features.

Seventh, nearest-neighbor similarity was calculated using scaled feature distance. This is appropriate for numerical feature comparison, but different distance metrics could produce different nearest-neighbor pairs.

## Importance for the Main Project Objective

Notebook 07 directly supports the main objective of the project.

The project started with the idea that classification accuracy alone is not enough to judge model trustworthiness. This notebook provides evidence for that idea.

The results show that explanations are not always consistent across models and XAI methods. The MLP model performed best in classification, but Logistic Regression and Random Forest showed stronger explanation consistency in several stability analyses.

This means that model trustworthiness should include both performance and explanation stability. A model that is slightly less accurate but more explanation-stable may be preferable in settings where interpretability and trust are important.

The notebook also shows that explanation stability is not random. Similar samples tend to have more similar explanations, especially in nearest-neighbor analysis. This supports the idea that explanation stability can be studied systematically.

## Next Step

The next notebook should be:

`08_final_results_and_report_figures.ipynb`

The purpose of the next notebook should be to collect the most important results from all previous notebooks and prepare final report-ready tables and plots.

It should include:

- Final model performance comparison
- SHAP global explanation summary
- LIME global explanation summary
- SHAP vs LIME agreement summary
- Cross-model stability summary
- Within-group stability summary
- Nearest-neighbor stability summary
- DERMASON/SIRA focused stability summary
- Final interpretation tables
- Final report figures

The final notebook should not redo all computations. It should load the saved outputs and create clean final results for reporting.

## Conclusion

Notebook 07 successfully measured explanation stability for the Dry Bean classification project.

The results show that explanation stability is strongest for nearest-neighbor samples and weakest across different models. Logistic Regression showed the highest SHAP vs LIME agreement, while Random Forest showed strong stability in several analyses. MLP had the best classification performance but did not have the strongest explanation stability.

The DERMASON/SIRA focused analysis showed moderate explanation stability, confirming that the hardest classification boundary is also explanation-sensitive.

The main conclusion is:

> Similar samples tend to receive more similar explanations, but explanation stability depends strongly on the model, the XAI method, and the sample group.

This supports the main research argument that accuracy alone is not enough. Explanation stability must be analyzed separately when evaluating the trustworthiness of machine learning models.