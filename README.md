# Les joueurs NBA saison 2022-23

## Introduction

A partir d'un jeu de données repertoriant les statistiques des joueurs NBA sur la saison 2022-23, je vais essayer de récuperer un maximum de data afin de pouvoir réaliser des datavisualisation. Ce jeu de données à été recupéré sur le site nbastuffer https://www.nbastuffer.com/2022-2023-nba-player-stats/ qui est un site qui réalise des analyses statistiques en NBA

[Jeu de données initial](Stats_NBA.csv)

Avec l'aide D'OpenRefine, j'ai retravaillé ce jeu de données afin de le rendre le plus précis possible et de récuperer de la data pertinante qui permettra de créer des visualisations. Le plus gros du travail à été de récuperer le l'identifiant wikidata de chaque joueur afin de l'associer à son nom. Une fois cela effectué, j'ai pu récuperer un grand nombre d'information via des réconciliations comme par exemple le lieu de naissance de chaque joueurs puis ensuite les coordonnées géographique de chaque lieu de naissance ou encore les logos de chaque équipe. Le jeu de

[Historique OpenRefine](history_NBA.json)



[Jeu de données final](NBA.csv)


# D'ou proviennent les joueurs qui ont évolué en NBA durant la saison 2022-23

La NBA étant une ligue Américaine, elle à longtemps été composer uniquement de joueurs américains. Cependant depuis quelques années on constate une augmentation des joueurs non américain au sein de la ligue. Il peut donc être intéressant de visualiser d'ou viennent les joueurs qui ont joué en 2022-23. Pour cela j'ai créer une map à partir du lieu de naissance de chaque joueurs.

<iframe title="Provenance des joueurs NBA" aria-label="Map" id="datawrapper-chart-gUGfg" src="https://datawrapper.dwcdn.net/gUGfg/1/" scrolling="no" frameborder="0" style="width: 0; min-width: 100% !important; border: none;" height="386" data-external="1"></iframe><script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(a){if(void 0!==a.data["datawrapper-height"]){var e=document.querySelectorAll("iframe");for(var t in a.data["datawrapper-height"])for(var r=0;r<e.length;r++)if(e[r].contentWindow===a.source){var i=a.data["datawrapper-height"][t]+"px";e[r].style.height=i}}}))}();
</script>


# A quels postes jouent les joueurs les meilleurs scoreurs

Les jouers étant rangé par ordre de scoring, on peut se demander quel est la répartition par poste des joueurs dans le classement des scoreurs. Pour ce faire j'ai utilisé un graphique en sucette afin de répartir tous les joueurs selon leurs poste et leurs position au classement

<div class="flourish-embed flourish-scatter" data-src="visualisation/16636350"><script src="https://public.flourish.studio/resources/embed.js"></script></div>

# Changement de perspective 
Après avoir utilisé les données de chaques joueurs, il semblait intéressant de regrouper les data de tous les joueurs par équipe afin de pouvoir les comparer entre elles. 
Pour ce faire j'ai donc réaliser un script python afin de moyenner les statistiques et de les regrouper par équipe. On récupère également le logo et les coordonnées de chaque équipe. 

### requête Pyhton
```python
import pandas as pd

data = pd.read_csv('NBA.csv')

selected_columns = ['AGE', 'GP', 'MPG', 'USG%', 'TO%', 'FTA', 'FT%', '2PA', '2P%', '3PA', '3P%', 'eFG%', 'TS%', 'PPG', 'RPG', 'APG', 'SPG', 'BPG', 'TPG', 'P+R', 'P+A', 'P+R+A', 'VI', 'ORtg', 'DRtg']

selected_data = data[['TEAM'] + selected_columns]

team_averages = selected_data.groupby('TEAM', as_index=False).agg({**{'TEAM': 'first'}, **{col: 'mean' for col in selected_columns}})

team_averages[['logo image', 'LAT TEAM', 'LONG TEAM']] = data.groupby('TEAM', as_index=False).agg({'logo image': 'first', 'LAT TEAM': 'first', 'LONG TEAM': 'first'})[['logo image', 'LAT TEAM', 'LONG TEAM']]

team_averages.to_csv('NBA_moyenne.csv', index=False)

```
Voici le résultat ; 
[Jeu de données par moyenne](NBA_moyenne.csv)

# Quel est la moyenne d'age des joueurs par équipe

