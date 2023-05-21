# Tp-ClientServeur
## 1/ Compter le nombre de Thriller présents en base  (films qui possèdent au moins le genre Thriller)
*


db.movies.find({ "genres": "Thriller" }).count()


## 2/ Trouver la liste des films qui contiennent le mot "ghost" dans leur titre
*
db.movies.find({ "title": { "$regex": /ghost/i } })

## 3/ Trouver la liste des films qui contiennent le mot "ghost" dans leur titre et qui sont sortis après 2013
*
db.movies.find({
  title: /.*ghost.*/i,
  year: { $gt: 2013 },
});

## 4/ Trouver le film qui a gagné le plus de récompenses
*
db.movies.aggregate([
   { "$sort": { "awards.wins": -1 } },
   { "$limit": 1 }
])
## 5/ Trouver le plus vieux film de plus de 2 heures ayant une note inférieur à 2 sur la plateforme imdb
*
db.movies.aggregate([
{
  $match: {
    "runtime": { $exists: true },
    "imdb.rating": { $exists: true },
    "year": { $exists: true },
  }
},
{
  $match: {
    "runtime": { $gt: 120 },
    "imdb.rating": { $lt: 2 },
  }
},
{
  $sort: { "year": -1 }
},
{
  $limit: 1
}
]);

## 6/ Modifier la requête précédente pour récupérer en même temps les commentaires.
*
db.movies.aggregate([
{
  $match: {
    "runtime": { $exists: true },
    "imdb.rating": { $exists: true },
    "year": { $exists: true },
  }
},
{
  $match: {
    "runtime": { $gt: 120 },
    "imdb.rating": { $lt: 2 },
  }
},
{
  $sort: { "year": -1 }
},
{
  $limit: 1
},
{
  $lookup: {
    from: "comments",
    localField: "_id",
    foreignField: "movie_id",
    as: "movie_comments"
  }
}
]);

## 7/ Récupérer le nombre de films par pays ainsi que leurs titres.
*
db.movies.aggregate([
    { "$unwind": "$countries" },
    {
        "$group": {
            "_id": "$countries",
            "count": { "$sum": 1 },
            "titles": { "$push": "$title" }
        }
    },
    {
        "$project": {
            "_id": 0,
            "country": "$_id",
            "count": 1,
            "titles": 1
        }
    },
    { "$sort": { "count": -1 } }
])



