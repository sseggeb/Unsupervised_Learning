# DTSA 5510 Unsupervised Algorithms in Machine Learning Final Project: Wine Cultivar Clustering

---

## 📋 Project Overview

This project explores the application of **unsupervised learning** (K-Means Clustering) to the Wine Recognition Dataset to determine if chemical analysis can successfully group wines according to their true cultivar origin. A **supervised learning** model (K-Nearest Neighbors, KNN) is also implemented as a comparative baseline to validate the structure and separability of the data.

### Unsupervised Learning Goal
The primary objective is to group the wines into clusters that accurately align with the three different known wine cultivators ("true" but unseen labels in an unsupervised context).

---

## 📊 Dataset: Wine Recognition Dataset

The analysis uses the **Wine Recognition Dataset** (UCI ID 109).

The dataset contains the results of a chemical analysis of wines grown in the same region in Italy. It includes 13 continuous features describing the chemical properties of the wine, such as:
* Alcohol
* Malic Acid
* Ash
* Alcalinity of Ash
* Magnesium
* Total Phenols
* Flavanoids
* etc

---

## 🧠 Methodology and Models

### 1. Data Preprocessing
* **Inspection:** The data was clean with no missing values (NaNs).
* **Scaling:** Exploratory Data Analysis (EDA) confirmed that the features had vastly different scales (e.g., Proline values are much higher than Malic Acid). To prevent high-magnitude features from dominating the distance calculations in K-Means and KNN, **StandardScaler** was applied.

### 2. Unsupervised Learning: K-Means Clustering
* **Model:** K-Means clustering was executed with $k=3$ (the known number of cultivars) on the **scaled** features.
* **Evaluation:** Clustering quality was assessed using permutation-invariant metrics:
    * **Adjusted Rand Index (ARI):** **0.8975** (A score close to 1 indicates near-perfect clustering alignment with the true classes).
    * **Adjusted Mutual Information (AMI):** **0.8746** (A score close to 1 indicates high mutual agreement between clustering and true classes).

### 3. Supervised Comparison: K-Nearest Neighbors (KNN)
* KNN was used as a classification model on the scaled training data to establish a performance baseline for a supervised model.
* **Result:** The KNN classifier achieved **near-perfect predictive performance** (Accuracy $\approx 0.98$).

### 🌟 Conclusion
The analysis successfully demonstrated that the chemical composition of the wine is highly predictive of its cultivar. The unsupervised K-Means model was remarkably effective, accurately inferring the three cultivar groupings. The high ARI/AMI scores for K-Means and the near-perfect accuracy for KNN confirm that the **Standardization** preprocessing step was crucial and that the structure of the Wine Recognition Dataset is inherently clean and highly separable.

---

## 🛠️ Installation and Requirements

To run this notebook locally, you need a Python environment with the following packages installed:

```bash
pip install pandas numpy scikit-learn seaborn matplotlib ucimlrepo
