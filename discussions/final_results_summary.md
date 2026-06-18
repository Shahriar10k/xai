# Discussion and Analysis: Notebook 08 — Final Results and Report Figures

## Purpose of This Notebook

This notebook was created as the final result-collection stage of the Dry Bean XAI project. The previous notebooks completed the full pipeline: data understanding, preprocessing, model training, model evaluation, SHAP explanations, LIME explanations, and explanation stability analysis.

The purpose of this notebook was not to retrain models or recompute explanations. Instead, it loaded the saved outputs from earlier notebooks and organized them into final report-ready tables, plots, and summary files.

This notebook connects all major findings of the project into one final interpretation. It combines:

- Predictive model performance
- SHAP explanation results
- LIME explanation results
- SHAP vs LIME agreement
- Cross-model explanation stability
- Within-group explanation stability
- Nearest-neighbor explanation stability
- DERMASON/SIRA focused stability
- Final key findings

The main project objective was to study whether accurate models also produce stable explanations. Therefore, this final notebook summarizes both model performance and explanation stability.

## Final Predictive Performance

The final model performance comparison confirmed that the MLP Neural Network achieved the best predictive performance.

The best model was:

| Model | Macro F1-score |
|---|---:|
| MLP | 0.9351 |

The MLP achieved the highest macro F1-score among the four trained models. Logistic Regression and Random Forest were very close behind, while Decision Tree had the weakest performance.

This confirms the result from the model evaluation notebook: the MLP is the strongest classifier in terms of predictive performance.

However, the performance difference between MLP, Logistic Regression, and Random Forest was relatively small. This is important because the project is not only about which model predicts best. The project also asks whether the explanations are stable and trustworthy.

A key conclusion from the performance results is:

> MLP gives the best classification performance, but classification performance alone is not enough to judge the model’s trustworthiness.

## Final XAI Feature Frequency Findings

The final XAI feature frequency plot shows which features appeared most often in the top-5 explanation lists across both SHAP and LIME and across all models.

The most frequent top-5 XAI features were:

1. `ShapeFactor1`
2. `MajorAxisLength`
3. `Perimeter`
4. `MinorAxisLength`
5. `ConvexArea`
6. `EquivDiameter`
7. `roundness`

This result is meaningful because all of these features describe bean morphology. They represent shape, size, axis length, perimeter, and geometric structure.

`ShapeFactor1` and `MajorAxisLength` appeared most frequently, showing that they are central explanatory features across models and XAI methods. `Perimeter` and `MinorAxisLength` also appeared repeatedly, confirming that size and boundary-related measurements are important for dry bean classification.

The frequency plot supports the idea that the explanations are domain-relevant. The models are not relying on random or meaningless variables. Instead, their explanations are based on physical properties of the bean samples.

However, the models and explanation methods did not always agree on the exact ranking of these features. This is why explanation stability analysis was necessary.

## Final SHAP vs LIME Agreement

The final SHAP vs LIME agreement plot showed that Logistic Regression had the strongest agreement between the two XAI methods.

The final ranking was:

| Model | Mean Top-5 SHAP/LIME Overlap |
|---|---:|
| Logistic Regression | 0.695 |
| Random Forest | 0.600 |
| MLP | 0.530 |
| Decision Tree | 0.510 |

Logistic Regression produced the most consistent explanations across SHAP and LIME. This is expected because Logistic Regression is a simpler linear model. Its decision structure is easier for explanation methods to approximate.

Random Forest also showed reasonably strong agreement between SHAP and LIME. This fits the earlier SHAP and LIME findings where Random Forest consistently emphasized morphology features such as `Perimeter`, `ConvexArea`, `MajorAxisLength`, `MinorAxisLength`, and `EquivDiameter`.

MLP had lower SHAP vs LIME agreement than Logistic Regression and Random Forest. This is one of the most important findings of the project because MLP was the best-performing model in terms of macro F1-score, but it was not the most explanation-stable model.

Decision Tree had the lowest SHAP vs LIME agreement. This may be because a single decision tree can create sharp split-based decisions, while LIME approximates predictions locally through perturbations. This can lead to differences between SHAP and LIME explanations.

The main conclusion from this comparison is:

> The best predictive model was not the most stable model across SHAP and LIME explanations.

## Final Overall Explanation Stability

The final explanation stability summary showed four main stability results:

| Stability Analysis | Mean Top-5 Feature Overlap |
|---|---:|
| Nearest-neighbor stability | 0.6294 |
| Within-group stability | approximately 0.598 |
| SHAP vs LIME method agreement | approximately 0.584 |
| Cross-model stability | 0.4608 |

