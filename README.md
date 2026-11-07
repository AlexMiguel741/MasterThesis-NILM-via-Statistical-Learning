# Fault Diagnosis of Electrical Appliances from their Current Signal

**MSc thesis · Politecnico di Milano · Automation & Control Engineering**
*Non-Intrusive Monitoring for condition-based diagnostic of electrical devices via machine learning*

Can you tell what is happening **inside** a machine just by looking at the current it draws,
without adding any sensor to it? This thesis applies **Non-Intrusive Load Monitoring (NILM)**
and machine learning to two dishwashers, recognising their internal washing phases (spray-arm
movement) from the electrical signature alone.

## Results

| Appliance | Task | Model | Accuracy (5-fold CV) |
| :-- | :-- | :-- | :-- |
| Dishwasher 1 | 3 washing phases | **k-NN (k = 10)** | **89%** |
| Dishwasher 1 | 3 washing phases | ECOC SVM | 72% |
| Dishwasher 2 | Active / Off | **SVM** | **99.5%** |

Final step: with current thresholds identified on both machines, a **single model predicts the
phases of both dishwashers running at the same time** from one aggregated current measurement.

## What I did

1. **Data acquisition** of voltage and current on two real dishwashers (rinse cycle)
2. **Signal processing**: RMS current computed over a 1 s moving window
3. **Feature engineering**: time-domain statistical features, selected by analysis:
   mean, skewness, kurtosis (dataset for Dishwasher 1: 11,280 samples)
4. **Classification**: ECOC multiclass SVM and k-NN, evaluated with confusion matrices and
   5-fold cross-validation
5. **Prediction on unseen acquisitions**, single-machine and aggregated

## Why it matters

The same approach applies to **predictive maintenance** of any electrical machine: low-cost
monitoring from one current sensor, no intrusive instrumentation.

## Stack

MATLAB · Statistical learning (SVM, k-NN, ECOC) · Signal processing

## Files

- [`MSCThesis_ALex.pdf`](MSCThesis_ALex.pdf): full thesis (method, features, results, confusion matrices)
