# Note Méthode – Prévision horaire Capital Bikeshare

## 1. Définition du problème et horizon de prédiction

L’objectif est de prédire le nombre de trajets horaires sur le réseau Capital Bikeshare à Washington D.C. pour 2025, à partir des données 2024‑2025.  
**Horizon choisi : 24h (H+24)**, pertinent pour la planification quotidienne de la flotte.

## 2. Stratégie de validation et justification des splits temporels

**Baseline naïve H-24 :**  
- Train = 2024, Test = 2025.  
- Split strict pour simuler un futur hors-échantillon, cohérent avec l’objectif opérationnel.

**Baseline 2 et Baseline 3 :**  
- Train = début 2024 → fin octobre 2024  
- Validation = novembre‑décembre 2024  
- Test = 2025  

**Justification :**  
- Le split train/validation permet d’optimiser les hyperparamètres (α pour Baseline 2, poids combinés pour Baseline 3) sans regarder le futur réel.  
- Le test 2025 reste un vrai futur hors-échantillon, garantissant une évaluation réaliste et robuste.  
- Cette approche évite la fuite de données(data leakage) et permet de tester la généralisation des modèles sur des périodes jamais vues.

## 3. Baselines et améliorations

| Modèle        | MAE     | sMAPE  | Commentaire |
|---------------|---------|--------|-------------|
| Naive H-24    | 194.83  | 33.88% | Capture la tendance générale et les cycles journaliers. |
| Baseline 2    | 174.09  | 29.22% | H-24 + moyenne historique jour/heure (α optimisé). Corrige transitions entre jours. |
| Baseline 3    | 165.33  | 29.93% | H-24 + saisonnalité + rolling 3h + ajustements week-end/jours fériés. Captation plus fine des variations locales et pics atypiques. |

**Comparaison critique :**  
- La naïve H-24 est simple mais efficace pour la tendance globale.  
- Baseline 2 réduit les écarts lors des transitions de régime journalières grâce à la saisonnalité.  
- Baseline 3 affine encore avec l’inertie courte (rolling 3h) et les facteurs calendaires, améliorant la robustesse et la précision sur les jours atypiques.  
- Les améliorations sont progressives et motivées par l’observation des écarts et des patterns horaires.

## 4. Limites du modèle

- Les pics exceptionnels ou événements météorologiques restent difficiles à prédire.  
- Sur-estimation des amplitudes lors des périodes de forte activité.  
- Méthode encore heuristique, sans apprentissage automatique complexe.

## 5. Vision MLOps

Pour industrialiser ce modèle et le mettre en “run” :  
- **Pipeline automatisé :** ingestion des données, prétraitement, génération des features, calcul des baselines et stockage des résultats.  
- **Monitoring :** suivi des métriques clés (MAE, sMAPE) et détection de dérives saisonnières ou pics inhabituels pour assurer la robustesse en production.  
- **Ré-entraînement :** mise à jour périodique ou en cas de dérive détectée, recalcul des profils saisonniers et rolling features, sauvegarde et versioning des modèles.  
- **Reproductibilité :** scripts et pipelines paramétrés pour pouvoir relancer l’ensemble sur de nouvelles données ou années sans intervention manuelle.  

Cette approche permet d’avoir un modèle robuste, surveillé et facilement ré-entraînable, directement exploitable pour la planification opérationnelle.