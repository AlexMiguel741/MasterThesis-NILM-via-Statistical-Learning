# NILM-Based Condition Monitoring of Electrical Appliances

![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?logo=mathworks&logoColor=white)
![ML](https://img.shields.io/badge/ML-SVM%20%7C%20k--NN%20%7C%20ECOC-orange)
![Type](https://img.shields.io/badge/MSc%20Thesis-Politecnico%20di%20Milano-1f3a60)

MSc thesis in Automation & Control Engineering, Politecnico di Milano.
*Non-Intrusive Monitoring for condition-based diagnostic of electrical devices via machine learning.*

## Overview

Non-Intrusive Load Monitoring (NILM) pipeline that infers the **internal operating state** of an
appliance from its electrical signature only, with no additional sensors on the machine.
Validated on two dishwashers: classification of washing phases (spray-arm movement) from
current and voltage measurements, including the **disaggregation of two machines running
simultaneously** from a single aggregated current measurement.

## Key Results

| Appliance | Classes | Model | Validation | Accuracy |
| :-- | :-: | :-- | :-- | :-: |
| Dishwasher 1 | 3 | k-NN (k = 10) | 5-fold CV | **89.0%** |
| Dishwasher 1 | 3 | ECOC multiclass SVM | 5-fold CV | 72.0% |
| Dishwasher 2 | 2 | Binary SVM | 5-fold CV | **99.5%** |

## Pipeline

```
Current / voltage acquisition ─► RMS current (1 s moving window) ─► Feature extraction
        ─► Feature selection ─► Classifier (SVM / k-NN) ─► 5-fold CV ─► Phase prediction
```

1. **Data acquisition**: voltage and current sampled per machine and in aggregate operation
   (rinse cycle)
2. **Preprocessing**: RMS current over a 1 s moving window
3. **Feature engineering**: time-domain statistical features; selected set: mean, skewness,
   kurtosis (Dishwasher 1 dataset: 11,280 × 7)
4. **Models**: ECOC SVM for the multiclass problem, k-NN (k = 10), binary SVM
5. **Evaluation**: confusion matrices, K-fold cross-validation (K = 5)
6. **Inference** on unseen acquisitions; threshold-based model for the aggregated two-machine case

## Applications

Low-cost condition monitoring and **predictive maintenance** of electrical machines from a
single current sensor.

## Tech Stack

MATLAB · Statistics and Machine Learning Toolbox · Signal processing

## Repository Structure

```
├── MSCThesis_ALex.pdf   # Full thesis: state of the art, method, features, results
└── README.md
```
