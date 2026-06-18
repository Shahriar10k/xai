# Final Results Summary

## Main Project Objective

The main objective of this project was to evaluate explanation stability in Dry Bean classification. The project did not only compare model accuracy; it also investigated whether similar samples receive similar and stable explanations across models and XAI methods.

## Best Predictive Model

The best predictive model was **MLP**, with a macro F1-score of **0.9351**.

## Best SHAP vs LIME Agreement

The strongest SHAP vs LIME agreement was achieved by **Logistic Regression**, with a mean top-5 feature overlap of **0.6950**.

## Best Nearest-Neighbor Stability

The strongest nearest-neighbor explanation stability was achieved by **Logistic Regression - SHAP**, with a mean top-5 feature overlap of **0.7750**.

## Overall Stability Finding

The strongest overall stability analysis was **Nearest-neighbor stability**, with a mean top-5 overlap of **0.6294**.

The weakest overall stability analysis was **Cross-model stability**, with a mean top-5 overlap of **0.4608**.

## Main Conclusion

The results show that explanation stability exists, but it is uneven. Similar samples tend to receive more similar explanations, especially in nearest-neighbor analysis. However, explanations differ more strongly across models.

The main conclusion is that high classification performance does not automatically guarantee stable explanations. Explanation stability should therefore be evaluated separately from model accuracy.
