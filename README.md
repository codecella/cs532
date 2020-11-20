
Carolina Hernandez Mateo
B0081551
cherna19@binghamton.edu





CS532 - Database Systems Fall 20


My original Dataset is too heavy so i uploaded into a public drive in Case is needed here is the link

https://drive.google.com/drive/folders/1zq0ep_d2F2CzZ2LG3WWVhwDCorhzc03Y?usp=sharing


## Running the code

> Jupyter lab
> Execute


Tips:

> Import command  the databse .json script into MongoDB  - Command to import and export from is on the Report
>  Use your default MongoDB Connection IP.
>Run the queries you like to explore on the Dataset. You can start with the exploratory questions i choose for my project. Look more into to the report to learn about it.


## Exploraroty Questions related to the Datasets in MongoDB

It's important to explore the datasets in order to write better Queries with python code and PyMongo in Jupyter notebook.

Related to Dataset & MongoDB Tweets Data analysis

Aggregation

Count all tweets by language (works)

db.tweets.aggregate([
{ $group: {
_id: '$lang',
count: {$sum: 1}
}},

{ $match: {
count: { $gt: 100 }
}},

{ $sort: {
count: -1
}},

{ $project: {
language: '$_id',
count: 1,
_id: 0
}}
]);

Top users in tweets collections (works)

db.tweets.aggregate(

    { $project: {
        _id: 0,
        "entities.user_mentions" : 1
    }},
    { $unwind: "$entities.user_mentions" },

    { $group : {
        _id: "$entities.user_mentions.screen_name",
        count: { $sum : 1 }
    }},

    { $sort : {
        "count" : -1
    }}
)


Count all the hashtags in the collection  - Top hashtags and keywords.


db.tweets.aggregate(

    { $project: {
        _id: 0,
        "entities.hashtags" : 1
    }},
    { $unwind: "$entities.hashtags" },

    { $group : {
        _id: "$entities.hashtags.text",
        count: { $sum : 1 }
    }},

    { $sort : {
        "count" : -1
    }}
)


MapReduce functions. Find covid associated Keywords. Slower than aggregation. so
so i didn't implement it in python.

var map = function(){
	 var tweet=this.tweet;
	if(tweet.search(“coronavirus”) > -1){
          tweet = tweet.toLowerCase().split(“  “);
           for(var i=0;  i<=tweet.length-1; i++){
                   emit(tweet[i],1);
                }
         }

}


Tweet Word count by locations in the US cities

db.tweets.aggregate(

    { $project: {
        _id: 0,
        "entities.location” : 1
    }},
    { $unwind: "$entities..location” },

    { $group : {
        _id: "$entities.location.text",
        count: { $sum : 1 }
    }},

    { $sort : {
        "count" : -1
    }}
)

