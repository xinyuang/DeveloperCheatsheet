https://docs.mongodb.com/manual/installation/
`sudo systemctl enable mongod`  
`sudo service mongod start`  

`mongod`  

`use test`  
`db.createUser(  
  {  
    user: "myTester",  
    pwd:  passwordPrompt(),   // or cleartext password  
    roles: [ { role: "readWrite", db: "test" },  
             { role: "read", db: "reporting" } ]  
  }  
)`  

`mongo --port 27017 -u "myTester" --authenticationDatabase "test" -p`  
`show dbs`  

`use newDB`  
`db.newDB.insertOne( { x: 1 } );`  

`db.getCollection('newDB').find()`  