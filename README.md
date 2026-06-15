# WFDS: Zero-Training Network Intrusion Detection System

WFDS is an unsupervised real-time network intrusion detection system designed to detect cyber attacks without any training data. The system utilizes sliding window analysis to monitor network traffic, calculates drift scores using Mahalanobis distance to identify anomalies, and presents attack detection through interactive visualizations. WFDS aims to improve network security by enabling zero-training deployment, enhancing edge device protection, and reducing dependency on labeled attack data.

## Key Features

- Zero Training Requirement - Detects attacks immediately without labeled data
- Real-time Detection - Processes network traffic in sliding windows
- Interpretable Scores - Provides drift scores for explainable decisions
- ML Comparison - Benchmarked against Random Forest
- Multi-Dataset Validation - Tested on CICIDS-2017 and UNSW-NB15 datasets
- Comprehensive Visualization - 6 professional plots for analysis

## Performance Summary

### CICIDS-2017 Dataset:
| Method | Accuracy | F1-Score | AUC | Training Time |
|--------|----------|----------|-----|---------------|
| WFDS | 84.9% | 75.5% | 83.5% | 0 seconds |
| Random Forest | 97.8% | 97.1% | 99.2% | 13.6 seconds |

### UNSW-NB15 Dataset:
| Method | Accuracy | F1-Score | AUC | Training Time |
|--------|----------|----------|-----|---------------|
| WFDS | 56.0% | 52.3% | 69.1% | 0 seconds |
| Random Forest | 92.2% | 94.0% | 98.0% | 15.2 seconds |

**Key Insight:** WFDS trades 13-36% accuracy for zero training time and immediate deployment, making it perfect for resource-constrained environments where ML training is infeasible.

## System Architecture

The system processes network traffic through sliding windows of 400 samples. Features are extracted and scaled, then passed to both WFDS (unsupervised detection) and ML models (baseline comparison). WFDS establishes a baseline from the first window assuming normal traffic, then calculates drift scores using Mahalanobis distance between window means and the baseline. Scores are normalized to [0,1] range, and a threshold at the 75th percentile classifies a window as an attack.

## WFDS Algorithm

The algorithm works in five steps:

1. Establish baseline from first window (normal traffic assumption)
2. Calculate covariance matrix and its inverse for baseline features
3. Slide window through network traffic data
4. Calculate drift = Mahalanobis distance between window mean and baseline mean
5. Normalize scores to [0,1] range and apply threshold (top 75th percentile = attack detected)

### Mathematical Formulation:

**Mahalanobis Distance:** Drift Score = √[(μ_window - μ_baseline)ᵀ × Σ⁻¹ × (μ_window - μ_baseline)]

Where:
- Σ = Covariance matrix of baseline window
- Σ⁻¹ = Inverse of covariance matrix
- μ_window = mean vector of current window features
- μ_baseline = mean vector of baseline (normal traffic) window

This calculates the correlation-aware distance between the current traffic pattern and the normal traffic baseline. Mahalanobis distance accounts for feature relationships and scale differences, making it effective for attack detection.

## Datasets Used

### CICIDS-2017:
- 691,406 samples
- Attack types: DDoS, DoS, Port Scan, Brute Force, Web Attack
- Attack percentage: 36.4%
- Features: 5 network flow features

### UNSW-NB15:
- 257,673 samples
- Attack types: Fuzzers, Analysis, Backdoors, DoS, Exploits, Generic, Reconnaissance, Shellcode, Worms
- Attack percentage: 63.9%
- Features: 5 network flow features

## Visualizations

### Figure 1: Real-time WFDS Drift Detection
![Figure 1](Figure1_WFDS_Detection.png)
Shows real-time attack detection over time windows. Red areas indicate detected attacks when drift score exceeds threshold.

### Figure 2: Confusion Matrices
![Figure 2](Figure2_Confusion_Matrices.png)
Comparison of classification performance between WFDS (unsupervised) and Random Forest (supervised).

### Figure 3: Performance Comparison
![Figure 3](Figure3_Performance.png)
Bar chart comparing Accuracy, Precision, Recall, F1-Score, and AUC between WFDS and Random Forest.

### Figure 4: ROC Curves
![Figure 4](Figure4_ROC_Curves.png)
ROC curves with AUC scores showing detection performance on both datasets.

### Figure 5: Score Distribution
![Figure 5](Figure5_Score_Distribution.png)
Distribution of WFDS drift scores for benign vs attack windows.

### Figure 6: Dataset Comparison
![Figure 6](Figure6_Dataset_Comparison.png)
Side-by-side comparison of WFDS vs Random Forest performance across both datasets.

## When to Use WFDS vs Machine Learning

### Use WFDS when:
- No labeled training data available (cold-start deployment)
- Real-time detection needed on edge devices
- Computational resources are limited
- Explainability of decisions is required

### Use Machine Learning when:
- Labeled training data is available
- Maximum accuracy is required
- Computational resources are sufficient for training
- Static patterns are expected over time

## Limitations

- WFDS performs lower on complex multi-class attack datasets (UNSW AUC 69.1% vs CICIDS 83.5%)
- Requires covariance matrix inversion (computational overhead)
- Assumes temporal ordering of data (data should not be shuffled)
- Fixed window size may not be optimal for all network traffic patterns

## Future Improvements

- Adaptive window sizing based on traffic patterns
- Ensemble of multiple distance metrics
- Feature weighting to learn important features from historical data
- Hybrid system combining WFDS with ML for optimal performance
- Live PCAP processing for real-time packet capture
- Web dashboard for visualization (Streamlit/FastAPI)
- Alert system for email/Slack notifications

## Technologies Used

| Category | Technologies |
|----------|--------------|
| Language | Python 3.8+ |
| ML & Processing | Scikit-learn, Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| ML Model | Random Forest |
| Distance Metric | Mahalanobis Distance |
| Datasets | CICIDS-2017, UNSW-NB15 |

## Acknowledgments

- CICIDS-2017 Dataset from Canadian Institute for Cybersecurity
- UNSW-NB15 Dataset from UNSW Canberra Cyber
- Scikit-learn, Pandas, NumPy, Matplotlib, Seaborn open-source community

---

*Built for zero-training network intrusion detection and edge device security*

