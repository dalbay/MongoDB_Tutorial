# MongoDB_Tutorial
![mongoDB explained](images/mongoDB1.png)  
**Key MongoDB Features:**
![mongoDB features](images/mongoDB2.png)
![mongoDB vs relational db](images/mongoDB3.png)

## Installing MongoDB on Windows
- run mongod.exe in cmd prompt located in  C:\Program Files\MongoDB\Server\4.2\bin - this will run the server
- run mongo shell mongo.exe in another cmd prompt located in C:\Program Files\MongoDB\Server\4.2\bin - for CRUD operations. this will connect to the server we just opened on the same port - 27017
- run db and see if the test database runs:
```
> db
test
>
```   
- close both cmd prompts - server and mongo shell with Ctrl+c
- ***to run the mongo server on a different directory***:  
  (the mongod.exe file comes from the bin directory); we need to look for this file in the bin directory by using **System variables**.  
  Go to Windows Setting -> in the search bar type in env and click on Edit system Environmental Variables -> **Environmental Variables** -> System variables - Path -> Edit -> Add mongoDB here; copy the bin directory for mongoDB; New -> past path here. -> OK  
  Now you can run mongo in any directory.  

## Creating a Local Database
- run server and mongo Shell (**cls** to clean the terminal)
- **use**command - will create or use the db
```
> use natours-test
switched to db natours-test
```
- create document inside a collection - **```> db.tours.insertMany()```**
- **insertMany()**method to create multiple documents; will except an array of objects; **insertOne()**method to insert one document
- insert a JavaScript object; this will be converted into JSON and later on to BSON.
```
> db.tours.insertOne({ name: "The Forest Hiker", price: 297, rating: 4.7 })
{
        "acknowledged" : true,
        "insertedId" : ObjectId("5db5c1031d3e30f64d200487")
}
>
```
- to check if the data is inserted - **```db.tours.find()```**(notice the auto created objectId; and return is a JSON object)
```
> db.tours.find()
{ "_id" : ObjectId("5db5c1031d3e30f64d200487"), "name" : "The Forest Hiker", "price" : 297, "rating" : 4.7 }
>
```  
- display all the databases that we have in mongoDB - **```show dbs```**
```
> show dbs
admin         0.000GB
config        0.000GB
local         0.000GB
natours-test  0.000GB
>
```
- displays the collections in a database - **```show collections```**  
```
> show collections
tours
>
```  
- to exit mongoDB - **```quit()```**
<br/>

## CRUD: Creating Documents
```JavaScript
> db.tours.insertMany( [{ name: "The Sea Explorer", price: 497, rating: 4.8 } , { name: "The Snow Adventure", price: 997, rating: 4.9, difficulty: "easy" }] )
{
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("5db5c49f5210ece73831c26e"),
                ObjectId("5db5c49f5210ece73831c26f")
        ]
}
>
```  
<br/>

##CRUD: Querying (Reading) Documents
- get all - **```> db.tours.find()```**
- get one - **```> db.tours.find( { name: "The Forest Hiker" } )```**
- use different query operators - **```> db.tours.find( { price: {$lte: 500} })```**
- use two search criterias at the same time - **```> db.tours.find( { price: {$lt: 500}, rating: {$gte: 4.8} })   ```**
- use or query - **```> db.tours.find({ $or: [ {price: {$lt: 500}}, {rating: {$gte: 4.8}} ] } )```**
- besides the filter object, we can also pass in an object for **projection**(selecting some of he fields in the output.) - **```> db.tours.find({ $or: [ {price: {$gt: 500}}, {rating: {$gte: 4.8}} ] }, {name: 1} )```**  
```JSON
{ "_id" : ObjectId("5db5c49f5210ece73831c26e"), "name" : "The Sea Explorer" }
{ "_id" : ObjectId("5db5c49f5210ece73831c26f"), "name" : "The Snow Adventure" }
```  
<br/>

