# Système d’Aide à la Décision pour la Fidélisation Client et l’Augmentation du Chiffre d’Affaires dans le E-commerce

## Description

Ce projet a été réalisé dans le cadre du module **Data-Driven Decision Making (DDDM)**.

L’objectif est de développer un système d’aide à la décision permettant à une plateforme e-commerce d’exploiter ses données afin de :

- Améliorer la fidélisation des clients ;
- Augmenter le chiffre d’affaires ;
- Comprendre les comportements d’achat ;
- Identifier les segments clients à forte valeur ;
- Prédire la fidélité des clients ;
- Fournir des recommandations décisionnelles basées sur les données.

Le projet couvre l’ensemble du cycle analytique, depuis la compréhension métier jusqu’à la visualisation décisionnelle et la modélisation prédictive.

---

## Problématique

Dans un environnement e-commerce fortement concurrentiel, la fidélisation client représente un levier stratégique majeur.

La question centrale étudiée est :

> Comment une plateforme e-commerce peut-elle exploiter les données de ventes, de comportement d’achat et de satisfaction client afin d’améliorer la fidélisation des clients et d’augmenter son chiffre d’affaires ?

### Questions décisionnelles

- Quels clients présentent un risque élevé de désengagement ?
- Quels facteurs influencent le plus la satisfaction client ?
- Quels produits génèrent le plus de revenus ?
- Quel impact les retards de livraison ont-ils sur la satisfaction client ?
- Quels segments de clients doivent être ciblés en priorité ?

---

## Jeux de Données Utilisés

### Olist Brazilian E-Commerce Dataset

- Source : Kaggle
- Environ 100 000 commandes
- Informations sur les clients, commandes, paiements, produits, avis et livraisons

### Instacart Market Basket Analysis

- Source : Kaggle
- 3,4 millions de commandes
- 206 000 clients
- Historique d’achats et composition des paniers

### Amazon Product Reviews

- Source : Kaggle
- 4 915 avis produits
- Notes et commentaires clients

---

## Architecture du Projet

```text
Business Understanding
        ↓
Collecte & Audit des Données
        ↓
ETL (Extract - Transform - Load)
        ↓
Exploratory Data Analysis (EDA)
        ↓
Tests Statistiques
        ↓
Segmentation Clients (K-Means)
        ↓
Feature Engineering
        ↓
Machine Learning
        ↓
Validation des Modèles
        ↓
Interprétabilité SHAP
        ↓
Dashboard Power BI
        ↓
Recommandations Décisionnelles
```

---

## Pipeline ETL

### Extract

- Chargement des fichiers CSV
- Importation des tables sources

### Transform

- Suppression des doublons
- Gestion des valeurs manquantes
- Conversion des dates
- Agrégation des paiements
- Filtrage des commandes livrées
- Création de variables métier

Variables créées :

- Recency
- Frequency
- Monetary (RFM)
- Délais de livraison
- Fréquence d’achat
- Taux de retard
- Valeur client

### Load

Génération du dataset analytique final :

```text
customer_analytics.csv
```

Caractéristiques :

- 93 350 clients
- 19 variables analytiques
- Aucune valeur manquante

---

## Analyse Exploratoire des Données

### Analyse des Ventes

- Chiffre d’affaires total
- Évolution temporelle des ventes
- Panier moyen
- Catégories les plus rentables

### Analyse des Clients

- Taux de fidélisation
- Répartition géographique
- Dépenses moyennes

### Analyse des Produits

- Produits les plus vendus
- Produits les plus rentables
- Répartition des catégories

### Analyse de la Satisfaction

- Distribution des notes
- Avis positifs et négatifs
- Satisfaction par catégorie

### Analyse Logistique

- Délais de livraison
- Taux de retard
- Impact sur la satisfaction

---

## Tests Statistiques

Les hypothèses métier ont été validées à l’aide de plusieurs méthodes statistiques :

- T-Test de Welch
- Mann-Whitney U
- Chi-Square
- ANOVA
- Kruskal-Wallis