The strongest stability result was nearest-neighbor stability. This means that samples close to each other in feature space generally received more similar explanations.

This is an important result because the main research question asks whether similar samples receive similar explanations. The nearest-neighbor result suggests that the answer is generally yes, although the stability is not perfect.

The weakest stability result was cross-model stability. This means that different models often explained the same samples differently.

This supports the central claim of the project:

> Similar samples tend to receive more similar explanations, but different models may still rely on different explanation patterns.

The overall results show that explanation stability exists, but it is uneven. It depends on the model, the XAI method, and the type of comparison.

## Final Nearest-Neighbor Stability Findings

Nearest-neighbor stability was the strongest overall stability analysis. This analysis compared explanations for samples that were closest to each other in scaled feature space.

The strongest model-method combination was:

| Model and Method | Mean Top-5 Overlap |
|---|---:|
| Logistic Regression - SHAP | 0.775 |

This was the highest stability result in the project. It suggests that Logistic Regression with SHAP produced highly consistent explanations for nearby samples.

Random Forest with LIME was also strong:

| Model and Method | Mean Top-5 Overlap |
|---|---:|
| Random Forest - LIME | approximately 0.730 |

This suggests that Random Forest explanations are also stable when explaining similar samples, especially with LIME.

The weakest nearest-neighbor stability result was Decision Tree with LIME. This fits the broader finding that Decision Tree explanations were less stable than Logistic Regression and Random Forest explanations.

The nearest-neighbor results are very important because they directly answer the project’s central idea:

> Similar dry bean samples generally receive more similar explanations, but the level of stability depends on the model and explanation method.

## Final DERMASON/SIRA Stability Findings

DERMASON and SIRA were the most important difficult class pair in this project. Earlier evaluation results showed that these two classes were frequently confused. Therefore, Notebook 08 included a final DERMASON/SIRA focused explanation stability summary.

The final DERMASON/SIRA stability results showed:

| Analysis | Mean Top-5 Feature Overlap |
|---|---:|
| Nearest-neighbor stability | approximately 0.607 |
| Within-group stability | approximately 0.583 |
| Method agreement | approximately 0.561 |

The strongest DERMASON/SIRA result was nearest-neighbor stability. This means that even around the difficult DERMASON/SIRA boundary, samples that are close in feature space tend to receive more similar explanations.

However, the stability values are only moderate. This means that the DERMASON/SIRA boundary is not only difficult for classification but also somewhat unstable in explanation behavior.

This is an important finding because it connects model confusion with explanation instability. The hardest class boundary also has only moderate explanation consistency.

The main interpretation is:

> DERMASON and SIRA explanations are not random, but they are not highly stable either. This confirms that the DERMASON/SIRA boundary is explanation-sensitive.

## Relationship Between Accuracy and Explanation Stability

One of the strongest conclusions of the whole project is that model accuracy and explanation stability are not the same thing.

The MLP model achieved the best predictive performance with the highest macro F1-score. However, it did not achieve the best SHAP vs LIME agreement and did not produce the strongest nearest-neighbor explanation stability.

Logistic Regression had the best SHAP vs LIME agreement and the strongest nearest-neighbor stability with SHAP. Random Forest also showed strong stability, especially in nearest-neighbor analysis and XAI feature agreement.

This means that a model can be slightly less accurate but more explanation-stable.

This is important for model selection. If the goal is only prediction, MLP may be the best choice. If the goal includes interpretability and explanation trustworthiness, Logistic Regression and Random Forest become very strong candidates.

The final interpretation is:

> The MLP is the best classifier, but Logistic Regression and Random Forest provide more stable explanation behavior.

## Final Key Findings

The final notebook produced the following key findings:

1. The best predictive model was MLP with a macro F1-score of 0.9351.

2. The best SHAP vs LIME agreement was achieved by Logistic Regression with a top-5 overlap of 0.695.

3. The best nearest-neighbor stability was achieved by Logistic Regression with SHAP, with a top-5 overlap of 0.775.

4. The strongest overall stability analysis was nearest-neighbor stability, with a top-5 overlap of 0.6294.

5. The weakest overall stability analysis was cross-model stability, with a top-5 overlap of 0.4608.

6. The most frequent explanatory features were `ShapeFactor1`, `MajorAxisLength`, `Perimeter`, `MinorAxisLength`, and `ConvexArea`.

7. DERMASON/SIRA explanations were moderately stable, with nearest-neighbor stability stronger than method agreement.

