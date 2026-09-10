# Gas Sensor Drift & Chemical Classification

## Overview

This project investigates gas classification using sensor-array measurements from the UCI Gas Sensor Array Drift at Different Concentrations dataset.

The dataset contains measurements from 16 chemical sensors across multiple batches collected over time. Each measurement is represented by 128 sensor-derived features along with a gas label and concentration value.

A Logistic Regression model is trained on Batch 1 to classify six gas types. The model is first evaluated on a held-out portion of Batch 1 and then tested on later unseen batches without retraining.

The main focus of the project is not only classification accuracy, but also how well the learned feature relationships generalize across different batches.

---

## Dataset

The project uses the **UCI Gas Sensor Array Drift at Different Concentrations** dataset.

The raw measurements are provided in `.dat` files. Each record contains:

- Gas label
- Concentration
- 128 sensor-derived features represented as `feature:value` pairs

The dataset contains measurements from six gas classes collected across multiple batches.

The raw dataset is not included in this repository.

Download the dataset from the UCI Machine Learning Repository and place the required batch files in the `data/` directory before running the notebook.

---

## Project Objective

The project addresses two questions:

1. Can the sensor-derived features be used to classify the six gas types effectively?
2. Does a model trained on one batch generalize to measurements from later unseen batches?

The second question is particularly important because sensor measurements can change across batches and over time. Rather than retraining the model for every batch, this project evaluates how the same trained model performs on unseen data.

---

## Approach

The project follows these main steps:

1. Parse the raw `.dat` files into Pandas DataFrames.
2. Examine gas-class and concentration distributions.
3. Analyze correlations among the 128 sensor-derived features.
4. Use PCA to visualize the high-dimensional feature space.
5. Train a Logistic Regression baseline on Batch 1.
6. Evaluate the model on a stratified held-out portion of Batch 1.
7. Apply the same trained model and scaler to unseen Batch 2.
8. Apply the same trained model and scaler to unseen Batch 3.
9. Compare performance across batches to investigate generalization.

---

## Exploratory Analysis

Exploratory analysis was performed to understand the structure of the sensor measurements before classification.

### Gas Classes

The dataset contains six gas classes:

| Label | Gas |
|---:|---|
| 1 | Ethanol |
| 2 | Ethylene |
| 3 | Ammonia |
| 4 | Acetaldehyde |
| 5 | Acetone |
| 6 | Toluene |

### Feature Correlation

Correlation analysis was performed across the 128 sensor-derived features.

Several features showed strong correlations, indicating that some features contain similar or redundant information.

### Principal Component Analysis

PCA was used to project the standardized Batch 1 measurements into a lower-dimensional space for visualization.

The first two principal components explained approximately **81.5%** of the variance in Batch 1, allowing the major structure of the measurements to be visualized in two dimensions.

PCA was used for exploratory visualization and was not included in the final classification pipeline.

---

## Classification

A Logistic Regression model was used as a baseline classifier.

The Batch 1 data was split into training and test sets using a stratified 80/20 split.

Feature standardization was performed **after the split**:

- The scaler was fitted only on the training data.
- The test data was transformed using the training-fitted scaler.
- The same scaler was later used for the unseen batches.

This prevents information from the test or unseen batches from influencing the training process.

---

## Cross-Batch Evaluation

After training on Batch 1, the model was evaluated on later batches without retraining.

This provides a simple test of whether the feature relationships learned from Batch 1 remain effective when the sensor measurements come from different batches.

### Results

| Evaluation | Accuracy |
|---|---:|
| Batch 1 test split | **95.5%** |
| Unseen Batch 2 | **74.0%** |
| Unseen Batch 3 | **69.4%** |

The classifier performs strongly on the held-out portion of Batch 1, but performance decreases on later unseen batches.

The decline indicates that the feature relationships learned from Batch 1 do not fully generalize across later batches. This demonstrates sensitivity to batch-to-batch distribution changes relevant to sensor drift.

---

## Key Findings

- The sensor-derived features provide strong separation between gas classes within Batch 1.
- Logistic Regression achieved **95.5% accuracy** on the held-out Batch 1 test set.
- Performance decreased to **74.0%** on unseen Batch 2.
- Performance decreased further to **69.4%** on unseen Batch 3.
- The difference between within-batch and cross-batch performance highlights a generalization challenge when sensor measurements come from different batches.
- The results are consistent with batch-to-batch distribution changes relevant to sensor drift, although the accuracy decrease alone does not establish that sensor drift is the only cause.

---

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook

### Machine Learning

- StandardScaler
- Principal Component Analysis (PCA)
- Logistic Regression
- Train/Test Split
- Classification Report
- Confusion Matrix

---

## Project Structure

```text
gas-sensor-drift-classification/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebook/
│   └── GasSense.ipynb
│
├── data/
│   └── README.md
│
└── images/
    └── project visualizations
```
---

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/LavanyaBansod/gas-sensor-drift-classification.git
cd gas-sensor-drift-classification
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Download the dataset

Download the **UCI Gas Sensor Array Drift at Different Concentrations** dataset and extract the required `.dat` files.

Place the dataset files in the `data/` directory:

```text
data/
├── batch1.dat
├── batch2.dat
└── batch3.dat
```

### 4. Open the notebook

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Then open:

```text
notebook/GasSense.ipynb
```

Run the notebook cells in order.

