# Prévision horaire de la demande Capital Bikeshare (2024-2025)
## Instructions pour exécuter le projet
1. Installer les dépendances
Installez les packages listés dans `requirements.txt` :

```bash
pip install -r requirements.txt

2. Télécharger les données
Télécharger tous les fichiers ZIP originaux depuis ce lien Google Drive :
https://drive.google.com/drive/folders/1wvS9tY4KiMKVPbYLi3ylZWoOo1IMFRNC?usp=drive_link 

Placer tous les fichiers ZIP dans le même dossier que le notebook (Bike_Sharing_Project/).

3. Lancer le notebook
Depuis le dossier Bike_Sharing_Project/, lancer :

jupyter notebook

Copier l’URL affichée dans le terminal et la coller dans un navigateur web

Ouvrir Bike_sharing.ipynb et exécuter toutes les cellules

Le code dézippera automatiquement les fichiers ZIP et préparera le dataset complet. Les fichiers déjà extraits ne seront pas écrasés.

## Introduction
Ce projet vise à prédire le nombre de trajets horaires du réseau Capital Bikeshare à Washington D.C. sur 2025, en se basant sur les données historiques 2024-2025. L'objectif est de fournir une baseline robuste pour planifier la flotte et les ressources.

## Dataset
- Données publiques 2024-2025, fichiers CSV mensuels compressés en ZIP.

## Méthodologie
1. Décompression et concaténation des CSV → `df_all_2024_2025.csv`.
2. Transformation en série temporelle : index `started_at` + features horaires (`weekday`, `hour`, `is_weekend`) + jours fériés.
3. Construction de trois baselines :
   - Naïve H-24
   - Baseline 2 : H-24 + moyenne historique jour-heure (alpha optimisé)
   - Baseline 3 : H-24 + saisonnalité + rolling 3h + ajustement week-end/jours fériés
4. Évaluation MAE et sMAPE sur test 2025.
5. Visualisation des résultats sur l’année et zoom sur périodes critiques (pics, jours fériés).

## Performances (Test 2025)
| Modèle          | MAE     | sMAPE   |
|-----------------|--------|---------|
| Naive H24       | 194.83 | 33.88%  |
| Baseline 2      | 174.09 | 29.22%  |
| Baseline 3      | 165.33 | 29.93%  |

## Transparence et reproductibilité
- Temps total consacré : environ 4 h (avec quelques pauses).
Utilisation de l'IA: Pour certaines parties répétitives ou techniques, comme la génération de graphiques, le formatage des plots et la vérification de syntaxe, j’ai utilisé des outils d’assistance IA (ChatGPT). Cela m’a permis de me concentrer sur l’essentiel : la décision analytique, le choix des baselines, l’interprétation des résultats et l’évaluation de la robustesse du modèle. Toutes les décisions méthodologiques et analyses finales ont été réalisées par moi-même.
- Notebooks et scripts incluent toutes les étapes, `requirements.txt` fourni.
- Les métriques utilisées : MAE et sMAPE (simplicité et interprétabilité opérationnelle), choix justifié pour efficacité et priorité sur les tâches essentielles.
- Utilisation d’outils : pandas, numpy, matplotlib, scikit-learn.
## Conclusion
La Baseline 3 combine l’historique, la saisonnalité et l’effet court terme pour fournir une prédiction robuste et exploitable opérationnellement. Les limitations principales concernent les pics exceptionnels et l’effet de la météo (température, pluie, etc.), qui n’étaient pas disponibles dans le dataset. Avec ces informations, le modèle pourrait être enrichi pour mieux capturer ces variations.
