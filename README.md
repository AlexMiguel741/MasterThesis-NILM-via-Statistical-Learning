# 📉 Non-Intrusive Load Monitoring (NILM) via Statistical Learning

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Algorithm](https://img.shields.io/badge/ML-SVM_%7C_kNN-orange)
![License](https://img.shields.io/badge/License-MIT-green)

> **MSc Thesis Project** | Politecnico di Milano  
> *Non-Intrusive Monitoring for Condition-Based Diagnostic of Electrical Devices* [file:11]

## 🔬 Abstract
This repository implements a **Non-Intrusive Load Monitoring (NILM)** framework for the fault diagnosis of electromechanical appliances. The project focuses on classifying internal mechanical states (e.g., specific spray-arm rotation phases in dishwashers) using high-frequency current signatures ($f_s = 5 \text{ kHz}$).

By mapping time-series electrical transients into a statistical feature space, **Support Vector Machines (SVM)** and **k-Nearest Neighbors (k-NN)** classifiers are employed to identify operational modes with high precision.

## ⚡ Signal Processing Pipeline

### 1. RMS Envelope Extraction
The raw current signal is non-stationary and dominated by the 50Hz mains frequency. We extract the power envelope using a sliding Root Mean Square (RMS) window:

$$
x_{RMS}[k] = \sqrt{\frac{1}{N} \sum_{j=k-N+1}^{k} i^2[j]}
$$

*   $N=100$ samples (20 ms window) effectively filters the AC component while preserving steady-state load dynamics.

### 2. Feature Engineering
The learning problem is defined in a feature space constructed from higher-order statistical moments computed over the RMS envelope.

**Skewness (3rd Moment)**  
Measures the asymmetry of the signal distribution, correlating with pump load irregularities.

$$
\text{Skewness} = \frac{\frac{1}{M}\sum_{i=1}^{M}(x_i - \mu)^3}{\sigma^3}
$$

**Kurtosis (4th Moment)**  
Quantifies the "tailedness" or impulsive nature of the signal, effective for detecting mechanical transients.

$$
\text{Kurtosis} = \frac{\frac{1}{M}\sum_{i=1}^{M}(x_i - \mu)^4}{\left(\frac{1}{M}\sum_{i=1}^{M}(x_i - \mu)^2\right)^2}
$$

## 🧠 Machine Learning Models

### Support Vector Machine (SVM)
The core model is a soft-margin SVM designed to find the optimal separating hyperplane.

**Primal Optimization Problem:**

$$
\min_{\mathbf{w}, b, \xi} \frac{1}{2}||\mathbf{w}||^2 + C \sum_{i=1}^{N} \xi_i
$$

**Kernel Trick (RBF)**  
To handle non-linear separability in the feature space, we employ the Radial Basis Function (RBF) kernel:

$$
K(\mathbf{x}, \mathbf{x}') = \exp\left(-\frac{||\mathbf{x} - \mathbf{x}'||^2}{2\sigma^2}\right)
$$

### Multiclass Strategy (ECOC)
For appliances with multiple states ($K > 2$), we utilize an **Error-Correcting Output Codes (ECOC)** framework, decomposing the problem into $L$ binary classifiers for robustness.

## 📊 Results & Performance
Models were validated using **5-Fold Cross-Validation**. The system demonstrated the ability to disaggregate simultaneous loads ($I_{agg} = I_1 + I_2$) and identify distinct mechanical phases.

| Appliance | Model | Kernel/Metric | Accuracy |
| :--- | :--- | :--- | :--- |
| **Dishwasher 1** | k-NN | Euclidean ($k=10$) | **89.0%** |
| **Dishwasher 1** | SVM | RBF | 72.0% |
| **Dishwasher 2** | SVM | Linear | **99.5%** |

## 📂 Repository Structure
```bash
├── data/           # Raw current acquisitions (5kHz) and preprocessed CSVs
├── notebooks/      # Jupyter notebooks for EDA, Feature Extraction, and Training
├── src/
│   ├── dsp.py      # Signal processing (RMS, Filtering)
│   ├── features.py # Statistical moments calculation
│   └── models.py   # SVM and ECOC implementations
├── thesis/         # Full PDF of the Master's Thesis
└── README.md
