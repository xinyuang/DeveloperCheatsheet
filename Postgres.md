# initial
`CREATE DATABASE dbname;`  
`CREATE USER usr1 WITH PASSWORD 'password';`  
`GRANT ALL ON DATABASE test TO usr1;`  
`psql -h localhost -U usr1 test;`  

`ALTER USER postgres PASSWORD 'myPassword';`

`sudo -i -u postgres`
`createdb my_db`
`psql -d my_db`

`\list`  
`\conninfo`