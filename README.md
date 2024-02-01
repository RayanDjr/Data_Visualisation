# Les joueurs NBA saison 2022-23  ![NBA Logo](https://logos-marques.com/wp-content/uploads/2021/02/NBA-Logo-1969.png)


## Introduction

A partir d'un jeu de données repertoriant les statistiques des joueurs NBA sur la saison 2022-23, je vais essayer de récuperer un maximum de data afin de pouvoir réaliser des datavisualisation. Ce jeu de données à été recupéré sur le site nbastuffer https://www.nbastuffer.com/2022-2023-nba-player-stats/ qui est un site qui réalise des analyses statistiques en NBA

[Jeu de données initial](Stats_NBA.csv)

Avec l'aide D'OpenRefine, j'ai retravaillé ce jeu de données afin de le rendre le plus précis possible et de récuperer de la data pertinante qui permettra de créer des visualisations. Le plus gros du travail à été de récuperer le l'identifiant wikidata de chaque joueur afin de l'associer à son nom. Une fois cela effectué, j'ai pu récuperer un grand nombre d'information via des réconciliations comme par exemple le lieu de naissance de chaque joueurs puis ensuite les coordonnées géographique de chaque lieu de naissance ou encore les logos de chaque équipe. Le jeu de

[Historique OpenRefine](history_NBA.json)



[Jeu de données final](NBA.csv)


# D'ou proviennent les joueurs qui ont évolué en NBA durant la saison 2022-23

La NBA étant une ligue Américaine, elle à longtemps été composer uniquement de joueurs américains. Cependant depuis quelques années on constate une augmentation des joueurs non américain au sein de la ligue. Il peut donc être intéressant de visualiser d'ou viennent les joueurs qui ont joué en 2022-23. Pour cela j'ai créer une map à partir du lieu de naissance de chaque joueurs. Chaque point correspond à un joueur. 

<iframe title="Provenance des joueurs NBA" aria-label="Map" id="datawrapper-chart-gUGfg" src="https://datawrapper.dwcdn.net/gUGfg/1/" scrolling="no" frameborder="0" style="width: 0; min-width: 100% !important; border: none;" height="386" data-external="1"></iframe><script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(a){if(void 0!==a.data["datawrapper-height"]){var e=document.querySelectorAll("iframe");for(var t in a.data["datawrapper-height"])for(var r=0;r<e.length;r++)if(e[r].contentWindow===a.source){var i=a.data["datawrapper-height"][t]+"px";e[r].style.height=i}}}))}();
</script>

On constate que la très grande majorité des joueurs NBA sont né aux Etats-Unis, nottament sur la côte est. Pour ce qui est des joueurs non américains, la plus grande majorité d'entre eux viennent d'Europe Central. Cela peut s'expliquer par la grande popularité du Basket dans cette région. 

# A quels postes jouent les joueurs les meilleurs scoreurs

Les jouers étant rangé par ordre de scoring, on peut se demander quel est la répartition par poste des joueurs dans le classement des scoreurs. Pour ce faire j'ai utilisé un graphique en sucette afin de répartir tous les joueurs selon leurs poste et leurs position au classement

<div class="flourish-embed flourish-scatter" data-src="visualisation/16636350"><script src="https://public.flourish.studio/resources/embed.js"></script></div>

L'intérêt ici est de regarder de plus près les première place du classement des scoreurs en fonction des postes. On constate que les Guard sont les plus présent dans les 20 premières place. Il est à noté que le numéro 1 à pour poste  Pivot(center) et que le deuxième Pivot présent dans le classement n'est qu'à la 28ème place. 

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
Voici le résultat :  
[Jeu de données par moyenne](NBA_moyenne.csv)

# Quel est la moyenne d'age des joueurs par équipe

Il est intéressant de se demander qu'elle est la moyenne d'âge par équipe en NBA. Pour ce faire, une map parait être une parfaite representation. les équipes sont rangé selon position géographique et en passant la souris sur les points on peut connaître la moyenne d'age de l'équipe. Les points sont également coloré en fonction de la moyenne d'âge

