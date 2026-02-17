
# Rapport d'Expertise : Analyse Quantitative de Portefeuilles et Modélisation du Risque de Crédit

**Auteur :** Analyseur de Données

**Date :** 17 février 2026

**Domaine :** Ingénierie Financière et Apprentissage Automatique

Nom et prénom : RIAD Douaa 
Nom et prénom : Marzaq Fatima ezzahra 



---

## Résumé Éxecutif

Ce document présente une étude analytique rigoureuse divisée en deux piliers fondamentaux de la finance moderne. La première partie expose une évaluation comparative de deux stratégies d'investissement (Portefeuilles A et B) à travers des indicateurs de performance et de risque. La seconde partie détaille la conception d'un système de *Scoring de Crédit* s'appuyant sur l'algorithme des  plus proches voisins (), incluant les protocoles de prétraitement indispensables à la fiabilité des prédictions.

---

## I. Analyse Statistique de la Performance Financière

L'étude porte sur des rendements mensuels historiques afin de caractériser le comportement de deux portefeuilles distincts : le **Portefeuille A** (Gestion Conservative) et le **Portefeuille B** (Gestion Agressive).

### 1.1 Comparaison des Métriques de Rendement

Les données révèlent une divergence significative de performance :

* **Rendement Moyen :** Le Portefeuille B affiche un rendement mensuel moyen de **2,89%**, soit plus du triple de celui du Portefeuille A (**0,94%**).
* **Annualisation :** Le rendement annualisé du Portefeuille B s'élève à **40,79%**, contre **11,85%** pour le profil conservateur.
* **Médiane :** La médiane de B (**4,70%**) est supérieure à sa moyenne, suggérant une distribution asymétrique, tandis que celle de A (**1,00%**) est très proche de sa moyenne.

### 1.2 Évaluation du Risque et de la Volatilité

Le risque est ici quantifié par la volatilité annualisée et la *Value at Risk* (VaR) :

* **Volatilité :** Le Portefeuille B est extrêmement volatil avec un écart-type annualisé de **15,41%**, contre seulement **1,65%** pour le Portefeuille A.
* **Value at Risk (95%) :**
* **Portefeuille A :** La perte potentielle annuelle est estimée à **9,12%**, soit un risque monétaire de **45 606,85 €**.
* **Portefeuille B :** Malgré ses rendements élevés, la perte annuelle potentielle est de **15,37%**, représentant **76 834,26 €**.



### 1.3 Efficience et Test de Normalité

Le **Ratio de Sharpe** a été calculé pour mesurer le rendement excédentaire par unité de risque (volatilité).

* Le Portefeuille A domine largement avec un ratio de **5,35**, contre **2,45** pour le Portefeuille B. Cela démontre que la stratégie conservative offre une bien meilleure rémunération du risque.
* **Test de Shapiro-Wilk :** Pour les deux portefeuilles, la p-value est très faible (A : **0,0003** ; B : **0,0012**). L'hypothèse nulle de normalité est rejetée, ce qui signifie que les rendements ne suivent pas une loi normale, justifiant l'utilisation de méthodes de gestion de risque plus complexes que la simple moyenne-variance.

---

## II. Modélisation Prédictive du Défaut de Paiement

La seconde phase du projet consiste à développer un modèle de classification capable d'identifier les clients susceptibles de faire défaut.

### 2.1 Architecture du Jeu de Données

Un échantillon de **1 000 observations** a été généré synthétiquement, intégrant huit variables explicatives critiques :

* **Données Socio-démographiques :** Âge et ancienneté dans l'emploi.
* **Indicateurs Financiers :** Salaire, dette totale, et surtout le **Ratio Dette/Revenu** (DTI).
* **Historique de Crédit :** Nombre de crédits actifs, historique des retards de paiement, et score de crédit bureau.

Le "target" (variable cible `defaut`) a été construit sur une logique probabiliste où un ratio dette/revenu supérieur à 0,5 ou un score de crédit inférieur à 600 augmente drastiquement la probabilité de défaut.

### 2.2 Protocole de Prétraitement (Pipeline)

Le succès d'un algorithme de type KNN repose sur une préparation rigoureuse des données :

1. **Partitionnement Stratifié :** Les données ont été divisées en ensembles d'entraînement (70%) et de test (30%). L'utilisation de `stratify=y` garantit que la proportion de cas de défaut est identique dans les deux sous-ensembles, évitant ainsi les biais d'apprentissage.
2. **Standardisation (Scaling) :** L'algorithme KNN calcule des distances euclidiennes entre les points. Sans normalisation, une variable comme le "Salaire" (en dizaines de milliers) écraserait la variable "Nombre de crédits" (de 0 à 10). L'utilisation du `StandardScaler` (Z-score normalization) remédie à ce problème en ramenant chaque caractéristique à une moyenne de 0 et une variance de 1.

### 2.3 Mise en Œuvre du Modèle KNN

Le classifieur `KNeighborsClassifier` a été initialisé pour traiter les données ainsi préparées. Ce modèle procède par analogie : un nouveau client est classé comme "en défaut" s'il ressemble statistiquement aux clients ayant déjà fait défaut dans le passé.

---

## III. Conclusion et Recommandations

### 3.1 Synthèse des Résultats

L'analyse montre que la recherche de rendements élevés (Portefeuille B) s'accompagne d'une dégradation de l'efficience (Sharpe plus faible) et d'une exposition au risque de perte monétaire bien plus importante. Concernant le volet prédictif, la méthodologie employée (stratification et normalisation) fournit une base solide pour un système de scoring de crédit automatisé.

### 3.2 Perspectives d'Amélioration

Pour optimiser ces résultats, il est recommandé de :

1. **Optimisation des Hyperparamètres :** Effectuer une recherche par grille (*GridSearch*) pour déterminer la valeur optimale de  dans le modèle KNN.
2. **Ingénierie de Caractéristiques :** Explorer des modèles non-linéaires comme les Forêts Aléatoires (*Random Forest*) pour capturer des interactions plus complexes entre les variables financières.
3. **Gestion de Risque :** Intégrer des stress-tests basés sur les résultats du test de Shapiro-Wilk, étant donné que la non-normalité des rendements expose à des risques de "queues de distribution" (Tail Risk).

---

### Annexe : Bibliographie Logicielle

* **Manipulation de données :** `pandas`, `numpy`.
* **Visualisation :** `matplotlib`, `seaborn`.
* **Analyse Statistique :** `scipy.stats`.
* **Machine Learning :** `scikit-learn`.
