# Automatisation et Ingénierie de Données Fitness — HealthTrack

## Contexte
Étude statistique de la fréquentation d'une chaîne de salles de sport
et automatisation de l'export des rapports d'analyse.

## Volume des Données
| Table | Lignes |
|---|---|
| centers | 33 |
| members | 8 500 |
| subscriptions | 11 073 |
| visits | 209 976 |

## Anomalies Détectées
- 2 dates de naissance invalides (1870, 2026)
- 2 membres mineurs (< 16 ans)
- 1 abonnement avec end_date < start_date
- 1 visite avec checkout < checkin
- 3 visites orphelines (member_id inexistant) → supprimées
- 2 197 méthodes de paiement manquantes (19,84%)
- 2 codes postaux invalides

## KPIs Calculés
**Par membre :** total visites, durée moyenne, centre favori,
date dernière visite, jours d'inactivité

**Par centre :** membres uniques reçus, moyenne visites/jour,
heure de pointe, taux d'occupation

## Automatisation de l'Export
Export automatisé en fin de pipeline vers :
- `fitness_data_clean.csv` — dataset complet enrichi (273 420 lignes)
- `fitness_analysis.xlsx` — 5 onglets :
  Visits_Clean | Members_KPIs | Centers_Performance |
  Members_Clean | Centers_Clean

## Stack Technique
Python | Pandas | NumPy | Matplotlib | Openpyxl
