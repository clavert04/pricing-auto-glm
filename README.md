# Tarification Auto par Modèle Linéaire Généralisé (GLM)

## Objectif
Construction d'un modèle de tarification non-vie (assurance automobile) à partir du jeu de données `BASEAUTO` (10 496 polices), visant à estimer la fréquence et le coût des sinistres pour un portefeuille de polices auto, dans une logique de pricing actuariel.

## Méthodologie
- Analyse exploratoire univariée et bivariée, regroupement de modalités (CSP, classe SRA, vitesse, type de véhicule)
- Séparation sinistres attritionnels / graves (seuil au 99e percentile de la charge)
- Ajustement de loi sur les sinistres attritionnels (comparaison Gamma / Log-normale / Weibull par AIC/BIC, test de Kolmogorov-Smirnov)
- **Modélisation de la fréquence** des sinistres via une régression Binomiale (GLM, lien logit) avec sélection pas-à-pas (stepAIC)
- **Modélisation du coût moyen** des sinistres via une régression Gamma (GLM, lien log)
- Validation croisée train/test (80/20)

## Résultats clés
- Taux de sinistralité du portefeuille : **13,3 %**
- 1 384 sinistres attritionnels retenus pour la construction du tarif (14 sinistres graves isolés, traités hors modèle via chargement/réassurance)
- **Fréquence** : les variables les plus discriminantes sont l'ancienneté du véhicule (ACV), l'usage, le type de garage, la classe SRA, le sexe et le bonus-malus (RM) — modèle bien calibré, écart prédit/observé < 0,1 %
- **Coût moyen** : le bonus-malus (RM) est la variable la plus significative sur le coût des sinistres
- Prime pure calculée = Fréquence prédite × Coût moyen prédit, restituée sous forme de grille tarifaire par profil de risque

## Outils utilisés
- R (dplyr, ggplot2, MASS, corrplot) via Quarto
- Rapport reproductible généré en PDF/HTML

## Structure du dépôt
```
├── LAST_GLM.qmd        # Analyse complète et modélisation (Quarto)
├── BASEAUTO.csv         # Jeu de données
└── README.md
```

## Auteur
Tchio Clavert — Master 2 Actuariat, ESSFAR Cameroun