<iframe title="Moyenne d'âge des joueurs NBA par équipe " aria-label="Map" id="datawrapper-chart-cOtAJ" src="https://datawrapper.dwcdn.net/cOtAJ/2/" scrolling="no" frameborder="0" style="width: 0; min-width: 100% !important; border: none;" height="643" data-external="1"></iframe><script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(a){if(void 0!==a.data["datawrapper-height"]){var e=document.querySelectorAll("iframe");for(var t in a.data["datawrapper-height"])for(var r=0;r<e.length;r++)if(e[r].contentWindow===a.source){var i=a.data["datawrapper-height"][t]+"px";e[r].style.height=i}}}))}();
</script>

Il est dommage que ces moyennes soient arrondie à autant de chiffre après la virgule, cela enlève de la pertinance sur la donnée. 


# Corrélation entre éfficacité et précision de loin

Les tirs à 3 points sont les plus difficile à mettre au Basket. Intéressont nous à la corrélation entre l'efficacité et la précision à l'aide des statistiques sur le nombre moyen de tire rentré par joueur par équipe sur la saison et le pourcentage moyen de tir rentré par joueur par saison afin de voir si les équipes les plus éfficace sont aussi les plus précise  

<div class="flourish-embed flourish-chart" data-src="visualisation/16637271"><script src="https://public.flourish.studio/resources/embed.js"></script></div>

On constate qu'il n'y a pas de réel corrélation entre éfficacité et précision. Pour la plupart des équipe le point correspondant au % de réussite à 3pts ne se rapproche pas du nombre de 3 points reussis. Il ya même de grande difféence entre ces données. certaines équipe sont précise sans être efficace et d'autre au contraire sont efficace sans être précise, on soupçonne donc un grand volume de tirs tentés. Seul les joueurs de l'équipe des Portland Trailblaizers on une efficacité fidèle à leur précision.

# Qui sont les plus efficaces 

Afin de pouvoir comparer toutes les équipes dans chacune des catégorie statistiques de la league, voici un shéma hiérarchique qu'il est possible de filtré selon la statistique désiré. Les statistiques utilisés ont été réalisé en faisant la moyenne des statistiques individuelles de chaques joueurs par équipe. 

<div class="flourish-embed flourish-hierarchy" data-src="visualisation/16636847"><script src="https://public.flourish.studio/resources/embed.js"></script></div>

Voici un glossaire permettant de pouvoir comprendre à quoi correspond chacune des statistiques disponible. 

## Glossaire (NBA Stats Avancées)

<details>
<summary style="font-weight: bold;">GP (Games Played)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Le nombre total de matchs auxquels les joueurs ont participé au cours de la saison.</span><br>
</details>

<details>
<summary style="font-weight: bold;">MPG (Minutes Per Game)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La moyenne de minutes jouées par match par joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">USG% (Usage Percentage)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Le pourcentage de possessions se terminant dans les mains du joueur lorsque il est sur le terrain.</span><br>
</details>

<details>
<summary style="font-weight: bold;">TO% (Turnover Percentage)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Le pourcentage de possessions du joueur se terminant par une perte de balle.</span><br>
</details>

<details>
<summary style="font-weight: bold;">FTA (Free Throws Attempted)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Le nombre total de lancers francs tentés par le joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">FT% (Free Throw Percentage)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Le pourcentage de réussite des lancers francs du joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">2PA (2-Point Attempts)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Le nombre total de tentatives de paniers à deux points par le joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">2P% (2-Point Percentage)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Le pourcentage de réussite des tentatives de paniers à deux points du joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">3PA (3-Point Attempts)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Le nombre total de tentatives de paniers à trois points par le joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">3P% (3-Point Percentage)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Le pourcentage de réussite des tentatives de paniers à trois points du joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">eFG% (Effective Field Goal Percentage)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Une statistique ajustant le pourcentage de réussite au tir pour tenir compte de la valeur accrue des tirs à trois points.</span><br>
<span style="color: green;">Formule:</span> <span style="color: green;">((FGM + (0.5 * 3PM)) / FGA)</span><br>
</details>

<details>
<summary style="font-weight: bold;">TS% (True Shooting Percentage)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Une statistique cumulant les différents types de tirs qu’un joueur peut prendre (2 points, 3 points, lancers francs) pour déterminer la qualité globale du tir.</span><br>
<span style="color: green;">Formule:</span> <span style="color: green;">(PTS / (2 * (FGA + 0.44 * FTA)))</span><br>
</details>

