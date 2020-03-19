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

db = client['posts'] # db name

`mongo --port 27017 -u "myTester" --authenticationDatabase "test" -p`  

# manage docs
`client = MongoClient('localhost',
                 username='test',
                 password='test***',
                 authSource='posts',
                 authMechanism='SCRAM-SHA-256')`  
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

`
    mongo_db.example.aggregate([
        {
            "$match":
                {"$and":
                    [
                        {"cur_mode": "manual"},
                        {"_id": {'$regex': regex}},
                        {"moving_avg_speed": {"$gt": speed_lower*1.60934, "$lt": speed_upper*1.60934}},
                        {"moving_avg_mpg": {"$gt": 2}},
                    ]
                }
        },
        {
            "$group": {
                '_id': 'null',
                'moving_avg_mpg': {'$avg': '$moving_avg_mpg'},
                "ttl_distance": {"$sum": '$ttl_distance'},
                "ttl_fuel_used": {"$sum": '$ttl_fuel_used'}
            }
        },
        {"$project": {"_id": 1, 'moving_avg_mpg': 1, "ttl_distance": 1, "ttl_fuel_used": 1}}
    ]))
`  

# backup and restore
`mongodump --collection=myCollection --db=test`  
`mongodump --host=mongodb1.example.net --port=3017 --username=user --password="pass" --out=/opt/backup/mongodump-2013-10-24`  
`mongorestore --host=mongodb1.example.net --port=3017 --username=user  --authenticationDatabase=admin /opt/backup/mongodump-2013-10-24`  