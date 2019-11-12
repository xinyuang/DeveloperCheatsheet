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
`db.collect1.insertOne( { x: 1 } );`  
`show collections;`  

`db.getCollection('collect1').find()`  