# From Raw Traffic to Actionable Intelligence

### A Feature-Optimized, Imbalance-Aware Framework for Uncovering Malicious Network Behavior

An explainable and deployment-oriented **Network Intrusion Detection System (NIDS)** built using the **CIC-IDS2018** dataset. The project focuses on transforming high-dimensional network traffic into actionable security intelligence through feature optimization, imbalance-aware evaluation, machine learning, explainable AI, and a real-time detection prototype.

---

##  Project Overview

Modern network traffic contains a large number of features, many of which may be redundant, highly correlated, or contribute little to attack detection.

This project develops an end-to-end machine learning pipeline that:

* Processes large-scale network traffic data
* Identifies and removes redundant features
* Handles severe class imbalance during evaluation
* Prevents data leakage through duplicate-aware dataset splitting
* Compares multiple machine learning models
* Uses SHAP for model explainability
* Reduces the production feature space to **40 optimized features**
* Integrates the trained model into a real-time traffic detection pipeline

The ultimate goal is to move beyond simply predicting an attack class toward generating **interpretable and actionable network security intelligence**.

---

## 🎯 Objectives

1. Build a reliable ML-based network intrusion detection pipeline.
2. Reduce feature redundancy while preserving predictive performance.
3. Address class imbalance through appropriate evaluation strategies.
4. Prevent overly optimistic results caused by duplicate traffic records.
5. Identify the most influential network traffic characteristics using explainable AI.
6. Develop a lightweight model suitable for real-time inference.
7. Convert live network traffic into model-ready flow features.

---

## 🧠 System Architecture

```text
Raw Network Traffic
        │
        ▼
Traffic Flow Extraction
        │
        ▼
Data Cleaning & Validation
        │
        ▼
Duplicate / Leakage Analysis
        │
        ▼
Feature Correlation Analysis
        │
        ▼
Feature Optimization
        │
        ▼
Machine Learning Models
        │
        ├── Logistic Regression
        ├── Decision Tree
        ├── Random Forest
        └── XGBoost
        │
        ▼
Model Evaluation
        │
        ▼
SHAP Explainability
        │
        ▼
40-Feature Production Model
        │
        ▼
Real-Time Traffic Detection
        │
        ▼
Attack Classification
(Benign / FTP-BruteForce / SSH-Bruteforce)
```

---

## 📊 Dataset

This project uses the **CIC-IDS2018** network intrusion dataset.

The dataset contains realistic network traffic representing both benign activity and multiple attack scenarios.

### Selected attack classes

| Class            | Description                                 |
| ---------------- | ------------------------------------------- |
| `Benign`         | Normal network traffic                      |
| `FTP-BruteForce` | Brute-force activity targeting FTP services |
| `SSH-Bruteforce` | Brute-force activity targeting SSH services |

The raw CIC-IDS2018 datasets are **not included in this repository** because of their large size.

---

## 🔬 Data Processing

The preprocessing pipeline includes:

* Missing-value analysis
* Duplicate detection
* Feature-type validation
* Correlation analysis
* Redundant-feature removal
* Train/test separation
* Duplicate-aware validation
* Class-distribution analysis
* Model-space feature optimization

A major focus of the project was preventing **data leakage caused by duplicated network-flow records**.

After correcting the split strategy, the final train/test partitions were verified to contain **zero exact/hash overlap**.

---

## ⚙️ Feature Optimization

The original traffic representation contained a large number of features.

Correlation analysis and explainability were used to identify redundant and low-value features.

The final production pipeline was reduced to:

### **40 optimized features**

This reduction makes the model more suitable for real-time inference while retaining the network characteristics required for intrusion detection.

The production feature list is stored in:

```text
final_nids_features_40.pkl
```

---

## 🤖 Machine Learning Models

Several supervised learning algorithms were evaluated:

* Logistic Regression
* Decision Tree
* Random Forest
* XGBoost

The experiments demonstrated extremely strong classification performance across the evaluated models, with the tree-based approaches achieving near-perfect results on the cleaned evaluation setup.

The final production model is:

```text
final_nids_model_40.pkl
```

### Final model

**Random Forest Classifier**

```text
Input:
40 optimized network-flow features

Output:
Benign
FTP-BruteForce
SSH-Bruteforce
```

---

## 📈 Model Performance

Representative evaluation results from the model comparison:

| Model               | Accuracy | F1-Score |
| ------------------- | -------: | -------: |
| Logistic Regression | 99.4887% | 99.5593% |
| Decision Tree       | 98.9994% | 99.9973% |
| Random Forest       | 97.0988% | 96.9965% |
| XGBoost             | 98.7988% | 99.0965% |

