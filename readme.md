
# 🧠 Projet StressLess v1 2025Q1 — Assistant IA de gestion du stress

Ce projet implémente un assistant IA, **StressLess**, conçu pour analyser des situations de stress professionnel décrites en langage naturel, identifier les facteurs de stress, estimer un niveau global de stress et proposer des recommandations priorisées et actionnables.

***

## 1. Contexte métier et mission

### 1.1 Problématique

Dans les environnements de travail modernes, de nombreux facteurs contribuent au stress :

- délais serrés,
- conflits relationnels,
- surcharge de travail,
- environnement bruyant,
- déséquilibre vie professionnelle / personnelle.

Ces facteurs peuvent mener à de la fatigue mentale, une baisse de performance, des erreurs répétées et, à terme, au burn-out.

### 1.2 Objectifs du projet

L’objectif de **StressLess** est de :

- transformer une description textuelle d’une situation de stress en **analyse structurée** (facteurs, symptômes, type de stress, niveau global) ;
- proposer des **recommandations personnalisées** triées par priorité et difficulté ;
- fournir un **système d’évaluation** quantitatif basé sur des scénarios de test annotés ;
- offrir un **tableau de bord visuel** permettant d’interpréter facilement les résultats.

***

## 2. Données et scénarios de test

### 2.1 Scénarios de test

Le projet n’utilise pas un dataset tabulaire classique, mais une série de **scénarios de test** définis manuellement.
Chaque scénario contient :

- `name` : nom du scénario (ex. « Stress lié aux délais ») ;
- `query` : description textuelle de la situation de stress ;
- `expected_factors` : liste des facteurs de stress attendus ;
- `expected_recommendations` : liste des recommandations attendues.

Exemples de scénarios (non exhaustif) :

- Stress lié aux délais (charge de travail, gestion du temps)
- Conflit au travail (relations, communication)
- Équilibre vie pro/perso (surcharge, épuisement)
- Anxiété de performance
- Environnement de travail bruyant


### 2.2 Sortie structurée de l’IA

Pour chaque `query`, la fonction `generate_structured_analysis(query)` renvoie une structure de type dictionnaire incluant :

- `stress_factors` : liste de facteurs avec sévérité, et parfois catégorie ;
- `symptoms` : symptômes avec intensité et type (physique, émotionnel, cognitif, comportemental) ;
- `overall_stress_level` : score global de stress (0–10) ;
- `stress_type` : type de stress (ex. aigu, chronique) ;
- `recommendations` : liste de recommandations avec difficulté et horizon temporel (`immediate`, `short_term`, `long_term`).

***

## 3. Architecture du code et pipeline IA

### 3.1 Bibliothèques principales

Le projet combine :

- **Data \& viz** : `pandas`, `numpy`, `matplotlib`, `seaborn` ;
- **IA générative** : Google Gemini (modèle `gemini-2.0-flash`) ;
- **RAG** : embeddings, index vectoriel (FAISS) ;
- **Interface / utils** : widgets, affichage HTML, etc.


### 3.2 Pipeline d’évaluation

Pour chaque scénario :

1. Appel de `generate_structured_analysis(query)` ;
2. Calcul d’un **score de pertinence des facteurs** via `evaluate_factors_relevance` :
    - proportion de facteurs attendus effectivement détectés ;
3. Calcul d’un **score de pertinence des recommandations** via `evaluate_recommendations_relevance` :
    - proportion de recommandations attendues effectivement proposées ;
4. Calcul d’un **score global** : moyenne des deux scores ;
5. Stockage des résultats dans une liste puis dans un DataFrame `results_df`.

Le DataFrame `results_df` contient :

- `scenario`
- `factors_score`
- `recommendations_score`
- `overall_score`

***

## 4. Système RAG et Function Calling

### 4.1 Base de connaissances (RAG)

Une petite base de connaissances métier est construite dans un DataFrame `stress_df`, comprenant des fiches structurées sur :

