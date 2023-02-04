# Ranking Scraper

![forthebadge](/assets/forthebadge.svg)

Utilitaire de scraping des sites web des principaux classements internationaux d'universités.

L'outil : 

1. récupère par web scraping la totalité des données exposées sur les sites web : les scores totaux et par critères, ainsi que les rangs internationaux et nationaux
2. calcule les données précises de ranking manquantes pour les universités au-delà du top 100, à l'échelle mondiale et nationale, pour les scores totaux et pour chaque critère. 
   - classements internationaux : le ranking est calculé de 2 manières différentes 
      - en cas de score identique, le ranking suit  l'ordre d'apparition de l'université (colonnes calculated_dense_rank)
      - en cas de score identique, le ranking est le même (colonnes calculated_first_rank)
   - classements nationaux : la méthode first est utilisée
1. Deux possibilités d'export des données scrapées :
   - fichier Excel mis en forme et téléchargeable : il comprend une feuille par critère + une feuille pour le ranking total + une feuille de synthèse compilant toutes les données
   - sauvegarde dans une table d'une bdd sqlite (toutes les données compilées)

![ranking-scraper-output](/assets/screenshot.png)


## Rankins configurés

### Classement de Shangaî

- ARWU
- GRSSSD

# Installation

### Pré-requis

- OS Windows
- Firefox installé
- Extension geckodriver installée ((https://github.com/mozilla/geckodriver/releases)[https://github.com/mozilla/geckodriver/releases])

### Releases

- Télécharger la dernière release 
- Dézipper et installer n'importe où sur son PC (dans un emplacement accessible en écriture pour que le fichier de logs puisse s'incrémenter)
- Double-cliquer pour lancer l'application
  - Accessoirement créer un raccourci clic droit sur RankingScrapper.exe -> Envoyer vers ...
- la base de données sqlite se trouve dans <YOUR_PATH>/RankingScraper/db/


## [Dev] 

### Lancer en local

```
pip install -r requirements.txt
python app.py
```

### Release Github

1. Créer un dossier RankingScraper contenant :

```
|RankingScrapper.exe
|--/db
|   |--rankings.db
```
rankings.db est une bdd sqlite vide

2. Zipper le tout et l'uploader en release dans Github


