# AI-CDFI Cross-Day Domain Shift

Reproducibility materials for the research study:

**Evaluating Transformer-Based Intrusion Detection Under Cross-Day Domain Shift for AI-Driven Cloud Digital Forensics**

## Overview

This repository contains the reproducibility materials associated with the above research study. The study evaluates Transformer-based network intrusion detection under cross-day domain shift and investigates unsupervised domain adaptation using a Domain-Adversarial Neural Network (DANN).

The repository provides the experimental configuration, preprocessing information, model configuration, experiment descriptions, evaluation results, explainability configuration, and forensic graph construction details required to understand and reproduce the reported experiments.

## Dataset

The experiments use the **CSE-CIC-IDS2018** dataset developed through the Canadian Institute for Cybersecurity (CIC) at the University of New Brunswick (UNB).

Official dataset source:

https://www.unb.ca/cic/datasets/ids-2018.html

The experiments reported in the manuscript use the following six daily domains:

* 02-14-2018
* 02-15-2018
* 02-16-2018
* 02-22-2018
* 02-28-2018
* 03-01-2018

The dataset itself is not redistributed in this repository. Users should obtain the dataset from the official source and comply with the dataset's stated terms and citation requirements.

## Experimental Configuration

### Data processing

* Dataset: CSE-CIC-IDS2018
* Number of daily domains: 6
* Temporal sequence length: 32 events
* Data split: 70% training / 15% validation / 15% test
* Block size: approximately 200 events
* Event projection dimension: 128

### Transformer model

* Transformer layers: 3
* Attention heads: 4
* Feed-forward dimension: 256
* Dropout: 0.15
* Optimizer: AdamW
* Learning rate: 1e-4
* Batch size: 128
* Maximum training epochs: 20
* Early stopping patience: 5

### Cross-Day Evaluation

Each daily domain is treated as an independent source-day domain. A separate Transformer model is trained from scratch for each source-day domain.

The trained source-domain models are then evaluated across the six daily domains, producing the complete set of 30 ordered source-to-target cross-day evaluations.

## Domain Adaptation

Domain adaptation is evaluated using a Domain-Adversarial Neural Network (DANN).

The adaptation objective is:

L_DANN = L_class + λ L_domain

where:

* L_DANN is the total adaptation objective;
* L_class is the source-domain anomaly classification loss;
* L_domain is the source/target domain-discrimination loss; and
* λ is the adversarial adaptation coefficient.

The primary DANN configuration uses:

* λ = 0.10
* Learning rate = 1e-4
* Maximum adaptation epochs = 15
* Unlabelled target-domain data during adaptation

Five representative source-target pairs are evaluated using DANN:

* 02-14-2018 → 02-15-2018
* 02-14-2018 → 02-22-2018
* 02-14-2018 → 02-28-2018
* 02-15-2018 → 02-16-2018
* 02-28-2018 → 03-01-2018

## Lambda Sensitivity Analysis

The effect of the DANN adaptation coefficient is evaluated using:

* λ = 0.05
* λ = 0.10
* λ = 0.20

## Supervised Target-Domain Fine-Tuning

Supervised target-domain fine-tuning is included as an upper-bound comparator for selected source-target domain pairs.

This experiment uses labelled target-domain data and is not treated as unsupervised domain adaptation.

## Explainability

Integrated Gradients is used to analyse feature-level and temporal attribution for model predictions.

The Integrated Gradients calculation uses:

* Baseline input: reference input
* Integration steps: 16

The attribution formulation used in the study is:

IG_i(x) = (x_i - x'_i) ∫₀¹ ∂F(x' + α(x - x')) / ∂x_i dα

## Temporal Forensic Graphs

Flagged network events are converted into deterministic temporal forensic graphs.

The graph construction uses:

* Temporal window: 300 seconds

The graph representation is intended to support forensic correlation and temporal interpretation of anomalous network activity.

## Evaluation Metrics

The experiments report:

* Precision
* Recall
* F1-score
* ROC-AUC
* PR-AUC

## Repository Structure

The repository is organized to separate configuration, preprocessing, models, experiments, explainability, forensic graph construction, and reported results.

```text
AI-CDFI-Cross-Day-Domain-Shift/
│
├── README.md
├── LICENSE
├── requirements.txt
│
├── data/
│   └── README.md
│
├── preprocessing/
│   ├── README.md
│   └── preprocessing.py
│
├── transformer/
│   ├── model.py
│   ├── train.py
│   └── evaluate.py
│
├── dann/
│   ├── dann_model.py
│   ├── train_dann.py
│   └── evaluate_dann.py
│
├── explainability/
│   ├── integrated_gradients.py
│   └── README.md
│
├── forensic_graph/
│   ├── temporal_graph.py
│   └── README.md
│
├── experiments/
│   ├── same_day_experiment.py
│   ├── cross_day_experiment.py
│   ├── dann_experiment.py
│   ├── lambda_sensitivity.py
│   └── supervised_finetuning.py
│
├── configs/
│   └── experiment_config.yaml
│
├── results/
│   ├── same_day_results.csv
│   ├── cross_day_results.csv
│   ├── dann_results.csv
│   └── lambda_sensitivity_results.csv
│
└── supplementary/
    └── reproducibility_notes.pdf
```

## Reproducibility

To reproduce the experiments:

1. Obtain CSE-CIC-IDS2018 from the official UNB/CIC source.
2. Select the six daily domains listed above.
3. Apply the preprocessing and temporal sequence construction described in the repository.
4. Train a separate Transformer model for each source-day domain.
5. Evaluate the models on the six daily domains.
6. Run the specified DANN experiments for the five source-target pairs.
7. Run the λ sensitivity experiments.
8. Run the supervised target-domain fine-tuning experiments.
9. Apply Integrated Gradients for model attribution analysis.
10. Construct temporal forensic graphs using the specified 300-second temporal window.

## Reported Results

The `results/` directory contains the result files corresponding to the experiments reported in the manuscript.

These files are provided to support verification of the reported performance values and experimental comparisons.

## Citation

If you use this repository or the associated experimental materials, please cite the corresponding research article:

**Waziri, M. R. et al. Evaluating Transformer-Based Intrusion Detection Under Cross-Day Domain Shift for AI-Driven Cloud Digital Forensics. Cureus Journal of Computer Science.**

The final bibliographic information and repository citation will be updated when the article and public repository are formally published.

## Data Availability

The CSE-CIC-IDS2018 dataset is available from the Canadian Institute for Cybersecurity / University of New Brunswick:

https://www.unb.ca/cic/datasets/ids-2018.html

The dataset is not included in this repository.

## Code Availability

The source code and reproducibility materials associated with the experiments are provided in this public repository.

The repository is intended to support transparency and reproducibility of the experiments reported in the manuscript.
