# Thermal Surrogate Bench

Controlled benchmark of neural, physics-regularized, and quantum-inspired surrogate models for two-dimensional steady-state heat conduction.

This repository contains the implementation associated with the paper:

**“A Controlled Benchmark of Neural, Physics-Regularized, and Quantum-Inspired Surrogates for Two-Dimensional Heat Conduction”**

> Status: work in progress / under preparation for submission.

---

## Overview

This project evaluates surrogate models for predicting two-dimensional temperature fields in steady-state heat conduction. The benchmark uses the public D6 heat-conduction dataset and compares models in terms of predictive accuracy, computational efficiency, model capacity, and physics-oriented consistency.

The goal is not to replace numerical solvers universally, nor to claim validation on full three-dimensional conjugate heat-transfer systems. Instead, the repository provides a controlled and reproducible benchmark on a structured two-dimensional thermal-field prediction task.

---

## Main Features

- **Neural baselines**
  - Mean-field baseline
  - Multilayer Perceptron (MLP)
  - Convolutional Neural Network (CNN)
  - Lightweight U-Net

- **Graph-based surrogate modeling**
  - Grid-based Graph Convolutional Network (Grid-GCN)

- **Physics regularization**
  - Masked discrete Laplacian residual
  - Interior and boundary error analysis
  - Ablation of the physics-regularization coefficient `lambda`

- **Randomized-feature baselines**
  - Extreme Learning Machine (ELM)
  - Emulated Quantum-Inspired Extreme Learning Machine (eQELM)

- **Capacity-aware evaluation**
  - MAE
  - RMSE
  - R²
  - Relative L2 error
  - Number of parameters
  - Inference latency
  - Masked physics residual
  - Interior MAE
  - Boundary MAE

---

## Repository Structure

```text
thermal-surrogate-bench/
├── notebooks/
│   └── d6_surrogate_modeling_amp_eqelm_full.ipynb
├── src/
│   ├── data/
│   ├── models/
│   ├── training/
│   ├── metrics/
│   └── utils/
├── results/
│   ├── tables/
│   ├── figures/
│   └── checkpoints/
├── requirements.txt
├── README.md
└── LICENSE


Installation
git clone https://github.com/gedeonnkishi/thermal-surrogate-bench.git
cd thermal-surrogate-bench
pip install -r requirements.txt

Dataset

The benchmark uses the public D6 heat-conduction dataset.
The official data partitions are strictly preserved:

xTrain / yTrain          -> training
xValidation / yValidation -> validation and model selection
xTest / yTest            -> final evaluation

Training
A typical training run can be launched with:
python train.py --model unet --epochs 30 --batch_size 64
To train the physics-regularized Grid-GCN:
python train.py --model grid_gcn --use_physics_loss --lambda_phys 1e-4
To run the physics-regularization ablation:
python train.py --model grid_gcn --run_lambda_ablation

Evaluation
The benchmark reports both predictive and physics-oriented metrics:
MAE
RMSE
R²
Relative L2 error
Number of parameters
Inference latency
Masked Laplacian residual
Interior MAE
Boundary MAE
The masked Laplacian residual is computed only on the interior of the computational domain in order to avoid penalizing boundary pixels as if they satisfied the same Laplace-type behavior as interior points.

Preliminary Results

Final results will be updated after the definitive training campaign.
| Model              | MAE | RMSE | Rel. L2 | Masked Residual | Params | Latency (ms) |
| ------------------ | --: | ---: | ------: | --------------: | -----: | -----------: |
| Mean baseline      | TBD |  TBD |     TBD |             TBD |      0 |          TBD |
| MLP                | TBD |  TBD |     TBD |             TBD |    TBD |          TBD |
| CNN                | TBD |  TBD |     TBD |             TBD |    TBD |          TBD |
| U-Net              | TBD |  TBD |     TBD |             TBD |    TBD |          TBD |
| Grid-GCN           | TBD |  TBD |     TBD |             TBD |    TBD |          TBD |
| Grid-GCN + Physics | TBD |  TBD |     TBD |             TBD |    TBD |          TBD |
| ELM                | TBD |  TBD |     TBD |             TBD |    TBD |          TBD |
| eQELM              | TBD |  TBD |     TBD |             TBD |    TBD |          TBD |


Important Notes

The eQELM model implemented in this repository is a quantum-inspired randomized-feature baseline executed on classical hardware. It should not be interpreted as a demonstration of quantum advantage.

The Grid-GCN model is constructed from a regular two-dimensional grid. It should therefore be interpreted as a controlled graph-based baseline, not as a full mesh-native graph neural network for arbitrary unstructured meshes.

The D6 dataset is a two-dimensional conduction benchmark. Results should not be directly generalized to industrial three-dimensional conjugate heat-transfer systems.

Citation

If you use this code or benchmark, please cite the associated work:

@article{Nkishi2026thermal,
  title   = {A Controlled Benchmark of Neural, Physics-Regularized, and Quantum-Inspired Surrogates for Two-Dimensional Heat Conduction},
  author  = {Nkishi, Gedeon Djenga},
  journal = {Under preparation},
  year    = {2026}
}
The citation will be updated after acceptance or publication.

Contact

Gedeon Nkishi Djenga
University of Kinshasa
Electrical and Computer Engineering
+243 820089373

