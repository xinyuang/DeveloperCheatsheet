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

# table
```SQL
CREATE TABLE playground (
    equip_id serial PRIMARY KEY,
    type varchar (50) NOT NULL,
    color varchar (25) NOT NULL,
    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),
    install_date date
);
```