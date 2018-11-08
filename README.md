# neo-pbc
The new pbc

Uses node + mongo on backend

***********
Mongo
***********
> show dbs   <-- see dbs
> use <db name>   <-- select a db to work with
> show collections   <-- show the documents/data stored in the selected db
> db.getCollection('repl-failOver').find({})     <-- if collection name has hyphen in it

*example run:*
~/neo-pbc$ mongo
MongoDB shell version v3.4.7
> show dbs
admin  0.000GB
local  0.000GB
pets   0.000GB
> use pets
switched to db pets
> show collections
pet-types
> db.getCollection('pet-types').find({}) 
{ "_id" : ObjectId("5be2728e22acc717a0ade878"), "domesticated" : true, "animal-class" : "mammal", "owner-name" : "kathryn" }
{ "_id" : ObjectId("5be27df322acc717a0ade879"), "domesticated" : true, "animal-class" : "bird", "owner-name" : "kathryn" }
{ "_id" : ObjectId("5be27dfd22acc717a0ade87a"), "domesticated" : true, "animal-class" : "bird", "owner-name" : "usman" }
{ "_id" : ObjectId("5be27e0122acc717a0ade87b"), "domesticated" : true, "animal-class" : "reptile", "owner-name" : "usman" }
{ "_id" : "", "domesticated" : true, "animal-class" : "reptile", "owner-name" : "Goblo", "pet-name" : "Lizzie", "pet-type" : "lizard", "vaccinations" : "", "medical-issue" : "none", "lastfed" : "18nov7" }
> exit
bye
