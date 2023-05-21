# Tp-ClientServeur
## 1/ Compter le nombre de Thriller présents en base  (films qui possèdent au moins le genre Thriller)
*


db.movies.find({ "genres": "Thriller" }).count()


## 2/ Trouver la liste des films qui contiennent le mot "ghost" dans leur titre
