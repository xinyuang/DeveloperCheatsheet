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

`mongo --port 27017 -u "myTester" --authenticationDatabase "test" -p`  

# manage tables
`db.collect1.insertOne( { _id: '001', x: 1 } );`  
`show collections;`  
`db.getCollection('collect1').find({"_id": '001'})`  
` db.collection.update()`  

# geospatial data
`location: {
      type: "Point",
      coordinates: [-73.856077, 40.848447]
}`  
`db.collection.createIndex( { <location field> : "2dsphere" } )`  