## CRUD: Updating Documents
- to update one document - **```> db.tours.updateOne( {name: "The Snow Adventure"}, { $set: {price: 597} })```**
- create new properties and set these to a new value - **```> db.tours.updateMany({ price: {$gt: 500}, rating: {$gte: 4.8} }, {$set: {premium: true} })```**  
```JSON
{ "_id" : ObjectId("5db5c49f5210ece73831c26f"), "name" : "The Snow Adventure", "price" : 597, "rating" : 4.9, "difficulty" : "easy", "premium" : true }
```  
- to replace documents, pass in the search query and then the new data - **```> db.tours.replaceOne()```** / **```> db.tours.replaceMany()```**  
<br/>  

## CRUD: Deleting Documents
- **```> db.tours.deleteMany( { rating: {$lt: 4.8} })```**  
- to delete all documents - **```> db.tours.deleteMany( {} )```**  
<br/>  

## Using Compass App for CRUD Operations
- Instead of using the terminal we can also use an Compass - download **MongoDB Compass** - mongodb.com -> Tools -> Compass
- Connet to Host
![mongoDB compass db](images/mongoDB4.png)
- here you can Insert, Update, and Delete Documents; and filter documents; use Aggregations, Indexes
![mongoDB compass db](images/mongoDB5.png)  
<br/>

## Creating a Hosted Database with Atlas
- create a remote db hosted on mongoDB atlas.
- mongodb.com -> Products -> MongoDB Atlas -> Start Free (Create Account) -> Create New Project -> Build A Cluster(instance of our db)
- Now we have a blank database, ready to connnect to our own development computer.
![mongoDB atlas db](images/mongoDB6.png)  
<br/>

## Connecting to Our Hosted Database
- in the Atlas app click on Connect; follow the steps:
![mongoDB vs relational db](images/mongoDB6.png)
- Create a MongoDB user by copying the user name and password and save it in your config file - **config.env**
```JavaScript
NODE_ENV=development
PORT=8000
DATABASE_PASSWORD=X31AiFbvjVRYNXXE
```
- Choose a connection method - Connect with MongoDB Compass
- Copy the connection string and open up Compass - this will detect the connection string in the clipboard and auto fill the settings for us. Copy your password here and click Connect
- Here, create a Database - Database Name = natours / Collection Name = tours
![create a mongoDB database](images/mongoDB8.png)
- Insert your first document on a remote database
![insert document to a remote database](images/mongoDB9.png)
- Open browser and check your cluster - Cluster -> Collections - Here we see the database name(natours), collection name(tours), and the tour that we have just created in Compass.
![display in Compass](images/mongoDB10.png)
- Next, allow access to this cluster from everywhere. (We listed our IP in order to grant access to this cluster) If we want to work on a different computer - Click on Clusters -> Security -> IP WhiteList -> Add IP Address -> ALLOW ACCESS FROM ANYWHERE
![ALLOW access anywhere](images/mongoDB11.png)
- Connect MongoDB Shell to this cluster. Clusters -> Connect -> Connect With MongoDB Shell (If you already have it installed just copy this connect string) -> open MongoDB Shell (cmd - mongo.exe) -> quit() (if it connected to locally running mongo server.) -> paste the string in the terminal 
mongo "mongodb+srv://cluster0-hhfve.mongodb.net/test"  --username aygun -> enter password
- type ```show dbs``` in the terminal and check if you can see the databases
```
MongoDB Enterprise Cluster0-shard-0:PRIMARY> show dbs
admin    0.000GB
local    3.283GB
natours  0.000GB
MongoDB Enterprise Cluster0-shard-0:PRIMARY>
```
- This display shows that we are connected
- next type ```use natours``` in the terminal - this will switch to db natours
- type ```db.tours.find()``` -  here we see the document that we have just created in Compass.
```
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use natours
switched to db natours
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.tours.find()
{ "_id" : ObjectId("5db9a63954620261740cfe4b"), "name" : "The Forest Hiker", "price" : 297, "rating" : 4.7 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY>
```
---
