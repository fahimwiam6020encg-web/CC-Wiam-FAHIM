# 📘 Rapport d’Analyse – Détection de Fraude

## 1. Introduction

La détection de fraude représente aujourd’hui un enjeu stratégique pour les institutions financières, les plateformes de paiement et les services en ligne. Les transactions frauduleuses, bien que minoritaires, entraînent des pertes économiques considérables et nécessitent des systèmes robustes et automatisés capables de les identifier efficacement.


---

## 2. Analyse et Développement

### 2.1 Analyse des données

#### 2.1.1 Déséquilibre de la variable cible

L'exploration initiale montre un **déséquilibre important** entre les transactions légitimes et frauduleuses.  
Les fraudes constituent une **faible minorité**, ce qui rend le problème difficile à traiter :

- Un modèle naïf pourrait prédire "non frauduleux" pour toutes les transactions et obtenir une *accuracy élevée*.
- Les métriques comme l’**accuracy** deviennent trompeuses dans ce contexte.

➡️ Il est donc nécessaire de compléter l’analyse avec des mesures comme la **precision**, le **recall**, le **F1-score** et la **matrice de confusion**.

#### 2.1.2 Corrélation entre les variables

La heatmap de corrélation met en évidence :

- Des relations modérées entre certaines caractéristiques et la variable cible.
- Des corrélations fortes entre certaines variables explicatives, pouvant introduire de la redondance.
- Une nécessité possible de réduire ou transformer les variables fortement corrélées.

L'ajout des variables temporelles (*mois*, *année*) améliore la compréhension des comportements inhabituels liés à la fraude.

#### 2.1.3 Prétraitement et création de nouvelles variables

Le notebook inclut :
- une extraction d'informations depuis les dates,  
- un nettoyage basique des données,  
- une préparation des features pour la modélisation.

L’enrichissement des données contribue à améliorer la capacité du modèle à détecter des anomalies.

---

### 2.2 Modélisation : Random Forest

Le modèle utilisé est un **Random Forest Classifier**, pertinent pour ce type de problème en raison de sa robustesse et de sa capacité à gérer :

- des données numériques et catégorielles (après encodage),
- des relations non linéaires,
- une importance variable des caractéristiques.

Le dataset est divisé selon un ratio :
- **70%** pour l’entraînement,
- **30%** pour le test.

Aucun tuning avancé n’est appliqué dans le notebook initial, ce qui fait du modèle une bonne base de référence.

---

### 2.3 Résultats obtenus et interprétation

#### 2.3.1 Accuracy

Bien que l’**accuracy soit élevée**, elle est insuffisante pour évaluer réellement la performance du modèle en raison du déséquilibre du dataset.

➡️ L’accuracy seule **ne reflète pas** la capacité à détecter les fraudes.

#### 2.3.2 Matrice de confusion

La matrice de confusion révèle :

- **TN (vrais négatifs)** élevés → le modèle identifie correctement les transactions normales.
- **TP (vrais positifs)** présents → le modèle détecte une partie des fraudes.
- **FP (faux positifs)** modérés → certains comportements légitimes sont signalés par erreur.
- **FN (faux négatifs)** encore trop nombreux → certaines fraudes ne sont pas captées.

➡️ Les **faux négatifs** constituent le principal problème car ils représentent des fraudes réelles non détectées.

#### 2.3.3 Interprétation avancée

- Le **recall** (capacité à détecter les fraudes) n’est pas maximal.
- La **precision** est correcte : les fraudes identifiées sont bien détectées.
- Le **F1-score** indique un compromis moyen entre precision et recall.

L’analyse montre que le modèle a une **bonne base**, mais qu’il nécessite des améliorations pour réduire les faux négatifs.

#### 2.3.4 Importance des caractéristiques

Le Random Forest met en avant plusieurs variables clés :

- montant de la transaction,  
- fréquence des opérations,  
- caractéristiques temporelles,  
- certains profils clients.

Cette importance des features contribue à expliquer les décisions du modèle et peut orienter les futures optimisations.

---<img width="652" height="540" alt="téléchargement (1)" src="https://github.com/user-attachments/assets/28c9c07d-ae53-48db-a591-c164db695bb1" />


## 3. Limites du modèle actuel

Plusieurs limites ressortent de l’analyse :

1. **Déséquilibre marqué des classes** → fausses non-détections.
2. **Absence de tuning** des hyperparamètres du modèle.
3. **Métriques limitées** dans la version initiale du notebook.
4. **Un seul modèle testé**, pas de comparaison avec d’autres algorithmes performants (XGBoost, LightGBM).
5. **Pas de traitement avancé du déséquilibre** (SMOTE, class_weight).

Ces limites expliquent la performance imparfaite, surtout pour la classe minoritaire.

---<img width="887" height="540" alt="téléchargement" src="https://github.com/user-attachments/assets/24030eb6-b42f-4450-8b5c-27dd0cf05029" />


## 4. Conclusion

Le modèle développé dans le notebook constitue une **première version solide** pour une détection automatique de fraude.  
Il parvient à :

- bien classifier les transactions normales,
- détecter une partie significative des fraudes,
- exploiter efficacement les caractéristiques disponibles.

Cependant, des améliorations sont nécessaires pour obtenir des résultats professionnels :

- gestion du déséquilibre via SMOTE ou *class_weight*,
- optimisation des hyperpa