### Principaux Résultats

- Les retards de livraison réduisent significativement la satisfaction client.
- Certaines catégories de produits obtiennent des niveaux de satisfaction différents.
- Les clients fidèles dépensent davantage que les clients occasionnels.
- Les performances logistiques varient selon les régions.

---

## Segmentation Client

### Méthode

Segmentation RFM à l’aide de l’algorithme K-Means.

### Nombre de Clusters

```text
K = 4
```

### Segments Identifiés

#### Champions

Clients actifs avec une forte fréquence d’achat.

#### Clients Fidèles

Clients à forte valeur générant une part importante du chiffre d’affaires.

#### Clients Occasionnels

Clients récents avec une faible fréquence d’achat.

#### Clients à Risque

Clients inactifs depuis une longue période nécessitant des actions de réactivation.

---

## Modélisation Prédictive

### Objectif

Prédire si un client réalisera un nouvel achat.

### Variable Cible

```python
is_loyal
```

### Variables Utilisées

- recency_days
- total_spent
- is_high_value
- state_encoded
- log_total_spent
- log_recency

### Modèles Évalués

| Modèle | F1-Score | AUC-ROC |
|---------|---------|----------|
| Logistic Regression | 0.5361 | 0.7819 |
| Random Forest | 0.5521 | 0.7867 |
| XGBoost | 0.5269 | 0.7681 |

### Modèle Retenu

Random Forest

Paramètres optimaux :

```python
n_estimators = 200
max_depth = 10
min_samples_split = 20
```

---

## Validation des Modèles

Méthodes utilisées :

- Validation croisée stratifiée (5-Fold)
- GridSearchCV

Résultats :

- Bonne stabilité
- Bonne capacité de généralisation
- Pas d’overfitting significatif

---

## Interprétabilité avec SHAP

L’outil SHAP a été utilisé pour expliquer les prédictions du modèle Random Forest.

### Variables les Plus Influentes

1. log_total_spent
2. total_spent
3. is_high_value
4. state_encoded
5. log_recency
6. recency_days

### Insight Principal

Les dépenses totales constituent le facteur le plus important dans la prédiction de la fidélité client.

---

## Dashboard Power BI

Le projet inclut un tableau de bord interactif composé de cinq pages :

### Page 1 : Vue d’Ensemble

- KPIs stratégiques
- Chiffre d’affaires
- Fidélisation

### Page 2 : Analyse Clients

- Répartition géographique
- Dépenses
- Segmentation

### Page 3 : Analyse Produits

- Performance commerciale
- Catégories de produits

### Page 4 : Satisfaction & Livraison

- Notes clients
- Retards de livraison
- Performance logistique

### Page 5 : Habitudes d’Achat

- Analyse comportementale
- Fréquence d’achat
- Répartition des commandes

---

## Recommandations Décisionnelles

### Programme de Fidélisation Premium

- Cibler les clients fidèles et les champions
- Augmenter la fréquence d’achat
- Récompenser les meilleurs clients

### Campagne de Réactivation

- Cibler les clients à risque
- Réduire le churn
- Générer des achats supplémentaires

### Déploiement du Scoring Machine Learning

- Calcul automatique de la propension à la fidélité
- Personnalisation des campagnes marketing
- Optimisation des investissements promotionnels

---

## Technologies Utilisées

### Traitement des Données

- Python
- Pandas
- NumPy

### Visualisation

- Matplotlib
- Seaborn
- Power BI

### Statistiques

- SciPy
- Statsmodels

### Machine Learning

- Scikit-Learn
- XGBoost

### Explainable AI

- SHAP

### Environnement

- Jupyter Notebook

---


---

## Auteurs

- Aya Mlouki
- Wissal Kallah

### Encadrant

Pr. Youness Tabii

---

## Contexte Académique

Projet réalisé dans le cadre du module **Data-Driven Decision Making (DDDM)** à l’École Nationale Supérieure d’Informatique et d’Analyse des Systèmes (ENSIAS).
