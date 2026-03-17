# Prédiction de la moyenne générale des étudiants

Analyse et prédiction de la moyenne générale (sur 20) à partir d'indicateurs
de mode de vie et de bien-être, sur un jeu de données de 100 000 étudiants simulés.
Réalisé en R dans le cadre du cours de *Data Mining et Économie* (Master 1 Data Science).

## Jeu de données

11 variables couvrant des facteurs démographiques, comportementaux et psychologiques :

| Variable | Type | Description |
|---|---|---|
| Age | Numérique | 18–24 ans |
| Genre | Facteur | Femelle / Male |
| Departement | Facteur | Arts, Commerce, Ingénierie, Médecine, Science |
| Sommeil_heures_par_jour | Numérique | Heures de sommeil par jour |
| Etudes_heures_par_jour | Numérique | Heures de travail par jour |
| Reseaux_sociaux_heures_par_jour | Numérique | Heures sur les réseaux sociaux par jour |
| Activite_physique_minutes_par_semaine | Numérique | Activité physique (min/semaine) |
| Stress_niveau_sur_10 | Numérique | Niveau de stress (2–10) |
| Depression_presente | Facteur | TRUE / FALSE |
| Moyenne_generale_sur_20 | **Cible** | Moyenne générale sur 20 |

## Méthodes

1. **Analyse exploratoire (EDA)** — statistiques descriptives, histogrammes,
   boxplots, matrice de corrélation (`corrplot`)
2. **Tests de comparaison de groupes** — tests de Welch par genre et dépression,
   ANOVA à un facteur par département
3. **Régression linéaire multiple (MCO)** — modèle complet + validation des
   hypothèses (Shapiro-Wilk, Breusch-Pagan, Durbin-Watson, VIF)
4. **Sélection de variables** — stepwise AIC (`MASS::stepAIC`),
   meilleurs sous-ensembles (`leaps`, R²aj, Cp, BIC)
5. **ACP** — sur données standardisées encodées en indicatrices
   (`prcomp`, `factoextra`)
6. **PCR** — Régression sur Composantes Principales avec CV 10 plis (`pls::pcr`)
7. **PLS** — Moindres Carrés Partiels avec CV 10 plis (`pls::plsr`)
8. **Bootstrap** — intervalles de confiance percentile à 100 réplications
   sur les coefficients des quatre modèles (`boot`)
9. **Comparaison des modèles** — RMSE, R², MAE, MAPE, analyse des résidus
   sur un jeu de test retenu à 20 %

## Principaux résultats

| Modèle | RMSE | R² | MAE | Composantes / Variables |
|---|---|---|---|---|
| MCO complet | 2,593 | 0,0488 | 2,221 | 15 |
| Stepwise | 2,593 | 0,0487 | 2,221 | 4 |
| PCR | 2,593 | 0,0487 | 2,221 | 11 |
| **PLS** | **2,593** | **0,0488** | **2,221** | **3** |

Les trois prédicteurs significatifs communs à tous les modèles :
- `Etudes_heures_par_jour` (effet positif)
- `Reseaux_sociaux_heures_par_jour` (effet négatif)
- `Depression_presente` (−1,55 point en moyenne)

Le faible R² (~5 %) s'explique par la construction du jeu de données :
la plupart des variables sont indépendantes par construction,
ce qui limite la variance explicable.

## Prérequis
```r
install.packages(c(
  "readxl", "corrplot", "lmtest", "car",
  "MASS", "leaps", "caret", "pls",
  "factoextra", "boot", "Metrics"
))
```

## Utilisation

1. Placer le fichier `moyennes_etudiants.xlsx` dans le répertoire de travail
2. Mettre à jour le chemin d'accès en section 1 du script
3. Exécuter `analyse_moyennes.R` de bout en bout
