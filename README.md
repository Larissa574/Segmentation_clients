#  Segmentation Clients

Projet de segmentation client en apprentissage non supervisé a partir d'un historique transactionnel e-commerce.

## Objectif

Construire un systeme de segmentation qui repond a la question business principale:

"Combien de groupes de clients distincts existe-t-il et qu'est-ce qui les caracterise ?"

## Donnees

- Source: Online Retail II (UCI / Kaggle)
- Fichier utilise dans le projet: `online_retail_II.csv`
- Granularite initiale: transaction (plusieurs lignes par client)

Colonnes principales:

- `Invoice` / `Facture`
- `StockCode` / `Code_Produit`
- `Description`
- `Quantity` / `Quantite`
- `InvoiceDate` / `Date_Facture`
- `Price` / `Prix_Unitaire`
- `Customer ID` / `ID_Client`
- `Country` / `Pays`

## KPIs Cibles

- Silhouette score >= 0.45
- Nombre de clusters compris entre 3 et 6
- Chaque cluster doit etre interpretable metier

## Approche Methodologique

### 1) EDA et preparation

- Analyse des valeurs manquantes
- Detection des doublons
- Visualisations de distribution et correlation

### 2) Feature Engineering RFM (niveau client)

Transformation du dataset transactionnel en table client (1 ligne = 1 client):

- **Recence**: nombre de jours depuis le dernier achat
- **Frequency**: nombre d'achats (factures uniques)
- **Monetary** (via `TotalPrice`): montant cumule depense

### 3) Pretraitement

- Split train/test au niveau client
- Pipeline `sklearn`:
	- imputation des numeriques (median)
	- standardisation (`StandardScaler`)
	- imputation + encodage des categorielles (`OneHotEncoder`)

### 4) Clustering compare

- **KMeans++** (avec selection de `k`)
- **Agglomerative Clustering (CAH)**
- **DBSCAN** (gestion du bruit / outliers)

Evaluation avec:

- Silhouette score
- Davies-Bouldin Index
- Contraintes metier (nombre de clusters, taille minimale de cluster, bruit)

### 5) Reduction de dimension et visualisation

- PCA pour reduction avant clustering final
- t-SNE 2D pour visualisation des segments
- Visualisation interactive avec Plotly

### 6) Profilage marketing des segments

Pour chaque cluster:

- caracteristiques RFM moyennes
- nom de segment marketing
- strategie recommandee

Exemples de personas utilises:

- VIP Fideles
- Clients Dormants
- Nouveaux Clients
- Acheteurs Occasionnels

## Structure Du Projet

- `segmentation_clients.ipynb` : notebook principal (EDA, modelisation, profilage)
- `online_retail_II.csv` : dataset source
- `requirements.txt` : dependances Python
- `images/` : exports des figures (matplotlib/plotly)

## Installation

1. Creer un environnement virtuel
2. Installer les dependances:

```bash
pip install -r requirements.txt
```

## Execution

Lancer le notebook:

```bash
jupyter notebook segmentation_clients.ipynb
```

Puis executer les cellules dans l'ordre.

## Dependances Principales

- pandas
- numpy
- matplotlib
- seaborn
- missingno
- scikit-learn
- scipy
- plotly
- kaleido (export PNG des figures Plotly)

## Livrables

- Notebook complet avec EDA + segmentation
- Comparaison de 3 familles d'algorithmes de clustering
- Visualisations PCA / t-SNE
- Tableau de recommandations marketing par segment

## Notes

- Les resultats peuvent varier legerement selon l'echantillonnage t-SNE.
- L'export PNG Plotly necessite `kaleido`.

