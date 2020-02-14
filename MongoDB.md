https://docs.mongodb.com/manual/installation/  
https://docs.mongodb.com/manual/tutorial/  
`sudo systemctl enable mongod`  
`sudo service mongod start`  

`mongod`  

# manage database:
`show dbs`  
`use test`  
`delete test`  

# manage user
`db.createUser(  
  {  
    user: "myTester",  
    pwd:  passwordPrompt(),   // or cleartext password  
    roles: [ { role: "readWrite", db: "test" },  
             { role: "read", db: "reporting" } ]  
  }  
)`  

from pymongo import MongoClient

# client = MongoClient('localhost', 27017)
client = MongoClient('localhost',
                 username='test',
                 password='test***',
                 authSource='posts',
                 authMechanism='SCRAM-SHA-256')
db = client['posts'] # db name

`mongo --port 27017 -u "myTester" --authenticationDatabase "test" -p`  

# manage docs
`db.list_collection_names()`  
`db.profiles.drop()`  
`db.collect1.insertOne( { _id: '001', x: 1 } );`  
`show collections;`  
`db.getCollection('collect1').find({"_id": '001'})`  
`db.posts.find({'day':{'$gt':'2019-11-30'}}`  
`db.posts.find({}, { "_id":1})`  // only "_id" field is returned 
`db.posts.find({}, { "_id":0})`  // only "_id" field is excluded 
`db.trips.find({"_id": {'$regex': "3F003"},
                                      "day": {
                                        '$gte': start_date,
                                        '$lt': end_date
                                      }})`  
`db.posts.findOne({})`  
`db.posts.remove({'_id':'test'});`  
` db.collection.update()`  
`db.posts.count()`  

# geospatial data
`location: {
      type: "Point",
      coordinates: [-73.856077, 40.848447]
}`  
`db.collection.createIndex( { <location field> : "2dsphere" } )`  