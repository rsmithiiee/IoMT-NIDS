# Healthcare IoT Network Intrusion Detection System

A machine learning-based Network Intrusion Detection System (NIDS) for Internet of Medical Things (IoMT) devices, trained and evaluated on the [CIC IoMT Dataset 2024](https://www.unb.ca/cic/datasets/iomt-dataset-2024.html). This project benchmarks three classification algorithms — Random Forest, Histogram Gradient Boosting, and Multi-layer Perceptron — against both binary (attack/benign) and multiclass (attack category) detection tasks.

---

## Table of Contents

- [What This Project Does](#what-this-project-does)
- [Why It's Useful](#why-its-useful)
- [Dataset](#dataset)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Dataset Setup](#dataset-setup)
  - [Running the Notebook](#running-the-notebook)
- [Project Structure](#project-structure)
- [Methodology](#methodology)
- [Results](#results)
- [Where to Get Help](#where-to-get-help)
- [Contributors](#contributors)

---

## What This Project Does

This project trains and evaluates multiple ML classifiers to detect network intrusions targeting IoMT devices. Given raw network traffic features extracted from pcap files, the system can:

- **Binary classify** traffic as either benign or malicious
- **Multiclass classify** traffic into one of five attack categories: `DDoS`, `DoS`, `Recon`, `MQTT`, and `Spoofing`
- **Optimize** each classifier using `RandomizedSearchCV` hyperparameter tuning
- **Compare** pre- and post-optimization F1 scores across all models

---

## Why It's Useful

Healthcare IoT devices are increasingly targeted by cyberattacks, yet many lack robust intrusion detection capabilities. This project:

- Provides a **reproducible benchmark** for IoMT intrusion detection using the most comprehensive publicly available dataset in this domain (18 attack types, 40 devices, 3 protocols)
- Enables **side-by-side algorithm comparison** with standardized evaluation metrics (precision, recall, F1-score)
- Demonstrates the **impact of hyperparameter tuning** on detection performance
- Serves as a **starting point** for researchers and students building or extending IoMT security solutions

---

## Dataset

This project uses the **CIC IoMT Dataset 2024** from the Canadian Institute for Cybersecurity.

| Property | Details |
|---|---|
| Devices | 40 IoMT devices (25 real + 15 simulated) |
| Protocols | Wi-Fi, MQTT, Bluetooth |
| Attack types | 18 attacks across 5 categories |
| Attack categories | DDoS, DoS, Recon, MQTT, Spoofing |
| Format | Pre-split Train/Test CSV files (one per attack type) |

**Download the dataset:** [https://www.unb.ca/cic/datasets/iomt-dataset-2024.html](https://www.unb.ca/cic/datasets/iomt-dataset-2024.html)

---

## Getting Started

### Prerequisites

- Python 3.9+
- Jupyter Notebook or JupyterLab
- ~8 GB RAM recommended (the dataset is large; training is memory-intensive)

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/CMPE132Project.git
   cd CMPE132Project
   ```

2. Install the required Python packages:
   ```bash
   pip install pandas scikit-learn jupyter
   ```

### Dataset Setup

1. Download the CIC IoMT Dataset 2024 CSV files from the link above.
2. Organize them into the following directory structure:
   ```
   CMPE132Project/
   ├── Dataset/
   │   ├── Train/
   │   │   ├── BenignTraffic_train.pcap.csv
   │   │   ├── DDoS-ICMP_Flood_train.pcap.csv
   │   │   └── ...
   │   └── Test/
   │       ├── BenignTraffic_test.pcap.csv
   │       ├── DDoS-ICMP_Flood_test.pcap.csv
   │       └── ...
   └── NIDS.ipynb
   ```

3. Open `NIDS.ipynb` and update the dataset paths in the first code cell to match your local directory:
   ```python
   train_dir = r"path/to/your/Dataset/Train"
   test_dir  = r"path/to/your/Dataset/Test"
   ```

### Running the Notebook

Launch Jupyter and open the notebook:

```bash
jupyter notebook NIDS.ipynb
```

Run cells sequentially from top to bottom. Each section is self-contained and clearly labeled. Training the optimized models with `RandomizedSearchCV` is computationally expensive — expect long runtimes depending on your hardware.

---

## Project Structure

```
CMPE132Project/
├── NIDS.ipynb        # Main notebook: preprocessing, training, evaluation, comparison
└── README.md         # This file
```

The `Dataset/` directory is not included in the repository and must be downloaded separately (see [Dataset Setup](#dataset-setup)).

---

## Methodology

The notebook follows this pipeline:

1. **Data Loading** — Reads all per-attack-type CSV files, infers attack labels from filenames, and concatenates them into unified train/test DataFrames. Adds a `binary_classification` column (0 = benign, 1 = attack).

2. **Class Balancing** — Downsamples to a target of 50,000 training samples, stratified proportionally by class distribution, to reduce training time and class imbalance effects.

3. **Preprocessing** — Standardizes features with `StandardScaler`, then reduces dimensionality with PCA retaining 95% of variance.

4. **Model Training** — Trains three classifiers for both binary and multiclass tasks:
   - Random Forest (`RandomForestClassifier`)
   - Histogram Gradient Boosting (`HistGradientBoostingClassifier`)
   - Multi-layer Perceptron (`MLPClassifier`)

5. **Hyperparameter Optimization** — Applies `RandomizedSearchCV` to each model/task combination to find improved parameters.

6. **Evaluation & Comparison** — Generates classification reports and comparison tables showing precision, recall, F1-score before and after optimization for every model.

---

## Results

The notebook outputs per-class and aggregate (macro/weighted) F1 scores for all six model/task combinations, displayed as styled comparison tables. Final results are summarized in a unified F1 comparison table covering all algorithms and classification types.

To view results without re-running training, open `NIDS.ipynb` in GitHub or JupyterLab — the cell outputs are saved in the notebook file.

---

## Where to Get Help

- **Dataset questions:** See the [CIC IoMT 2024 dataset page](https://www.unb.ca/cic/datasets/iomt-dataset-2024.html)
- **scikit-learn documentation:** [https://scikit-learn.org/stable/](https://scikit-learn.org/stable/)
- - **pandas documentation:** [https://pandas.pydata.org/docs/](https://pandas.pydata.org/docs/)
- **Jupyter documentation:** [https://jupyter.org/documentation](https://jupyter.org/documentation)

---

## Contributors

Developed as a course project for **CMPE 132 – Information Security** at San José State University.