Il est intéressant de se demander qu'elle est la moyenne d'âge par équipe en NBA

<iframe title="Moyenne d'âge des joueurs NBA par équipe " aria-label="Map" id="datawrapper-chart-cOtAJ" src="https://datawrapper.dwcdn.net/cOtAJ/2/" scrolling="no" frameborder="0" style="width: 0; min-width: 100% !important; border: none;" height="643" data-external="1"></iframe><script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(a){if(void 0!==a.data["datawrapper-height"]){var e=document.querySelectorAll("iframe");for(var t in a.data["datawrapper-height"])for(var r=0;r<e.length;r++)if(e[r].contentWindow===a.source){var i=a.data["datawrapper-height"][t]+"px";e[r].style.height=i}}}))}();
</script>

Il est dommage que ces moyennes soient arrondie à autant de chiffre après la virgule, cela enlève de la pertinance sur la donnée. 


# Corrélation entre éfficacité et précision de loin

Les tirs à 3 points sont les plus difficile à mettre au Basket. Intéressont nous à la corrélation entre l'efficacité et la précision à l'aide des statistiques sur le nombre moyen de tire rentré par équipe et le pourcentage moyen de tir rentré afin de voir si les équipes les plus éfficace sont aussi les plus précise  

<div class="flourish-embed flourish-chart" data-src="visualisation/16637271"><script src="https://public.flourish.studio/resources/embed.js"></script></div>


# Qui sont les plus efficaces 

Afin de pouvoir comparer toutes les équipes dans chacune des catégorie statistiques de la league, voici un shéma hiérarchique qu'il est possible de filtré selon la statistique désiré. 

<div class="flourish-embed flourish-hierarchy" data-src="visualisation/16636847"><script src="https://public.flourish.studio/resources/embed.js"></script></div>

# Ordre de grandeur 

A l'aide d'une requete wikidata il m'a été possible d'afficher la photo des joueurs ayant jouer en NBA mesurant plus de 2 mètre 15. Ils sont ranger dans l'ordre décroissant, il s'agit donc d'un classement des joueurs les plus grands. 

<iframe style="width: 80vw; height: 50vh; border: none;" src="https://query.wikidata.org/embed.html#%23defaultView%3AImageGrid%0ASELECT%20%3Fplayer%20%3FplayerLabel%20%3Fleague%20%3FleagueLabel%20%3Fimg%20%3Fheight%20%3Fbirthdate%20%3Fage%20%3Fnationality%20%3FnationalityLabel%0AWHERE%20%7B%0A%20%20%3Fplayer%20wdt%3AP106%20wd%3AQ3665646%3B%0A%20%20%20%20%20%20%20%20%20%20wdt%3AP118%20%3Fteam%3B%0A%20%20%20%20%20%20%20%20%20%20wdt%3AP2048%20%3Fheight%3B%0A%20%20%20%20%20%20%20%20%20%20wdt%3AP18%20%3Fimg.%0A%20%20%0A%20%20OPTIONAL%20%7B%20%3Fplayer%20wdt%3AP569%20%3Fbirthdate.%20%7D%0A%20%20BIND(year(now())%20-%20year(%3Fbirthdate)%20AS%20%3Fage).%0A%20%20%0A%20%20OPTIONAL%20%7B%20%3Fplayer%20wdt%3AP27%20%3Fnationality.%20%7D%0A%0A%20%20FILTER(%3Fteam%20%3D%20wd%3AQ155223%20%26%26%20%3Fheight%20%3E%20215).%0A%20%20%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%7D%0A%0AORDER%20BY%20DESC(%3Fheight)%0A%0A%0A%0A" referrerpolicy="origin" sandbox="allow-scripts allow-same-origin allow-popups"></iframe>

### requête SPARQL

```sparql
#defaultView:ImageGrid
SELECT ?player ?playerLabel ?league ?leagueLabel ?img ?height ?birthdate ?age ?nationality ?nationalityLabel
WHERE {
  ?player wdt:P106 wd:Q3665646;
          wdt:P118 ?team;
          wdt:P2048 ?height;
          wdt:P18 ?img.
  
  OPTIONAL { ?player wdt:P569 ?birthdate. }
  BIND(year(now()) - year(?birthdate) AS ?age).
  
  OPTIONAL { ?player wdt:P27 ?nationality. }

  FILTER(?team = wd:Q155223 && ?height > 215).
  
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }
}

ORDER BY DESC(?height)
```
