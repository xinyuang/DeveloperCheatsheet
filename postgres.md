# Postgres

## initial

```sql
CREATE DATABASE dbname;
CREATE USER usr1 WITH PASSWORD 'password';
GRANT ALL ON DATABASE test TO usr1;
psql -h localhost -U usr1 test;
ALTER USER postgres PASSWORD 'myPassword';
```

```sql
sudo -i -u postgres createdb my_db psql -d my_db
\list
\conninfo
```

## table

1. create table

   ```sql
   CREATE TABLE playground (
    equip_id serial PRIMARY KEY,
    type varchar (50) NOT NULL,
    color varchar (25) NOT NULL,
    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),
    install_date date
   );
   ```

2. table insert

   ```sql
   INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2014-04-28');
   INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2010-08-16');
   ```

3. query table

   ```sql
   DELETE FROM playground WHERE type = 'slide';
   SELECT * FROM playground;
   ```

4. modify table

   ```sql
   ALTER TABLE playground ADD last_maint date;
   ALTER TABLE playground DROP last_maint;
   ```

5. update table

   ```sql
   UPDATE playground SET color = 'red' WHERE type = 'swing';
   ```

## Backup and restore

```sql
pg_dump my_db > test.dump
psql -d my_db -f test.dump
```

## PostGIS

find closest point in a line

```sql
SELECT ST_AsText(ST_ClosestPoint(pt,line)) AS cp_pt_line,
    ST_AsText(ST_ClosestPoint(line,pt)) As cp_line_pt
FROM (SELECT 'POINT(100 100)'::geometry As pt,
        'LINESTRING (20 80, 98 190, 110 180, 50 75 )'::geometry As line
    ) As foo;
```

| cp\_pt\_line | cp\_line\_pt |
| :--- | :--- |
| POINT\(100 100\) | POINT\(73.0769230769231 115.384615384615\)\` |

-- Mark a point as WGS 84 long lat --

```sql
SELECT ST_SetSRID(ST_Point(-123.365556, 48.428611),4326) As wgs84long_lat;
```

`SRID=4326;POINT(-123.365556 48.428611)` --- converting lon lat coords to geography

```sql
ALTER TABLE sometable ADD COLUMN geog geography(POINT,4326);
UPDATE sometable SET geog = ST_GeogFromText('SRID=4326;POINT(' || lon || ' ' || lat || ')');
```

Geometry example - units in planar degrees 4326 is WGS 84 long lat, units are degrees.

```sql
SELECT ST_Distance(
        'SRID=4326;POINT(-72.1235 42.3521)'::geometry,
        'SRID=4326;LINESTRING(-72.1260 42.45, -72.123 42.1546)'::geometry
    );
```

`st_distance 0.00150567726382282`

find closest point on a line to a point or other geometry

```sql
 SELECT ST_AsText(ST_LineInterpolatePoint(foo.the_line, ST_LineLocatePoint(foo.the_line, ST_GeomFromText('POINT(4 3)'))))
FROM (SELECT ST_GeomFromText('LINESTRING(1 2, 4 5, 6 7)') As the_line) As foo;
```

`st_astext POINT(3 4)`

