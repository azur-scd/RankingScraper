# Ranking Scraper

![forthebadge](/assets/forthebadge.svg)

Utilitaire de scraping des sites web des principaux classements internationaux d'universités.

L'outil : 

1. récupère par web scraping la totalité des données exposées sur les sites web : les scores totaux et par critères, ainsi que les rangs internationaux et nationaux
2. calcule les données précises de ranking manquantes pour les universités au-delà du top 100, à l'échelle mondiale et nationale, pour les scores totaux et pour chaque critère
3. fournit en sortie un fichier Excel mis en forme et téléchargeable

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
- Accessoirement créer un raccourci clic droit sur RnakingScrapper.exe -> Envoyer vers ...


## [Dev] 

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

### Release Github

Zipper l'exécutable en RangingScrapper.zip et l'uploader en release dans Github

