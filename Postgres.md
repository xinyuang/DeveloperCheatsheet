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
1. create table
```SQL
CREATE TABLE playground (
    equip_id serial PRIMARY KEY,
    type varchar (50) NOT NULL,
    color varchar (25) NOT NULL,
    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),
    install_date date
);
```
2. table insert
```SQL
INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2014-04-28');
INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2010-08-16');
```
3. query table
```SQL
DELETE FROM playground WHERE type = 'slide';
SELECT * FROM playground;
```

4. modify table
```SQL
ALTER TABLE playground ADD last_maint date;
ALTER TABLE playground DROP last_maint;
```

5. update table
```SQL
UPDATE playground SET color = 'red' WHERE type = 'swing';
```

#Backup and restore
```SQL
pg_dump my_db > test.dump
psql -d my_db -f test.dump
```

#PostGIS
```SQL
SELECT ST_AsText(ST_ClosestPoint(pt,line)) AS cp_pt_line,
	ST_AsText(ST_ClosestPoint(line,pt)) As cp_line_pt
FROM (SELECT 'POINT(100 100)'::geometry As pt,
		'LINESTRING (20 80, 98 190, 110 180, 50 75 )'::geometry As line
	) As foo;
```
`   cp_pt_line   |                cp_line_pt
----------------+------------------------------------------
 POINT(100 100) | POINT(73.0769230769231 115.384615384615)`