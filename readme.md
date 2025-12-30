# 📘 Projet StressLess

## Analyse, visualisation et suivi du niveau de stress

**Préparé par : AKHRAIS Hasnae**

---

<img src="AKHRAIS HASNAE.jpg" style="height:464px;margin-right:432px"/>  

## 📑 Sommaire

1. [Contexte Métier et Mission](#1-contexte-métier-et-mission)
   
   1.1 [Problème (Business Case)](#11-problème-business-case)
   1.2 [Objectifs et enjeux métiers](#12-objectifs-et-enjeux-métiers)
   
2. [Les Données – Input du Système](#2-les-données--input-du-système)
   2.1 [Description des variables](#21-description-des-variables)
   2.2 [Nature et qualité des données](#22-nature-et-qualité-des-données)
3. [Laboratoire Python – Chaîne Technique](#3-laboratoire-python--chaîne-technique)
   
4. [Analyse Approfondie : Nettoyage et Préparation (Data Wrangling)](#4-analyse-approfondie--nettoyage-et-préparation-data-wrangling)
   4.1 [Problématique des valeurs manquantes](#41-problématique-des-valeurs-manquantes)
   4.2 [Mécanique de l’imputation](#42-mécanique-de-limputation)
   4.3 [Data Leakage : risque et bonne pratique](#43-data-leakage--risque-et-bonne-pratique)
   4.4 [Normalisation des variables](#44-normalisation-des-variables)
5. [Analyse Exploratoire des Données (EDA)](#5-analyse-exploratoire-des-données-eda)
   5.1 [Statistiques descriptives](#51-statistiques-descriptives)
   5.2 [Distribution et asymétrie](#52-distribution-et-asymétrie)
   5.3 [Corrélations et redondances](#53-corrélations-et-redondances)
6. [Méthodologie Expérimentale : Split & Généralisation](#6-méthodologie-expérimentale--split--généralisation)
7. [Focus Théorique : Modélisation ML (Random Forest)](#7-focus-théorique--modélisation-ml-random-forest)
   7.1 [Faiblesse d’un modèle isolé](#71-faiblesse-dun-modèle-isolé)
   7.2 [Bagging et diversité](#72-bagging-et-diversité)
   7.3 [Consensus et robustesse](#73-consensus-et-robustesse)
8. [Évaluation et Audit de Performance](#8-évaluation-et-audit-de-performance)
   8.1 [Matrice de confusion et erreurs](#81-matrice-de-confusion-et-erreurs)
   8.2 [Métriques clés et priorisation](#82-métriques-clés-et-priorisation)
9. [Visualisations et Interprétation](#9-visualisations-et-interprétation)
10. [Conclusion et Perspectives](#10-conclusion-et-perspectives)

---

## 1. Contexte Métier et Mission

### 1.1 Problème (Business Case)

Le stress chronique représente un enjeu majeur de santé publique et de performance individuelle. Mal anticipé, il peut entraîner une baisse de productivité, des troubles du sommeil et des risques psychosociaux. Le stress est une variable **subjective**, **multifactorielle** et **évolutive dans le temps**, ce qui complique son analyse.

### 1.2 Objectifs et enjeux métiers

**StressLess** vise à concevoir un **assistant analytique** permettant le suivi, la compréhension et l’anticipation des niveaux de stress à partir de données temporelles et comportementales. L’enjeu métier est **préventif** : détecter tôt les situations à risque afin d’orienter des actions correctives.

---

## 2. Les Données – Input du Système

### 2.1 Description des variables

| Variable       | Description                           |
| -------------- | ------------------------------------- |
| `date`         | Horodatage de l’observation           |
| `stress_level` | Niveau de stress (échelle normalisée) |
| `activity`     | Intensité de l’activité               |
| `sleep`        | Qualité/durée du sommeil              |
| `workload`     | Charge de travail                     |
| `user_id`      | Identifiant utilisateur               |

### 2.2 Nature et qualité des données

Les données sont **temporelles**, **quantitatives**, potentiellement **bruitées** et **incomplètes**, avec des variations inter-individuelles marquées. Ces caractéristiques imposent une préparation rigoureuse.

---

## 3. Laboratoire Python – Chaîne Technique

Le notebook Python constitue la **paillasse de laboratoire** du projet : acquisition, nettoyage, EDA, modélisation et évaluation. Il permet de simuler des scénarios réalistes et de tester la robustesse des choix méthodologiques.

---

## 4. Analyse Approfondie : Nettoyage et Préparation (Data Wrangling)

### 4.1 Problématique des valeurs manquantes

Les algorithmes statistiques et de Machine Learning ne peuvent pas traiter directement les valeurs `NaN`. Une seule valeur manquante peut invalider un calcul matriciel ou fausser une corrélation.

### 4.2 Mécanique de l’imputation

L’imputation (moyenne ou médiane) suit deux étapes :

1. **Apprentissage (fit)** : calcul du paramètre statistique sur les données disponibles.
2. **Transformation (transform)** : remplacement des valeurs manquantes par ce paramètre.

### 4.3 Data Leakage : risque et bonne pratique

Calculer les paramètres de nettoyage sur l’ensemble des données avant séparation peut introduire une **fuite d’information**. La bonne pratique consiste à :

* séparer Train/Test,
* apprendre les paramètres sur le Train,
* appliquer au Test.

### 4.4 Normalisation des variables

Une normalisation Min-Max est appliquée afin d’homogénéiser les échelles et d’éviter qu’une variable domine artificiellement les analyses.

---

## 5. Analyse Exploratoire des Données (EDA)

### 5.1 Statistiques descriptives

Les statistiques de base (moyenne, médiane, écart-type) permettent de dresser le **profil global** du stress et de ses déterminants.

### 5.2 Distribution et asymétrie

La comparaison moyenne/médiane renseigne sur l’asymétrie. Une distribution fortement biaisée signale des périodes critiques ou des valeurs extrêmes.

### 5.3 Corrélations et redondances

Les matrices de corrélation identifient les relations fortes (ex. stress–workload) et les redondances potentielles entre variables explicatives.

---

## 6. Méthodologie Expérimentale : Split & Généralisation

La séparation Train/Test garantit la **capacité de généralisation**. Le paramètre `random_state` assure la reproductibilité scientifique des résultats.

---

## 7. Focus Théorique : Modélisation ML (Random Forest)

### 7.1 Faiblesse d’un modèle isolé

Un modèle unique peut sur-apprendre le bruit et présenter une variance élevée.

### 7.2 Bagging et diversité

Le Random Forest introduit une diversité par **bootstrapping** des observations et **sélection aléatoire des variables**, réduisant la variance globale.

### 7.3 Consensus et robustesse

Les prédictions finales résultent d’un **vote majoritaire**, ce qui annule une partie des erreurs individuelles.

---

## 8. Évaluation et Audit de Performance

### 8.1 Matrice de confusion et erreurs

La matrice de confusion permet d’analyser les erreurs de prédiction et de distinguer faux positifs et faux négatifs selon les scénarios de stress.

### 8.2 Métriques clés et priorisation

Au-delà de l’accuracy, des métriques comme le **recall** sont privilégiées pour ne pas manquer des situations de stress élevé.

---

## 9. Visualisations et Interprétation

Les visualisations constituent une étape clé du projet **StressLess**, car elles permettent de transformer des résultats analytiques en **enseignements exploitables pour la prise de décision**. Elles facilitent l’identification des tendances, des relations entre variables et des facteurs prioritaires de stress.

### 9.1 Évolution temporelle du niveau de stress

<img src="Courbe d’évolution du stress.png" style="height:464px;margin-right:432px"/>  

Ce graphique illustre l’évolution du niveau de stress sur une période d’un mois. On observe une **tendance globale à la baisse**, avec un passage progressif d’un niveau élevé (environ 8,0) vers un niveau plus modéré (autour de 4,7). Cette dynamique suggère une amélioration graduelle de la situation, sans variations brusques ni pics critiques. La relative régularité de la trajectoire indique une évolution maîtrisée et cohérente dans le temps, compatible avec une meilleure gestion du quotidien.

### 9.2 Analyse des relations : matrice de corrélation

<img src="matrice de corrélation.png" style="height:464px;margin-right:432px"/>

La matrice de corrélation met en évidence les liens entre les **facteurs de stress**, les **recommandations générées** et le **score global de stress**. Le score global présente une corrélation forte avec les recommandations (≈ 0,93), ce qui indique que ces dernières jouent un rôle central dans la synthèse de l’information. La corrélation plus modérée avec les facteurs initiaux (≈ 0,69) suggère que les recommandations agissent comme un mécanisme d’agrégation et d’amplification des signaux issus des variables explicatives.

### 9.3 Priorisation des facteurs : diagramme de Pareto

<img src="Diagramme de Pareto.png" style="height:464px;margin-right:432px"/>  

Le diagramme de Pareto permet d’identifier les facteurs de stress ayant l’impact le plus significatif. Il ressort que la **charge de travail** concentre à elle seule une part importante de la sévérité cumulée, atteignant rapidement le seuil des 80 %. Cette observation confirme l’intérêt d’une approche de priorisation : agir sur un nombre restreint de facteurs clés peut produire des effets significatifs sur le niveau global de stress.

### 9.4 Analyse croisée : heatmap activité / stress

<img src="heatmap.png" style="height:464px;margin-right:432px"/>  

La heatmap offre une lecture croisée de l’impact des différents scénarios sur les scores des facteurs, des recommandations et le score global. Les dimensions liées à l’**environnement de travail** et à l’**équilibre vie professionnelle / vie personnelle** apparaissent comme les plus influentes sur les recommandations et le score final. À l’inverse, l’**anxiété de performance** présente un impact négligeable dans ce jeu de données, ce qui peut indiquer soit une faible variabilité observée, soit un effet limité dans le modèle actuel.

Dans leur ensemble, ces visualisations renforcent la compréhension des mécanismes du stress et constituent un **outil d’aide à la décision** pour orienter des actions préventives ciblées.

---

## 10. Conclusion et Perspectives

Le projet **StressLess** démontre qu’un projet Data Science est une **chaîne cohérente de décisions** reliant compréhension métier, préparation des données, choix méthodologiques et interprétation des résultats. Il ouvre la voie à des tableaux de bord avancés, des systèmes de recommandation personnalisés et des assistants intelligents de bien-être.