- respiration profonde,
- technique Pomodoro,
- méditation de pleine conscience,
- exercice physique,
- restructuration cognitive,
- établissement de limites,
- journal de gratitude,
- analyse des facteurs de stress,
- techniques de visualisation,
- communication assertive.

Ces textes sont :

1. découpés en chunks ;
2. encodés en vecteurs (embeddings) ;
3. indexés dans un moteur de recherche vectoriel (FAISS).

La fonction RAG :

- récupère les documents les plus pertinents par similarité ;
- injecte ce contexte dans un prompt ;
- appelle le modèle génératif pour produire une réponse ancrée dans ces connaissances.


### 4.2 Function Calling simplifié

Un routage simple basé sur des mots-clés est utilisé pour déclencher :

- `breathing_exercise(...)` pour les requêtes liées à la respiration/anxiété ;
- `schedule_break(...)` pour les requêtes sur les pauses/relaxation ;
- `analyze_work_habits(...)` pour l’analyse des habitudes de travail ;
- sinon, une réponse générée directement par le modèle.

Le résultat de la fonction appelée est ensuite reformaté en texte explicatif et empathique.

***

## 5. Visualisations d’évaluation (niveau modèle)

Cette section décrit les graphiques que tu peux exporter en `.png` et inclure dans le README :

### 5.1 Histogramme de la distribution des scores globaux

**But :** Visualiser la distribution de `overall_score` sur l’ensemble des scénarios.

Code (à exécuter dans ton notebook/script) :

```python
plt.figure(figsize=(8, 5))
sns.histplot(results_df['overall_score'], kde=True, bins=5, color='skyblue')
plt.title('Distribution des Scores Globaux (Overall Score)')
plt.xlabel('Overall Score')
plt.ylabel('Fréquence')
plt.tight_layout()
plt.savefig('figures/overall_score_distribution.png', dpi=300)
plt.show()
```

Dans ton README :

```markdown
![Distribution des scores globaux](figures/overall_score_distribution.png)
```


### 5.2 Barres groupées par scénario

**But :** Comparer, pour chaque scénario, la pertinence des facteurs, des recommandations et le score global.

```python
sns.set(style="whitegrid")
x = np.arange(len(results_df))
width = 0.25

fig, ax = plt.subplots(figsize=(12, 6))
rects1 = ax.bar(x - width, results_df["factors_score"], width, label="Score des facteurs")
rects2 = ax.bar(x, results_df["recommendations_score"], width, label="Score des recommandations")
rects3 = ax.bar(x + width, results_df["overall_score"], width, label="Score global")

ax.set_ylabel("Score (0-1)")
ax.set_title("Évaluation de StressLess sur différents scénarios")
ax.set_xticks(x)
ax.set_xticklabels(results_df["scenario"], rotation=45, ha="right")
ax.legend()

def autolabel(rects):
    for rect in rects:
        height = rect.get_height()
        ax.annotate(f"{height:.2f}",
                    xy=(rect.get_x() + rect.get_width() / 2, height),
                    xytext=(0, 3),
                    textcoords="offset points",
                    ha="center", va="bottom")

autolabel(rects1)
autolabel(rects2)
autolabel(rects3)

fig.tight_layout()
plt.savefig('figures/scenario_scores_grouped_bar.png', dpi=300)
plt.show()
```

Dans ton README :

```markdown
![Scores par scénario](figures/scenario_scores_grouped_bar.png)
```


### 5.3 Boxplots des différents scores

**But :** Visualiser la variance et la distribution de `factors_score`, `recommendations_score` et `overall_score`.

```python
plt.figure(figsize=(10, 6))
sns.boxplot(data=results_df[['factors_score', 'recommendations_score', 'overall_score']],
            palette='pastel')
plt.title('Distribution des Scores par Type (Boxplots)')
plt.ylabel('Score (0-1)')
plt.tight_layout()
plt.savefig('figures/scores_boxplots.png', dpi=300)
plt.show()
```

Dans le README :

```markdown
![Boxplots des scores](figures/scores_boxplots.png)
```


