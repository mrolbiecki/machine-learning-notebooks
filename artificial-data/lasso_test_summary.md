# Lasso Regression Test Results Summary

## Initial Performance Issues

When testing regression models on our artificial dataset, we observed that Lasso regression performed significantly worse than other methods:

```
--- Dataset: Original ---
Model: LinearRegression
Mean CV R²: 0.3750 ± 0.0624
Mean CV RMSE: 2.8024 ± 0.0987

Model: Ridge
Mean CV R²: 0.3753 ± 0.0623
Mean CV RMSE: 2.8016 ± 0.0987

Model: Lasso
Mean CV R²: 0.0236 ± 0.0126
Mean CV RMSE: 3.5113 ± 0.1306
```

This pattern was consistent across all datasets (Original, Top Correlated, SelectKBest, RFE, and PCA), with Lasso consistently showing the worst performance among all regression methods.

## Investigation

To understand why Lasso performed so poorly, we conducted additional tests focusing on the `alpha` parameter, which controls the strength of regularization in Lasso regression.

Our investigation revealed:

```
Default Lasso (alpha=1.0):
Alpha: 1.00000000
Mean CV R²: 0.0236 ± 0.0126
Mean CV RMSE: 3.5113 ± 0.1306
Non-zero coefficients: 2 out of 400

Lasso with alpha=0.1:
Alpha: 0.10000000
Mean CV R²: 0.4861 ± 0.0327
Mean CV RMSE: 2.5445 ± 0.0843
Non-zero coefficients: 48 out of 400

Lasso with alpha=0.01:
Alpha: 0.01000000
Mean CV R²: 0.4172 ± 0.0574
Mean CV RMSE: 2.7063 ± 0.0973
Non-zero coefficients: 335 out of 400

Lasso with alpha=0.001:
Alpha: 0.00100000
Mean CV R²: 0.3803 ± 0.0619
Mean CV RMSE: 2.7903 ± 0.0988
Non-zero coefficients: 376 out of 400

Lasso with alpha=0.0001:
Alpha: 0.00010000
Mean CV R²: 0.3755 ± 0.0623
Mean CV RMSE: 2.8012 ± 0.0987
Non-zero coefficients: 386 out of 400
```

## Key Findings

1. **Default Alpha Too High**: With scikit-learn's default alpha value of 1.0, Lasso eliminated 398 out of 400 features, resulting in extreme underfitting.

2. **Optimal Alpha**: With alpha=0.1, Lasso achieved R²=0.4861 and RMSE=2.5445, making it one of the best-performing models when properly tuned.

3. **Feature Selection**: The best-performing alpha (0.1) retained 48 out of 400 features, suggesting that most features in the dataset are irrelevant for predicting the target variable.

4. **Performance vs. Sparsity Tradeoff**: As alpha decreased, the model included more features but didn't necessarily perform better, indicating an optimal balance at alpha=0.1.

## Conclusion

Lasso's poor performance in the original analysis was due to using the default alpha value (1.0), which was too aggressive for this dataset. When properly tuned, Lasso not only performs well but also provides useful feature selection, identifying a small subset of features that are most relevant for prediction.

This highlights the critical importance of hyperparameter tuning when using regularized regression methods, especially Lasso, where the choice of alpha can dramatically impact both feature selection and model performance.
