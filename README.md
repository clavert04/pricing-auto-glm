# Tarification automobile par GLM (fréquence × coût)

![R](https://img.shields.io/badge/R-276DC3?logo=r&logoColor=white) ![Quarto](https://img.shields.io/badge/Quarto-39729E?logo=quarto&logoColor=white)

📄 **[Lire le rapport complet (PDF)](tarification_auto_glm.pdf)** · Version HTML interactive : `tarification_auto_glm.html`

## Objectif

Construire un tarif d'assurance automobile (non-vie) à partir du jeu de données pédagogique **BASEAUTO** (10 496 polices, 17 variables) : estimer la probabilité de sinistre et le coût moyen des sinistres, puis en déduire une **prime pure par profil de risque**.

## Méthodologie

1. **Analyse exploratoire** univariée et bivariée, regroupement de modalités (CSP, classe SRA, vitesse, type de véhicule).
2. **Séparation sinistres attritionnels / graves** au 99ᵉ percentile de la charge (≈ 17 500 €).
3. **Ajustement de loi** sur les sinistres attritionnels : Gamma, log-normale et Weibull estimées par maximum de vraisemblance, comparées par AIC/BIC ; test de Kolmogorov-Smirnov avec p-values par bootstrap paramétrique.
4. **GLM fréquence** : régression binomiale (lien logit), sélection pas à pas par AIC (`stepAIC`).
5. **GLM coût** : régression Gamma (lien log), sélection par AIC.
6. **Validation** sur un échantillon test (80/20) : calage global, **AUC / Gini** et courbe de Lorenz.
7. **Tarif** : prime pure = fréquence prédite × coût moyen prédit, plus une surprime répartissant le coût des sinistres graves.

## Résultats clés

| Élément | Résultat |
|---|---|
| Taux de sinistralité du portefeuille | 13,3 % |
| Base de construction du tarif | 1 384 sinistres attritionnels (14 graves traités par surprime : ≈ 36 € par police) |
| Variables du GLM fréquence | classe SRA, vitesse, sexe, usage, kilométrage (K8000), bonus-malus (RM), ancienneté du permis, âge du véhicule |
| Pouvoir discriminant (test) | AUC = 0,73 · **Gini = 0,46** |
| Calage (test) | écart fréquence prédite / observée ≈ 4,6 % ; coût moyen ≈ 4,3 % |
| Variables du GLM coût | vitesse, statut, bonus-malus (RM) |
| Prime pure (profils de la grille) | d'environ 145 € à 255 € selon classe, vitesse et sexe |

**Limites identifiées** : la log-normale ajuste un peu mieux la charge que la Gamma (AIC), et le test KS rejette les deux lois. Le GLM Gamma est conservé comme hypothèse de travail standard. La base ne contient pas d'exposition, donc la fréquence est modélisée comme une probabilité (binomiale) et non comme un nombre de sinistres (Poisson). Pistes : GLM Tweedie, approche valeurs extrêmes pour le seuil des graves, validation croisée k-fold.

## Outils

R (`dplyr`, `ggplot2`, `MASS`, `corrplot`, `kableExtra`) et Quarto, pour un rapport reproductible en PDF et HTML.

## Reproduire

```bash
quarto render tarification_auto_glm.qmd   # produit le PDF et le HTML
```

## Structure du dépôt

```
├── tarification_auto_glm.qmd    # Analyse et modélisation (Quarto / R)
├── tarification_auto_glm.pdf    # Rapport rendu
├── tarification_auto_glm.html   # Rapport rendu (HTML autonome)
├── BASEAUTO.csv                 # Jeu de données
└── README.md
```

## Auteur

**Tchio Clavert**, Master 2 Actuariat, ESSFAR Cameroun · [LinkedIn](https://www.linkedin.com/in/clavert-tchio-4271a3297)

---

*English summary: auto insurance pricing on 10,496 policies using a frequency (binomial GLM) × severity (Gamma GLM) approach in R/Quarto, with large-loss capping at the 99th percentile, distribution fitting by MLE, and out-of-sample validation (Gini = 0.46).*
