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

### Packaging avec pyinstaller

A la racine du projet 

*Multi-files in /dist*
```
pyinstaller --noconfirm --onedir --windowed --add-data "C:/users/geoffroy/pythonapps/ranking-scrapper/venv_ranking/lib/site-packages/customtkinter;customtkinter/" "app.py"
```

*One executable file named RankingSraper in /dist and icone (but icon not working)*
```
pyinstaller --noconfirm --onefile --name RankingSraper --windowed --icon="C:/users/geoffroy/pythonapps/ranking-scrapper/assets/icone_cassiopeia.ico" --noconsole --add-data "C:/users/geoffroy/pythonapps/ranking-scrapper/venv_ranking/lib/site-packages/customtkinter;customtkinter/" "app.py"
```

*One executable file named RankingSraper in /dist and without icone*
```
pyinstaller --noconfirm --onefile --name RankingSraper --windowed --noconsole --add-data "C:/users/geoffroy/pythonapps/ranking-scrapper/venv_ranking/lib/site-packages/customtkinter;customtkinter/" "app.py"
```

L'exécutable se trouve dans le répertoire /dist

### Pour mémoire, bidouille interne

Charger le fichier Excel compilant toutes les données ARWU dans une bdd Sqlite

```
ranking_selection = "arwu"
wb = load_workbook("C:/Users/geoffroy/Downloads/ARWU.xlsx")
# sheet All
ws = wb.active
data = ws.values
columns = next(data)[0:]
df = pd.DataFrame(data, columns=columns).drop(columns=["index"])
df['calculated_first_world_rank'] = range(1, 1+len(df))
df = df.rename(columns={'N&S_score': 'NS_score', 'calculated_N&S_world_rank': 'calculated_NS_world_rank', 'calculated_N&S_national_rank': 'calculated_NS_national_rank', 'calculated_world_rank': 'calculated_dense_world_rank'})
conn = sqlite3.connect('C:/Users/geoffroy/PythonApps/ranking-scrapper/dist/rankings.db')
cur = conn.cursor()
cur.execute(f"DROP TABLE IF EXISTS {ranking_selection}")
cur.execute(f"CREATE TABLE {ranking_selection} ({','.join(map(str,df.columns)).replace('N&S', 'NS').replace('calculated_world_rank', 'calculated_dense_world_rank')})")
df.to_sql(f"{ranking_selection}", conn, if_exists='append', index=False)
```


