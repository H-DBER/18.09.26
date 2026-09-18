# TP - Auditer un jeu de données open data - Helene Debrabandere

## 1. Lire la fiche du jeu

| Élément | Votre réponse |
|---|---|
| Titre du jeu | Prix des carburants en France - Flux instantané - v2 |
| Producteur (=diffuseur?) | Ministères économiques et financiers |
| URL | https://www.data.gouv.fr/api/1/datasets/r/edd67f5b-46d0-4663-9de9-e5db1c880160 |
| Licence | Licence Ouverte 2.0 |
| Date de dernière mise à jour | 18-sept-26 |
| Fréquence de mise à jour | toute les 10min |
| Couverture géographique et temporelle | Tous les points de vente de carburant en france, snapshot 11h50 |
| Format téléchargé | .csv |
| Dictionnaire des variables disponible ? (oui/non) | oui,  =Structure des données |

## 2. Ouvrir et décrire

| Élément | Votre réponse |
|---|---|
| Nombre de lignes | 9804 |
| Nombre de colonnes | 47 |


- Pour 5 colonnes au choix : nom, type de valeur (texte, nombre, date), exemple de valeur :

| Nom | Type de valeur | Exemple de valeur |
|---|---|---|
| id | nombre | 80570001 |
| Début rupture sp95 (si temporaire) | date | 2024-08-21T15:17:53+00:00 |
| rupture e85 | texte | definitive |
| Prix SP98 | nombre | 2.249 |
| Services proposés | texte | Boutique non alimentaire |

- Signalez tout problème d'ouverture (séparateur, accents mal affichés, dates en texte)	

Il y a du JSON dans les colonnes "horaires", "service", "prix" et "rupture"

## 3. Remplir la grille qualité

| Dimension | Méthode utilisée dans le tableur | Constat (chiffré ou exemple) | Gravité (faible / moyenne / forte) |
|---|---|---|---|
| Complétude | `NB.VIDE`, filtre sur *(Vides)* | dans la colonne horaires, 27 occ. absentes | forte|
| Exactitude | Tri, `MIN` / `MAX`, valeurs implausibles |Prix E85 : valeurs plausibles | RAS |
| Cohérence | Comparaison entre deux colonnes liées | Type rupture e10 et Carburants en rupture definitive : 1 occ de non adéquation (E10 non mentionné dans 1 ligne de la colone Carburant en rupture définitive) | forte |
| Validité | Filtre : formats hétérogènes dans une colonne | colonne Ville, pas de défaut identifié | RAS |
| Unicité | MFC *Valeurs en double* sur l'identifiant | colonne id, pas de défaut identifié | RAS |
| Fraîcheur | Date la plus récente vs date du jour | 2026-09-18T10:29:08+00:00, 18/09/2026| RAS |

## 4. Proposer des usages

### Usage 1

Pour les conducteurs routiers : où se réapprovisionner, de sorte à peser le moins possible dans les coûts généraux de carburant de l'entreprise ?

Identifier les stations services où le type d'essence utilisée est la moins chère, voir colonne "Carburants disponibles" pour vérifier la disponibilité du carburant, 
Consulter la colone du carburant concerné : Prix GPLc, Prix E10, Prix SP98, prix E85, Prix SP95, Prix Gazole.

Verifier la distance entre l'emplacement des camions à ravitailler et celui de la station essence envisagée (en cas de camion de panne sèche / ne sert à rien de traverser tout le département pour faire le plein - il y a une étude géographique à réaliser pour identifier une distance maximum, au delà de laquelle le déplacement n'est plus profitable).

Défaut qualité qui pourrait fausser la réponse : comme identifié en 3. dimension : cohérence, si la colone "Carburants disponibles" n'est pas mise à jour en fonction des ruptures affichées, un conducteur routier pourrait arriver dans une station essence qui ne vend pas le carburant de son camion.

### Usage 2

Cartographier la répartition de stations essence pour tester son uniformité, autrement dit si les stations essences sont en nombre suffisant et situées à des emplacements accessibles pour les habitants de la région.

Colonne utilisée : geom

Engage le Plan Local d'Urbanisme via la mairie et l'intercommunalité, et des acteurs locaux tels que des enseignes de grande distribution (Leclerc, Carrefour, etc) ou des compagnies pétrolières (TotalEnergies, Esso, etc.) qui voudraient implanter une nouvelle station dans le département, décider du meilleur emplacement stratégique.

Pas de défaut qualité qui pourrait fausser la réponse identifié.