> **Note:** Extremely high performance on intrusion-detection datasets must be interpreted carefully. This project therefore places particular emphasis on duplicate analysis, leakage prevention, class distribution, and robust validation rather than relying on accuracy alone.

---

## ⚖️ Imbalance-Aware Evaluation

Network intrusion datasets are often highly imbalanced, with benign traffic substantially exceeding individual attack classes.

Therefore, the project evaluates more than accuracy.

Key metrics include:

* Precision
* Recall
* F1-score
* ROC-AUC
* Class-wise support
* Confusion matrix

The evaluation pipeline explicitly examines minority-class behavior to ensure that high overall accuracy does not hide poor attack detection.

---

## 🔍 Explainable AI with SHAP

To understand **why** the model makes its predictions, SHAP (SHapley Additive exPlanations) was incorporated into the analysis.

The SHAP analysis identified highly influential network characteristics, including:

* **Fwd Seg Size Min**
* **Dst Port**
* **Flow Duration**

Feature-contribution analysis showed that a small subset of features accounted for the majority of model decision influence.

For example, within the 68-feature analysis:

* Benign: top 2 features contributed **88.38%**
* Benign: top 3 contributed **93.76%**
* Benign: top 10 contributed **99.48%**
* FTP-BruteForce: top 3 contributed **99.33%**
* SSH-Bruteforce: top 3 contributed **89.87%**

These findings supported the transition toward a more compact production feature space.

---

## 🛡️ Real-Time Detection Prototype

The project extends beyond offline classification toward real-time network monitoring.

The prototype follows:

```text
Live / Captured Traffic
        ↓
Packet Capture
        ↓
CICFlowMeter
        ↓
Network Flow CSV
        ↓
40-Feature Transformation
        ↓
Random Forest Model
        ↓
Prediction
        ↓
Security Alert
```

The objective is to transform network traffic into the same feature representation used during model training and classify the resulting flow in near real time.

---

## 📁 Repository Structure

```text
NIDS/
│
├── Network_Intrusion.ipynb
│
├── final_nids_model_40.pkl
├── final_nids_features_40.pkl
│
├── reduced_40_feature_correlation_matrix.csv
├── reduced_40_high_correlation_pairs.csv
├── training_feature_correlation_matrix.csv
├── training_feature_correlation_pairs.csv
│
├── .gitignore
└── README.md
```

### Important

The following files are intentionally excluded from GitHub:

* Raw CIC-IDS2018 datasets
* Temporary models
* Older model versions
* Runtime-generated alert logs

This keeps the repository lightweight and focused on the reproducible research pipeline.

---

## 🧰 Technologies & Tools

### Programming

* Python

### Machine Learning

* Scikit-learn
* XGBoost
* Random Forest
* Logistic Regression
* Decision Trees

### Data Processing

* Pandas
* NumPy

### Explainable AI

* SHAP

### Network Analysis

* CICFlowMeter
* Wireshark
* TShark
* Scapy
* PCAP traffic

### Development

* Jupyter Notebook
* Git
* GitHub

---

## 🔑 Key Contributions

### 1. Leakage-Aware Evaluation

Identified and eliminated duplicate overlap between training and testing data to obtain a more reliable evaluation setup.

### 2. Feature Optimization

Reduced the production feature representation to **40 optimized features** for efficient inference.

### 3. Explainable Detection

Integrated SHAP analysis to identify the network characteristics driving model predictions.

### 4. Imbalance-Aware Analysis

Evaluated minority attack classes using class-wise precision, recall, F1-score, and support instead of relying solely on overall accuracy.

### 5. Real-Time Pipeline

Extended the offline ML workflow toward real-time network-flow extraction and intrusion detection.

---

## 🔮 Future Work

Future development will focus on:

* Real-time streaming inference
* Automated alert generation
* Attack severity scoring
* Online / incremental learning
* Concept-drift detection
* Additional attack categories
* Model monitoring and MLOps
* Deployment through a REST API
* Security dashboard for real-time visualization
* Further testing on unseen network environments

---

## 📌 Current Status

**Research & Prototype — Active Development**

The core machine learning pipeline, feature optimization, explainability analysis, leakage checks, and production model have been completed.

The current development focus is the **real-time network traffic → 40-feature transformation → intrusion prediction pipeline**.

---

## 👩‍💻 Author

**Aleeba Ahmad**

Computer Engineering | Machine Learning | Deep Learning | AI & Cybersecurity

Interested in building intelligent systems at the intersection of **AI, automation, cybersecurity, and real-world applications**.

---

## ⭐ Acknowledgements

This project uses the **CIC-IDS2018** dataset developed by the Communications Security Establishment (CSE) and the Canadian Institute for Cybersecurity (CIC).

If you find this project useful for research or experimentation, consider ⭐ starring the repository.