<details>
<summary style="font-weight: bold;">PPG (Points Per Game)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La moyenne de points marqués par match du joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">RPG (Rebounds Per Game)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La moyenne de rebonds par match du joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">APG (Assists Per Game)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La moyenne de passes décisives par match du joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">SPG (Steals Per Game)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La moyenne d'interceptions par match du joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">BPG (Blocks Per Game)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La moyenne de contres par match du joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">TPG (Turnovers Per Game)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La moyenne de pertes de balle par match du joueur.</span><br>
</details>

<details>
<summary style="font-weight: bold;">P+R (Points + Rebounds)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La somme des points et des rebonds du joueur par match.</span><br>
</details>

<details>
<summary style="font-weight: bold;">P+A (Points + Assists)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La somme des points et des passes décisives du joueur par match.</span><br>
</details>

<details>
<summary style="font-weight: bold;">P+R+A (Points + Rebounds + Assists)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">La somme des points, des rebonds et des passes décisives du joueur par match.</span><br>
</details>

<details>
<summary style="font-weight: bold;">VI (Versatility Index)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Un indice mesurant la polyvalence d'un joueur en combinant points, rebonds et passes décisives.</span><br>
<span style="color: green;">Formule:</span> <span style="color: green;">(P+R+A) / GP</span><br>
</details>

<details>
<summary style="font-weight: bold;">ORtg (Offensive Rating)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Mesure le nombre de points produits par un joueur pour 100 possessions.</span><br>
</details>

<details>
<summary style="font-weight: bold;">DRtg (Defensive Rating)</summary>
<span style="color: blue;">Description:</span> <span style="color: blue;">Mesure le nombre de points concédés par un joueur pour 100 possessions.</span><br>
</details>

<details>
<summary>Source:</summary>
<span style="font-size: small;">ce glossaire à été réalisé grâce aux informations disponibles sur [Viziball](https://viziball.app/glossary/nba/fr) et [Who's The Bet](https://whosthebet.blogspot.com/2015/08/la-nba-pour-les-nuls-les-statistiques.html).</span>
</details>

# Ordre de grandeur 

A l'aide d'une requete wikidata il m'a été possible d'afficher la photo des joueurs ayant jouer en NBA mesurant plus de 2 mètre 15. Ils sont ranger dans l'ordre décroissant, il s'agit donc d'un classement des joueurs les plus grands. 

<iframe style="width: 80vw; height: 50vh; border: none;" src="https://query.wikidata.org/embed.html#%23defaultView%3AImageGrid%0ASELECT%20DISTINCT%20%3FplayerLabel%20%3FleagueLabel%20%3Fheight%20%3Fbirthdate%20%3FimgLabel%20%3Fage%20%3FnationalityLabel%0AWHERE%20%7B%0A%20%20%3Fplayer%20wdt%3AP106%20wd%3AQ3665646%3B%0A%20%20%20%20%20%20%20%20%20%20wdt%3AP118%20%3Fleague%3B%0A%20%20%20%20%20%20%20%20%20%20wdt%3AP2048%20%3Fheight%3B%0A%20%20%20%20%20%20%20%20%20%20wdt%3AP18%20%3Fimg%3B%0A%20%20%20%20%20%20%20%20%20%20wdt%3AP27%20%3Fnationality%3B%0A%20%20%20%20%20%20%20%20%20%20wdt%3AP569%20%3Fbirthdate.%20%0A%20%20%0A%20%20BIND(year(now())%20-%20year(%3Fbirthdate)%20AS%20%3Fage).%0A%20%0A%20%20FILTER(%3Fleague%20%3D%20wd%3AQ155223%20%26%26%20%3Fheight%20%3E%20215).%0A%20%20%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%7D%0A%0AORDER%20BY%20DESC(%3Fheight)" referrerpolicy="origin" sandbox="allow-scripts allow-same-origin allow-popups"></iframe>

Certains joueurs ayant plusieurs nationalité appraraissent 2 fois. 

## requête SPARQL

```sparql
#defaultView:ImageGrid
SELECT ?playerLabel ?league ?imgLabel ?height ?birthdate ?age ?nationality
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
