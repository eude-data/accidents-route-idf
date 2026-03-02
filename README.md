# 🚗 Gravité des accidents de la route en Île-de-France

> Analyse discriminante · Random Forest · Régression logistique  
> Cours *Apprentissage Statistique* - M2 ISIFAR · Université Paris Cité  
> En collaboration avec Chloé & Shanice · Novembre 2025

---

## Vue d'ensemble

Analyse de **plus de 15 000 accidents corporels** en Île-de-France à partir des données BAAC 2024 (data.gouv.fr). Objectif : identifier les facteurs qui influencent la **gravité d'un accident** et construire un modèle capable de discriminer accidents graves et non graves.

**Problématique** : Quels facteurs influencent la gravité d'un accident de la route, et peut-on discriminer les accidents graves des non graves à partir de ces variables ?

---

## Données

- **Source** : [Bases annuelles des accidents corporels - data.gouv.fr](https://www.data.gouv.fr/datasets/bases-de-donnees-annuelles-des-accidents-corporels-de-la-circulation-routiere-annees-de-2005-a-2024/)
- **Périmètre** : Île-de-France · Année 2024
- **Variable cible** : `grav_bin` - binaire construite à partir de `grav`
  - `0` : Indemne ou Blessé léger
  - `1` : Blessé hospitalisé ou Tué (~9% des cas)
- **Déséquilibre de classes** : 91% non graves vs 9% graves

---

## Méthodologie

### 1. Analyse exploratoire & sélection de variables

**ACM (Analyse des Correspondances Multiples)** pour identifier les variables les plus contributives aux deux axes principaux (4.5% de variance expliquée au total, ce qui est attendu avec de nombreuses modalités).

**Variables retenues** : `agg`, `catr`, `lum`, `obsm`, `obsf`, `catu`, `int`, `col`, `grav`, `manv`, `obs`, `choc`, `vma`, `surf`, `age`, `mois`, `nbv`, `circ`, `prof`, `secu`, `sexe`

Région ciblée : **Île-de-France** (plus grand nombre d'accidents). Département avec le plus d'accidents : Paris (75).

---

### 2. Modèles comparés

#### a) LDA - Analyse Discriminante Linéaire

- **AUC : 0.74** · Taux de bonne classification : 92.2%
- Détecte bien les accidents non graves (2703/2728 corrects)
- Détecte très mal les accidents graves (21/225, soit 9.3%)
- Variables discriminantes clés : `vma`, `surf`, `catr`, `lum`, `manv`, `secu`

#### b) Random Forest

- **AUC : 0.72** · Taux de bonne classification : 92.45%
- Top variables (Mean Decrease Gini) : `age`, `mois`, `manv`, `nbv`, `vma`, `col`
- Même constat : accident grave très difficile à détecter (7.1% recall)

#### c) Régression logistique *(modèle le plus performant)*

- **AUC : 0.74** · Taux de bonne classification : 92.52%
- Meilleur équilibre précision / interprétabilité
- Facteurs augmentant le risque d'accident grave :
  - **Vitesse** : à partir de 80 km/h, risque multiplié par 4 à 6 vs zone 50
  - **Catégorie de route** : routes communales et départementales plus dangereuses
  - **Luminosité** : risque 1.6× plus élevé la nuit / crépuscule
  - **Âge** : jeunes conducteurs et personnes âgées plus à risque
  - **Manœuvres** : dépassements, changements de file

---

## Résultats

| Modèle | AUC | Accuracy | Recall accidents graves |
|--------|-----|----------|------------------------|
| LDA | 0.74 | 92.2% | 9.3% |
| Random Forest | 0.72 | 92.45% | 7.1% |
| Régression logistique | 0.74 | 92.52% | ~5% |

**Conclusion** : Les trois modèles souffrent du fort déséquilibre de classes. La régression logistique reste la plus performante en termes d'équilibre précision / interprétabilité. L'hypothèse H3 (Random Forest > approches linéaires) est **infirmée**.

---

## Structure du projet

```
accidents-route-idf/
├── Eude_Géro_Chloé_Shanice_Projet.pdf          # Rapport final
├── Eude_Géro_Chloé_Shanice_Annexe_code_R_.rmd  # Code R commenté
└── README.md
```

---

## Stack technique

`R` · `FactoMineR` · `factoextra` · `randomForest` · `MASS` · `pROC` · `ggplot2` · `dplyr`

---

## Auteurs

**Eude Géro Dekadjevi**, Chloé, Shanice · M2 ISIFAR · Université Paris Cité · 2025  
[LinkedIn](https://www.linkedin.com/in/eude-gero-dekadjevi-56924722a) · [GitHub](https://github.com/eude-data)
