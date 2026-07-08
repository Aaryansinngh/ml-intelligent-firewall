# ML Intelligent Firewall — Real-Time Network Intrusion Detection

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![LightGBM](https://img.shields.io/badge/LightGBM-gradient%20boosting-9ACD32?style=flat-square)
![Scikit--learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)
![Dataset](https://img.shields.io/badge/dataset-CICIDS2018-blue?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)

**A real-time, ML-based intrusion detection firewall achieving 96–98% detection accuracy with sub-millisecond inference latency.**

[Results](#results) · [Architecture](#architecture) · [Getting Started](#getting-started) · [Methodology](#methodology)

</div>

---

## Overview

This project implements a machine-learning-driven firewall capable of classifying network traffic in real time and flagging malicious flows before they reach their destination. It's trained on the CIC-IDS-2018 dataset and optimized specifically for the production constraint that matters most in a firewall: **inference speed**, not just accuracy.

## Results

| Metric | Value |
|---|---|
| Detection Accuracy | 96–98% |
| Inference Latency | < 1ms per sample |
| Model | LightGBM (gradient-boosted trees) |
| Hyperparameter Tuning | GridSearchCV |
| Optimization Target | False-positive rate minimization via decision-threshold tuning |

## Methodology

1. **Data preprocessing** — network flow records from CIC-IDS-2018 are cleaned, normalized, and encoded into model-ready feature vectors.
2. **Model training** — a LightGBM classifier is trained on the labeled flow data, chosen for its balance of speed and accuracy on tabular, high-dimensional traffic data.
3. **Hyperparameter tuning** — GridSearchCV systematically searches the hyperparameter space (tree depth, learning rate, leaf count, regularization) to maximize detection performance.
4. **Threshold optimization** — rather than using a default 0.5 decision threshold, the classification boundary is tuned specifically to minimize false positives, since a firewall that cries wolf too often gets ignored or disabled.
5. **Inline deployment design** — the model is packaged for real-time scoring of live traffic, processing flow records as they arrive rather than in offline batches.

## Architecture

```
┌──────────────────┐      ┌──────────────────────┐      ┌───────────────────┐
│  Network Traffic   │ ───► │  Feature Extraction   │ ───► │  LightGBM Classifier│
│  (flow records)     │      │  (CICFlowMeter-style)  │      │  (< 1ms inference)   │
└──────────────────┘      └──────────────────────┘      └─────────┬─────────┘
                                                                       │
                                                          ┌────────────▼────────────┐
                                                          │   Decision Engine          │
                                                          │   (tuned threshold)         │
                                                          └────────────┬────────────┘
                                                                       │
                                                        ┌──────────────▼──────────────┐
                                                        │   Allow / Block Traffic       │
                                                        └────────────────────────────┘
```

## Tech Stack

| Component | Technology |
|---|---|
| Model | LightGBM |
| Tuning | GridSearchCV |
| Dataset | CIC-IDS-2018 |
| Language | Python |
| Feature Extraction | CICFlowMeter |

## Getting Started

### Prerequisites

- Python 3.9+
- pip

### Installation

```bash
git clone https://github.com/Aaryansinngh/ml-intelligent-firewall.git
cd ml-intelligent-firewall
pip install -r requirements.txt
```

### Training

```bash
python train.py --dataset path/to/cicids2018.csv
```

### Running Inference

```bash
python infer.py --input sample_traffic.csv
```

> Update this section with your actual script names and CLI arguments — this is a placeholder based on the documented pipeline.

## Future Work

- [ ] Extend to CICIDS2017 for cross-dataset generalization testing
- [ ] Add streaming ingestion (Kafka/live packet capture) instead of batch CSV input
- [ ] Package as a deployable inline network appliance
- [ ] Add explainability (SHAP) for flagged traffic

## License

MIT — see [LICENSE](LICENSE) for details.

---

<div align="center">
Built by <a href="https://github.com/Aaryansinngh">Aryan Singh</a>
</div>
