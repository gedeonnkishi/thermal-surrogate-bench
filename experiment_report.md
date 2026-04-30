# Rapport expérimental — D6 Surrogate Modeling

## Dataset

- Fichier `.npz` : `/kaggle/input/datasets/nkishidjengagdon/d6-heat-transfer-phenomena/HeatTransferPhenomena_35_58.npz`
- Train officiel : `30912` échantillons
- Validation officielle : `6624` échantillons
- Test officiel : `6624` échantillons
- Format : `N,C,H,W`
- Entrée : `C=2`, `H=64`, `W=64`
- Sortie : `C=1`, `H=64`, `W=64`

## Protocole

- Normalisation : min-max par canal, statistiques calculées uniquement sur le train officiel.
- Sélection : meilleure validation MSE.
- Évaluation finale : test officiel uniquement.
- Seeds : `(42,)`
- Device : `cuda`
- Nombre de GPU détectés : `2`
- DataParallel activé si possible : `False`
- Masque physique : `rectangular`
- Cellules intérieures : `3844`
- Cellules frontières : `252`
- Ablation physics_lambda : `True` ; lignes d'ablation générées : `5`
- Baselines rapides Ridge/ELM/eQELM : `True`
- eQELM : features trigonométriques quantum-inspired, sans simulation QPU/circuit complet

## Meilleur modèle selon RMSE normalisée

- Modèle : `UNetSmall`
- Seed : `42`
- RMSE normalisée : `0.00216232`
- Relative L2 normalisée : `0.0067672`
- MAE normalisée : `0.0011357`
- R² normalisé : `0.999936`
- RMSE physique : `0.00216228`
- Résidu laplacien prédit masqué MSE : `0.0440694`
- Erreur de Laplacien masquée MSE : `3.50319e-05`
- MAE intérieur : `0.00117439`
- MAE frontière : `0.000545159`
- Paramètres : `1928129`
- Taille modèle MB : `7.355`

## Lecture critique

1. Si U-Net ou DilatedCNN domine, c'est cohérent avec la nature régulière 2D du benchmark D6.
2. Si GridGCN est inférieur, cela ne réfute pas les GNN : le graphe testé ici reste régulier.
3. Si GridGCN + PhysicsLoss réduit le résidu laplacien mais dégrade RMSE, le coefficient physique doit être ajusté.
4. L'ablation `physics_lambda` doit être lue comme un compromis entre erreur statistique et cohérence physique.
5. Le tableau de capacité est indispensable : un modèle plus précis mais beaucoup plus gros ou plus lent n'est pas automatiquement meilleur.
6. Pour une soumission finale, utiliser plusieurs seeds et reporter moyenne ± écart-type.
7. La ligne `Pooled_eQELM` doit être interprétée comme une baseline quantum-inspired rapide, pas comme une preuve de supériorité du calcul quantique.

## Fichiers générés

- `tables/results_by_seed.csv`
- `tables/results_summary_mean_std.csv`
- `tables/capacity_performance_physics_comparison.csv`
- `tables/interior_boundary_physics_metrics.csv`
- `tables/physics_lambda_ablation.csv`
- `tables/results_by_seed.tex`
- `tables/results_summary_mean_std.tex`
- `tables/capacity_performance_physics_comparison.tex`
- `figures/validation_mse_curves.png`
- `figures/qualitative_predictions_best_models.png`
- `figures/rmse_vs_laplacian_error_masked.png`
- `figures/parameters_vs_rmse_norm.png`
- `figures/latency_vs_rmse_norm.png`
- `figures/physics_lambda_ablation.png`
