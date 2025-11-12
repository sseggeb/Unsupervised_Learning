# Model Discussion and Conclusions for Wine Dataset Analysis

This section details the results of the model building and training phase, focusing on a direct comparison between two distance-based algorithms: K-Means Clustering (unsupervised), K-Nearest Neighbors (KNN, supervised), and Principal Component Analysis (PCA).

## Unsupervised Learning and Preprocessing

Objective: To determine if the inherent feature space, without using the true class labels, naturally groups the data points into the three distinct wine cultivars.

Procedure:
1. Scaling: The data was first Standardized using StandardScaler, as dictated by the EDA, to ensure all features contribute equally to the distance calculation.
2. Clustering: K-Means was run with $k=3$ (the known number of classes).
3. Evaluation: Clustering quality was assessed using permutation-invariant metrics: Adjusted Rand Index (ARI) and Adjusted Mutual Information (AMI).

Results:
The K-Means model achieved extremely high performance in separating the data into three clusters, which closely align with the true classes.

Adjusted Rand Index (ARI): $\approx 0.89$

Adjusted Mutual Information (AMI): $\approx 0.89$

An ARI close to $1.0$ confirms the initial EDA hypothesis: the features contain a strong, separable structure that defines the three cultivars. This high score is only possible because the crucial step of Standardization was performed, preventing the feature 'Proline' (with its high magnitude) from dominating the distance calculations.

4. Principal Component Analysis
PCA was introduced to address multicollinearlity identified in the EDA and simplify the model by reducing the feature count.

Variance Retention: The PCA analysis found that 8 components are required to explain over 90% of the total variance in the dataset.

Visual Confirmation: A scatter plot of the first two Principal Components clearly showed that the three wine classes are already highly distinct and clustered in this 2D space.

Conclusion: PCA is effective, but only reduces dimensionality from 13 to 8 components.

## Supervised Learning Model: K-Nearest Neighbors (KNN)

The KNN classifier was used to evaluate both he full feature set and the PCA-transformed feature set.

| Model Varient | Number of Features | Best Accuracy on Test Set     | Dimensionality Reduction Effectiveness                            |
|---------------|--------------------|-------------------------------|-------------------------------------------------------------------|
| KNN           | 13                 | $\approx 0.98$ (Near-Perfect) | Baseline performance                                              |
| KNN (PCA)     | 8                  | $\approx 0.96$ (Excellent)    | Retained $\approx 0.98$ of the original accuracy with 8 features. |


Procedure:
1. Scaling: The training data was scaled (StandardScaler fitted on $X_{train}$), and this transformation was applied to both $X_{train}$ and $X_{test}$ to prevent data leakage.
2. Hyperparameter Optimization (HPO): GridSearchCV with 5-fold cross-validation was used to tune the KNeighborsClassifier. The optimized parameters included $n\_neighbors$ (k value), $weights$ (uniform vs. distance), and the $metric$ (euclidean vs. manhattan).
3. Evaluation: Final accuracy and metrics were assessed on the unseen, scaled test set.

Results:
The optimized KNN model demonstrated near-perfect separation of the wine classes, highlighting its effectiveness when applied to properly scaled data.

Best Hyperparameters: Typically, an optimal configuration is found with small k values (e.g., $n\_neighbors: 3$ or $5$) and using $weights: distance$, often paired with the $manhattan$ metric.

Test Set Accuracy: $\approx 0.98$ (98% accuracy)

Conclusion: The high accuracy validates the model choice. Since KNN is distance-based, its success is a direct result of the mandatory Standardization step identified in the EDA.

## Discussion and Final Conclusions
### Model Comparison: K-Means vs. KNN

| Feature  | K-Means (Unsupervised)  | K-Nearest Neighbors (Supervised)  | Justification                                                                                                                                                                         |
|---|---|---|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Learning Type | Clustering (Finds Structure)  |  Classification (Predicts Label) | Both rely on calculating distance between points.                                                                                                                                     |
| Scaling Need  |  ABSOLUTELY NECESSARY | ABSOLUTELY NECESSARY  | The success of both models proves the EDA finding that feature scaling is non-negotiable for this dataset due to the high range of features like Proline.                             |
| Performance  | Excellent (ARI $\approx 0.89$)  | Near-Perfect (Accuracy $\approx 0.98$)  | KNN achieves higher performance because it uses the true class labels during training, optimizing the prediction boundary directly. K-Means must discover the clusters independently. |
| Feature Importance  | Implicit (Features like Flavanoids drive the cluster centers)  | Not Provided  | KNN doesn't rank features; performance depends on the combined distance of all features.                                                                                                                                                                                      |

## Final Conclusion
The analysis confirms that the wine dataset is highly separable and structurally robust. The high scores for both K-Means and K-Nearest Neighbors demonstrate that Standardization was the correct preprocessing choice. The structure of the data is so clean that even the unsupervised K-Means model can accurately infer the three cultivar groupings, providing a strong foundation for the supervised KNN classifier to achieve near-perfect predictive performance.