### 5.4 Scatter plot : facteurs vs recommandations

**But :** Étudier la corrélation entre `factors_score` et `recommendations_score` par scénario.

```python
plt.figure(figsize=(8, 6))
sns.scatterplot(x='factors_score',
                y='recommendations_score',
                hue='scenario',
                data=results_df,
                s=100)
plt.title('Relation entre Score des Facteurs et Score des Recommandations')
plt.xlabel('Score des Facteurs')
plt.ylabel('Score des Recommandations')
plt.legend(bbox_to_anchor=(1.05, 1), loc='upper left')
plt.grid(True, linestyle='--', alpha=0.7)
plt.tight_layout()
plt.savefig('figures/factors_vs_recommendations_scatter.png', dpi=300)
plt.show()
```

Dans le README :

```markdown
![Corrélation facteurs vs recommandations](figures/factors_vs_recommendations_scatter.png)
```


***

## 6. Visualisations d’analyse individuelle (tableau de bord StressLess)

Ici, tu peux réutiliser tes fonctions `visualize_stress_analysis` ou `visualize_stress_analysis_enhanced(analysis)` pour un exemple de cas utilisateur, puis les exporter.

### 6.1 Exemple : radar des facteurs de stress

Après avoir obtenu une `analysis` sur une requête réelle :

```python
analysis_example = generate_structured_analysis("Je suis débordé par les délais et les conflits au travail.")
visualize_stress_analysis_enhanced(analysis_example)  # ou une version spécifique pour le radar

plt.savefig('figures/stress_factors_radar.png', dpi=300, bbox_inches='tight')
plt.show()
```

Dans le README :

```markdown
![Profil des facteurs de stress](figures/stress_factors_radar.png)
```


### 6.2 Jauge du niveau de stress global

Tu peux isoler la partie jauge de ta fonction et sauvegarder l’image :

```python
# Après avoir tracé la jauge pour analysis_example
plt.savefig('figures/overall_stress_gauge.png', dpi=300, bbox_inches='tight')
plt.show()
```

README :

```markdown
![Niveau de stress global](figures/overall_stress_gauge.png)
```


### 6.3 Symptômes par catégorie et intensité

Même principe : après le tracé des barres horizontales des symptômes, sauver la figure :

```python
plt.savefig('figures/symptoms_by_category.png', dpi=300, bbox_inches='tight')
plt.show()
```

README :

```markdown
![Symptômes par catégorie et intensité](figures/symptoms_by_category.png)
```


### 6.4 Recommandations priorisées

```python
plt.savefig('figures/prioritized_recommendations.png', dpi=300, bbox_inches='tight')
plt.show()
```

README :

```markdown
![Recommandations priorisées](figures/prioritized_recommendations.png)
```


***

## 7. Utilisation dans GitHub

### 7.1 Organisation des fichiers

Une structure possible :

```text
StressLess/
├─ notebooks/
├─ src/
├─ figures/
│  ├─ overall_score_distribution.png
│  ├─ scenario_scores_grouped_bar.png
│  ├─ scores_boxplots.png
│  ├─ factors_vs_recommendations_scatter.png
│  ├─ stress_factors_radar.png
│  ├─ overall_stress_gauge.png
│  ├─ symptoms_by_category.png
│  └─ prioritized_recommendations.png
└─ README.md
```


### 7.2 Intégration dans le README GitHub

Le rapport que tu viens de lire peut servir directement de **README.md**.
Il te suffit de :

- générer les images avec le code de sauvegarde `plt.savefig(...)` ;
- créer le dossier `figures/` ;
- committer le tout sur GitHub.

***

## 8. Perspectives et améliorations

- Ajouter un suivi temporel du stress par utilisateur (séries temporelles + graphiques de tendance).
- Exposer StressLess via une API (FastAPI) et une UI (Streamlit ou autre).
- Étendre le corpus RAG avec des sources validées (psychologie du travail, ergonomie).
- Affiner les métriques d’évaluation (par type de scénario, par type de recommandation, etc.).
