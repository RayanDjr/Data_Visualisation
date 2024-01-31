# Les joueurs NBA saison 2022-23

## Introduction

A partir d'un jeu de données repertoriant les statistiques des joueurs NBA sur la saison 2022-23, je vais essayer de récuperer un maximum de data afin de pouvoir réaliser des datavisualisation. Ce jeu de données à été recupéré sur le site nba stuffer https://www.nbastuffer.com/2022-2023-nba-player-stats/ qui est un site qui réalise des analyses statistiques en NBA

[Jeu de données initial](Stats_NBA.csv)

Avec l'aide D'OpenRefine, j'ai retravaillé ce jeu de données afin de le rendre le plus précis possible et de récuperer de la data pertinante qui permettra de créer des visualisations. Le plus gros du travail à été de récuperer le l'identifiant wikidata de chaque joueur afin de l'associer à son nom. Une fois cela effectué, j'ai pu récuperer un grand nombre d'information via des réconciliations comme par exemple le lieu de naissance de chaque joueurs puis ensuite les coordonnées géographique de chaque lieu de naissance ou encore les logos de chaque équipe. Le jeu de

[Historique OpenRefine](history_NBA.json)



[Jeu de données final](NBA.csv)


# D'ou proviennent les joueurs qui ont évolué en NBA durant la saison 2022-23

<iframe title="Provenance des joueurs NBA" aria-label="Map" id="datawrapper-chart-gUGfg" src="https://datawrapper.dwcdn.net/gUGfg/1/" scrolling="no" frameborder="0" style="width: 0; min-width: 100% !important; border: none;" height="386" data-external="1"></iframe><script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(a){if(void 0!==a.data["datawrapper-height"]){var e=document.querySelectorAll("iframe");for(var t in a.data["datawrapper-height"])for(var r=0;r<e.length;r++)if(e[r].contentWindow===a.source){var i=a.data["datawrapper-height"][t]+"px";e[r].style.height=i}}}))}();
</script>

La NBA étant une ligue Américaine, elle à longtemps été composer uniquement de joueurs américains. Cependant depuis quelques années on constate une augmentation des joueurs non américain au sein de la ligue. Il peut donc être intéressant de visualiser d'ou viennent les joueurs qui ont joué en 2022-23. Pour cela j'ai créer une map à partir du lieu de naissance de chaque joueurs.

# A quels postes jouent les joueurs les meilleurs scoreurs


<div class="flourish-embed flourish-scatter" data-src="visualisation/16636350"><script src="https://public.flourish.studio/resources/embed.js"></script></div>


# Quel est la moyenne d'age des joueurs par équipe 
<iframe title="Moyenne d'âge des joueurs NBA par équipe " aria-label="Map" id="datawrapper-chart-cOtAJ" src="https://datawrapper.dwcdn.net/cOtAJ/1/" scrolling="no" frameborder="0" style="width: 0; min-width: 100% !important; border: none;" height="645" data-external="1"></iframe><script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(a){if(void 0!==a.data["datawrapper-height"]){var e=document.querySelectorAll("iframe");for(var t in a.data["datawrapper-height"])for(var r=0;r<e.length;r++)if(e[r].contentWindow===a.source){var i=a.data["datawrapper-height"][t]+"px";e[r].style.height=i}}}))}();
</script>

```python
import pandas as pd

data = pd.read_csv('NBA.csv')

selected_columns = ['AGE', 'GP', 'MPG', 'USG%', 'TO%', 'FTA', 'FT%', '2PA', '2P%', '3PA', '3P%', 'eFG%', 'TS%', 'PPG', 'RPG', 'APG', 'SPG', 'BPG', 'TPG', 'P+R', 'P+A', 'P+R+A', 'VI', 'ORtg', 'DRtg']

selected_data = data[['TEAM'] + selected_columns]

team_averages = selected_data.groupby('TEAM', as_index=False).agg({**{'TEAM': 'first'}, **{col: 'mean' for col in selected_columns}})

team_averages[['logo image', 'LAT TEAM', 'LONG TEAM']] = data.groupby('TEAM', as_index=False).agg({'logo image': 'first', 'LAT TEAM': 'first', 'LONG TEAM': 'first'})[['logo image', 'LAT TEAM', 'LONG TEAM']]

team_averages.to_csv('NBA_moyenne.csv', index=False)

[Jeu de données par moyenne](NBA_moyenne.csv)

<div class="flourish-embed flourish-chart" data-src="visualisation/16637271"><script src="https://public.flourish.studio/resources/embed.js"></script></div>


<div class="flourish-embed flourish-hierarchy" data-src="visualisation/16636847"><script src="https://public.flourish.studio/resources/embed.js"></script></div>