8. Similar samples generally received more similar explanations.

9. Different models often produced different explanation patterns.

10. Predictive performance and explanation stability should be evaluated separately.

## Interpretation of Final Plots

The final model performance plot clearly showed that MLP had the highest macro F1-score, while Logistic Regression and Random Forest were very close. Decision Tree was the weakest model.

The top-5 XAI feature frequency plot showed that `ShapeFactor1`, `MajorAxisLength`, `Perimeter`, and `MinorAxisLength` were the most repeated explanatory features across models and methods. This confirms that the explanation methods focused on meaningful morphological features.

The final SHAP vs LIME agreement plot showed that Logistic Regression had the highest agreement between the two XAI methods, followed by Random Forest. MLP and Decision Tree showed weaker agreement.

The final explanation stability summary plot showed that nearest-neighbor stability was the strongest type of stability and cross-model stability was the weakest.

The final nearest-neighbor stability plot showed that Logistic Regression with SHAP was the most stable model-method combination for similar samples. Random Forest with LIME also performed strongly.

The final DERMASON/SIRA stability plot showed that nearest-neighbor stability was the strongest DERMASON/SIRA result, while method agreement was the weakest.

Together, these plots provide a clear visual summary of the project:

> Explanation stability is strongest when comparing similar samples, but weaker when comparing different models.

## Limitations and Cautions

There are several limitations that should be considered when interpreting the final results.

First, explanation stability was measured on 40 selected samples rather than the entire test set. This was intentional because the project focused on meaningful local cases, especially confusion pairs. However, the results should not be interpreted as full-test-set explanation stability.

Second, the selected groups were not perfectly balanced. Some groups had more selected samples than others, and some duplicate sample IDs were removed earlier in the pipeline.

Third, the stability analysis used top-5 explanation features. Using a different value such as top-3 or top-10 may slightly change the overlap scores.

Fourth, SHAP and LIME use different explanation mechanisms. SHAP values and LIME weights should not be directly compared as raw magnitudes. This project correctly focused on feature overlap, ranking, and similarity-based comparisons.

Fifth, the Dry Bean dataset contains highly correlated features. Related features such as `Area`, `ConvexArea`, `EquivDiameter`, and `Perimeter` may share similar information. Explanation methods may assign importance to one feature in one case and a correlated feature in another case. This can reduce apparent stability even when the model reasoning is broadly similar.

Sixth, nearest-neighbor similarity was based on scaled Euclidean distance. Other distance metrics could produce different nearest-neighbor pairs.

Seventh, LIME explanations depend on perturbation sampling and discretization. The notebook used 5000 perturbation samples, but different LIME settings could slightly affect local explanation weights.

These limitations do not invalidate the results, but they should be discussed in the final report.

## Main Research Conclusion

The main conclusion of the project is that explanation stability is an important and separate dimension of model evaluation.

The MLP model achieved the strongest predictive performance, but Logistic Regression and Random Forest produced more stable explanations in several analyses. This shows that accuracy alone is not enough to judge the trustworthiness of a machine learning model.

The results also show that similar samples generally receive more similar explanations. This was especially clear in the nearest-neighbor stability analysis, which produced the highest overall stability score.

However, explanation stability was weaker across different models. This means that different models can solve the same classification task while relying on different explanatory patterns.

Therefore, the project supports the following final claim:

> In Dry Bean classification, predictive accuracy and explanation stability do not always align. Similar samples tend to receive more similar explanations, but the stability of those explanations depends on the model and the XAI method.

## Final Project Conclusion

This project successfully built a complete machine learning and XAI pipeline for the Dry Bean dataset.

The project included:

- Data understanding
- Preprocessing
- Comparative model training
- Model evaluation
- SHAP explanation generation
- LIME explanation generation
- Explanation stability analysis
- Final report-ready result generation

The final findings show that the Dry Bean models rely on meaningful morphology-based features such as `ShapeFactor1`, `MajorAxisLength`, `Perimeter`, `MinorAxisLength`, and `ConvexArea`.

The MLP model was the best classifier, but it was not the most explanation-stable model. Logistic Regression showed the strongest SHAP vs LIME agreement, and Logistic Regression with SHAP showed the strongest nearest-neighbor explanation stability.

The strongest stability pattern was found among nearest-neighbor samples, showing that similar samples tend to receive more similar explanations. The weakest stability pattern was found across different models, showing that model choice strongly affects explanation behavior.

The final conclusion is:

> Explanation stability should be evaluated alongside accuracy because a model can be accurate without being the most explanation-stable.