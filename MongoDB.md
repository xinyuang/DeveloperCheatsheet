https://docs.mongodb.com/manual/installation/
sudo systemctl enable mongod
sudo service mongod start

`mongod`  
`db.createUser({user:"admin_name", pwd:"1234",roles:["readWrite","dbAdmin"]})`  
db.auth('admin_name','1234')
