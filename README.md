# neo-pbc
The new pbc

Uses node + mongo on backend

###### Prereq
```
1. get node + npm
2. create project directory and cd into it
3. install LoopBack CLI via NPM: $ npm install -g loopback-cli
4. use loopback (make sure you are in project dir): $ lb 
     i. lb will ask questions, give your app a name, use the current loopback version, 
        select api-server as application type, follow through with the install
    ii. at this point the app can be run: $ node .
   iii. loopback generated APIs can be checked at the localhost address the CLI shows
5. Need to setup mongo connection to the app:
     i. open datasources.json in the Server folder, it should show connection is in memory.
        This means that the data source being used is RAM. Need to change this to Mongo.
    ii. make sure mongod service is running: $ sudo service mongodb status
   iii. $ npm install --save loopback-connector-mongodb    <--   no need for --save hook (automatic w/ this npm ver)
    iv. $ lb datasource mongoDS --connector mongoDB    <-- this will ask questions:
     v. datasource name: mongoDS, connector for mongoDS: mongoDB (supported by Strongloop)
        host: localhost, port: 27017, database: [any name]
    vi. The above selections should automatically leave the datasources.json file updated
        if not, datasources.json will need to be manually updated, here is an example:
        {
          "db": {
            "host": "localhost",
            "port": 27017,
            "url": "",
            "database": "food",
            "password": "",
            "name": "mongoDS",
            "user": "",
            "connector": "mongodb"
          }
        }
6. Now models need to be created in the db
      i. $ lb model
     ii. model name: <name>, datasource: db (mongodb), base class: PersistedModel, Rest API: yes, common model, then add properties.
    iii. This should automatically update the models in the common/models/db_name.json
7. Now if you run app: $ node .
   The loopback explorer (localhost:xxxx/explorer) will have REST API test links for the new db/models
```

###### Auth
```
1. $ lb acl
2. apply to all existing models, scope is all methods/properties, access type is all match all types, role:any unauth user, permission:deny
3. Now APIs will not work without auth
```

###### Register/Login
```
1. To register, POST to /Users: 
   {
     "realm": "string",
     "username": "urname",
     "email": "email@address.com",
     "emailVerified": true,
     "password": "xxxxx"
   }
   You should get response 200
2. To login, POST to /Users/login the info used for registration:
   {
     "email": "mail@address.com" ,
      "password": "xxxxx"
   }
3. Response json will contain an id field which is the access token. This token can be entered in the Loopback API explorer top bar to provide access to APIs in the explorer. 
For example: 

ACCESS_TOKEN=6Nb2ti5QEXIoDBS5FQGWIz4poRFiBCMMYJbYXSGHWuulOuy0GTEuGx2VCEVvbpBK

# Authorization Header
curl -X GET -H "Authorization: $ACCESS_TOKEN" \
http://localhost:3000/api/widgets

# Query Parameter
curl -X GET http://localhost:3000/api/widgets?access_token=$ACCESS_TOKEN

More info: https://loopback.io/doc/en/lb2/Making-authenticated-requests.html
```

###### Mongo
```
> show dbs   <-- see dbs
> use <db name>   <-- select a db to work with
> show collections   <-- show the documents/data stored in the selected db
> db.getCollection('repl-failOver').find({})     <-- if collection name has hyphen in it
```

###### Mongo example run:
```
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
```
