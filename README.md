# Automatisation et ingénierie de données Fitness

## Contexte
Étude de la fréquentation d'une chaîne de salles de sport à partir de quatre fichiers CSV fictifs :
nettoyage, contrôle de qualité, enrichissement, indicateurs et export automatisé des rapports.

Le travail est réalisé dans le notebook `Analyse_Fitness.ipynb` (Python, pandas).
Une démo en ligne rejoue le même traitement : voir le dépôt Hugging Face du projet.

## Volume des données
| Table | Lignes |
|---|---|
| centers | 33 |
| members | 8 500 |
| subscriptions | 11 073 |
| visits | 209 976 |

## Anomalies détectées
| Anomalie | Nombre | Traitement |
|---|---|---|
| Dates de naissance impossibles (1870 et 2026) | 2 | signalées |
| Membres de moins de 16 ans | 2 | signalés (l'un des deux est la naissance en 2026) |
| Abonnement avec fin avant début | 1 | supprimé |
| Visite avec sortie avant entrée | 1 | supprimée |
| Visites orphelines (membre inexistant) | 3 | supprimées |
| Codes postaux invalides (750012, ABC12, 1234) | 3 | signalés |
| Prix différent de celui de la formule (0,00 / -29,99 / 9 999,99) | 3 | mis à vide |
| Mode de paiement manquant | 2 197 (19,84 %) | conservés |

## Points techniques
- **Dates** : les fichiers mélangent `JJ/MM/AAAA` et `AAAA-MM-JJ`. Chaque format est lu séparément ;
  une lecture automatique unique inverse le jour et le mois et supprime à tort des centaines de lignes.
- **Codes postaux** : lus comme du texte, sinon `06000` devient `6000` et paraît invalide.
- **Date de référence** : la dernière visite des données (15/12/2024), pas la date du jour.
- **Jointures** : un membre avec plusieurs abonnements dupliquerait ses visites. On garde l'abonnement
  actif à la date de la visite. Chaque visite apparaît une seule fois (contrôlé par une assertion).
- **Prix** : le prix est fixe par formule (29,99 / 49,99 / 79,99 pour les formules mensuelles,
  299,99 / 499,99 pour les annuelles). Tout autre prix est une anomalie.

## Indicateurs calculés
**Par membre** : nombre de visites, durée moyenne, centre favori, dernière visite, jours depuis la dernière visite.

**Par centre** : membres uniques, nombre de visites, visites par jour, heure de pointe, taux d'occupation.

## Export
- `fitness_data_clean.csv` : table complète enrichie (210 129 lignes, 46 colonnes)
- `fitness_analysis.xlsx` : 5 onglets (Visits_Clean, Members_KPIs, Centers_Performance, Members_Clean, Centers_Clean)

La création du fichier Excel prend environ 2 minutes à cause de l'onglet de visites.

## Limites
- `total_paid` = prix x durée / 30. Pour les formules annuelles, le prix est annuel : ce total est donc
  surestimé pour elles. À corriger si l'on veut un chiffre d'affaires exact.
- Le taux d'occupation compare les visites à la surface du centre : c'est une hypothèse de calcul,
  pas une capacité d'accueil réelle.
- Données fictives : la méthode est démontrée, pas des résultats métier.

## Stack technique
Python | pandas | NumPy | Streamlit | XlsxWriter

Dernière mise à jour : 02/11/2026
