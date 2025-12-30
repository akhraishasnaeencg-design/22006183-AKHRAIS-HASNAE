# 📘 Projet StressLess

## Analyse, visualisation et suivi du niveau de stress

**Préparé par : AKHRAIS Hasnae**

---
<img src="AKHRAIS HASNAE.jpg" style="height:464px;margin-right:432px"/>  
## 1. Le Contexte Métier et la Mission

### 1.1 Le Problème (Business Case)

Dans le contexte actuel, le stress chronique constitue un enjeu majeur de santé publique et de performance professionnelle. Une mauvaise gestion du stress peut entraîner :

* une baisse de productivité,
* des troubles du sommeil,
* des risques psychosociaux,
* une dégradation du bien-être général.

Cependant, le stress est une variable **subjective**, **évolutive dans le temps**, et influencée par plusieurs facteurs simultanés (activité, sommeil, charge de travail).

**Problématique centrale :**

> Comment analyser, suivre et anticiper l’évolution du niveau de stress d’un individu à partir de données temporelles et comportementales ?

### 1.2 Objectif du projet

Le projet **StressLess** vise à construire une chaîne complète d’analyse de données permettant :

* d’explorer les niveaux de stress observés,
* d’identifier les tendances temporelles,
* de comprendre les relations entre stress et facteurs explicatifs,
* de fournir des indicateurs exploitables par des professionnels du bien-être.

L’objectif n’est pas uniquement prédictif, mais également **descriptif et préventif**, afin d’anticiper les périodes de stress élevé.

---

## 2. Les Données (Input du Système)

Le projet repose sur un jeu de données structuré sous forme de **DataFrame pandas**, organisé selon une logique temporelle.

### 2.1 Description des variables

| Variable       | Description                                      |
| -------------- | ------------------------------------------------ |
| `date`         | Date ou horodatage de l’observation              |
| `stress_level` | Niveau de stress (échelle 0–100 ou 1–5)          |
| `activity`     | Niveau ou intensité d’activité                   |
| `sleep`        | Qualité ou durée du sommeil                      |
| `workload`     | Charge de travail                                |
| `user_id`      | Identifiant utilisateur (cas multi-utilisateurs) |

### 2.2 Nature des données

* Données **temporelles** (séries chronologiques),
* Données **quantitatives continues**,
* Données potentiellement **bruitées** ou **incomplètes**,
* Possibilité de **variations inter-individuelles**.

Ces caractéristiques rendent indispensable une phase rigoureuse de préparation et d’exploration.

---

## 3. Analyse Approfondie : Nettoyage et Préparation des Données (Data Wrangling)

### 3.1 Problématique des données manquantes

Les algorithmes statistiques et analytiques ne peuvent pas fonctionner correctement en présence de valeurs manquantes (`NaN`). Une valeur absente peut fausser :

* les moyennes,
* les corrélations,
* les visualisations temporelles.

### 3.2 Stratégie de nettoyage adoptée

Les étapes suivantes ont été appliquées :

* **Imputation des valeurs manquantes** par la moyenne ou la médiane,
* **Suppression des valeurs aberrantes**, notamment :

  * stress négatif,
  * stress supérieur au seuil maximal autorisé,
* **Uniformisation des formats temporels**.

```python
df['date'] = pd.to_datetime(df['date'])
df = df.sort_values('date')
```

### 3.3 Normalisation des variables

Afin d’homogénéiser les échelles et faciliter l’analyse comparative, une normalisation de type Min-Max a été appliquée :

```python
from sklearn.preprocessing import MinMaxScaler
df['stress_norm'] = MinMaxScaler().fit_transform(df[['stress_level']])
```

Cette étape permet d’éviter qu’une variable domine artificiellement les analyses.

---

## 4. Analyse Exploratoire des Données (EDA)

L’analyse exploratoire constitue une phase essentielle pour comprendre la structure interne des données avant toute interprétation avancée.

### 4.1 Distribution du niveau de stress

L’étude de la distribution du stress permet de répondre aux questions suivantes :

* Le stress est-il majoritairement modéré ou élevé ?
* Existe-t-il des valeurs extrêmes ?
* La distribution est-elle symétrique ou biaisée ?
  <img src="Diagramme de Pareto.png" style="height:464px;margin-right:432px"/>  
Un histogramme permet d’identifier les zones de concentration et les pics de stress.

---

### 4.2 Évolution temporelle du stress

L’analyse temporelle met en évidence :

* les tendances générales (hausse ou baisse),
* les cycles (journaliers, hebdomadaires),
* les périodes critiques.
<img src="Courbe d’évolution du stress.png" style="height:464px;margin-right:432px"/>  
La visualisation sous forme de courbe facilite l’interprétation de l’évolution du stress dans le temps et sa relation avec les événements quotidiens.

---

### 4.3 Analyse des corrélations

La matrice de corrélation permet d’identifier les relations entre :

* stress et sommeil,
* stress et activité,
* stress et charge de travail.
<img src="matrice de corrélation.png" style="height:464px;margin-right:432px"/>  
Elle aide à détecter les variables les plus influentes et à comprendre les mécanismes sous-jacents du stress.

---

### 4.4 Analyse des scénarios de test

Afin de tester la robustesse du système, plusieurs **scénarios simulés** ont été intégrés :

* surcharge de travail prolongée,
* repos et amélioration du sommeil,
* stress variable sur une courte période.
<img src="heatmap.png" style="height:464px;margin-right:432px"/>  
Ces scénarios permettent de vérifier la cohérence des indicateurs et la réaction du système face à des situations extrêmes.

---

## 5. Analyse Méthodologique : Indicateurs et Évaluation

### 5.1 Indicateurs clés de suivi

Le projet StressLess s’appuie sur plusieurs métriques :

* **Niveau moyen de stress**
* **Volatilité du stress**
* **Durée passée en zone de stress élevé**
* **Amplitude des variations journalières**

Ces indicateurs permettent un suivi précis et individualisé.

---

### 5.2 Fonctions d’évaluation

Le notebook intègre des fonctions dédiées à l’évaluation et à la visualisation :

```python
evaluate_stress_level()
visualize_stress_evolution()
visualize_correlation_matrix()
```

Chaque fonction est testée sur différents scénarios afin de garantir sa fiabilité.

---

## 6. Visualisations et Interprétation

Les visualisations prévues jouent un rôle central dans la compréhension des résultats :

* **Courbe d’évolution du stress**
  (date → stress_level)

* **Matrice de corrélation**
  entre sommeil, activité, charge de travail et stress

* **Diagramme de Pareto**
  pour identifier les principales sources de stress

* **Heatmap activité / stress**
  pour détecter des patterns comportementaux

Ces outils facilitent la prise de décision et l’analyse préventive.

---

## 7. Conclusion et Perspectives

Le projet **StressLess** met en place une chaîne complète et cohérente d’analyse de données :

* nettoyage rigoureux des données,
* exploration statistique approfondie,
* visualisation claire et interprétable,
* tests via scénarios simulés.

Il constitue une base solide pour le développement futur de :

* tableaux de bord de suivi du stress,
* systèmes de recommandation personnalisés,
* outils de détection précoce d’anomalies,
* assistants intelligents de bien-être.

Ce projet illustre que l’analyse de données ne se limite pas à des graphiques, mais repose sur une compréhension métier, méthodologique et analytique rigoureuse.

---


