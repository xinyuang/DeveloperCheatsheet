# mySQL

MySQL:  
`show databases;`  
`show tables;`  
`create database testDB;`  
`use testDB;`

`CREATE DATABASE testDB`

`CREATE USER 'test'@'localhost' IDENTIFIED BY 'mypass';`

`USE testDB`  
Once you are logged in to mysql, grant privileges to the user test \(remember to change password\)

`GRANT ALL privileges on testDB.* to test@localhost identified by 'mypass';`

`sql> EXECUTE sp_set_database_firewall_rule N'My Firewall Rule', '40.78.7.138', '40.78.7.138';`

```text
sql> CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
sql> ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
sql> ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
sql> DENY SELECT ON SalesLT.Address TO ApplicationUser;
```

## login

`mysql -u <username> -p`

## set new password

`set password = password('yourpw');` `flush privileges;`

## creat&show&midify data

`create database <yourdb>;`  
`use <yourdb>;`  
`create table <yourtb>;`  
`drop table if exists <yourtb>;`  
`rename table <yourtb> to <newtb>;`  
`alter table <yourtb> add column <command>;`  
`alter table <yourtb> drop <command>;`  
`alter table <yourtb> change <command>;`  
`ALTER TABLE <yourtb> ALTER COLUMN <yourcolumn> SET DEFAULT 0;`

## modify table

`update <yourtb> set <column1=''> where <column2=''>;`  
`delete from <yourtb> where <column1=''>;`  
`select*(column) from <yourtb> where <column1=''>;`  
`insert into <yourtb> (colomn1,column2) values ('a','b');`

## excute multiple command in a sql file

`touch <cmds.sql>;`  
`source <cmds.sql>;`

## export file using terminal

`mysql -u <username> -p -e "<command;>" > <yourfile.txt>`

## simple example for creating a table

`create table db_album.Album ( albumid INT NOT NULL AUTO_INCREMENT, PRIMARY KEY (albumid), title VARCHAR(50) NOT NULL, lastupdated TIMESTAMP null on update current_timestamp, created TIMESTAMP default current_timestamp, username VARCHAR(20) NOT NULL, FOREIGN KEY (username) REFERENCES User (username) on delete cascade)ENGINE=InnoDB;`

## config mySQL

`set innodb_lock_wait_timeout=30;`  
`SET GLOBAL sql_mode = 'NO_ENGINE_SUBSTITUTION';`  
`SET FOREIGN_KEY_CHECKS=0;`  
`show processlist;`

## query mySQL

`select sum(Totaltime) as ttl_time,avg(speed) as spd from demo_table where name like '%tom%';`  
`select * from testTable limit 10;`  
`select empno,ename,sal,sal*12 from emp order by sal*12 desc;`  
`select avg(sal) from emp;`  
`select distinct title, deptno from emp;`  
`select worker,deptname from emp,dept where emp.deptno=dept.deptno;`  
`select b.ename, e.ename from emp b, emp e where b.empno = e.mgr;`  
edit SQL inquiry: `ed`  
execute last SQL inquiry: `/`  
add alias to sal_12:  
\`select empno,ename, deptno, sal,sal_12 annlsal from emp order by deptno, annlsal desc;`inquiry interval:`select  _from emp where sal between 1000 and 2000;```SELECT users.name AS user, products.name AS favorite FROM users INNER JOIN products ON users.fav = products.id``  
`UPDATE customers SET address = %s WHERE address = %s`_  


_Inner join table_

```sql
select from tableA a inner join tableB b on a.common = b.common inner join 
TableC c on b.common = c.common
```

Create procedure

```sql
mysql> delimiter //

mysql> CREATE PROCEDURE citycount (IN country CHAR(3), OUT cities INT)
       BEGIN
         SELECT COUNT(*) INTO cities FROM world.city
         WHERE CountryCode = country;
       END//
Query OK, 0 rows affected (0.01 sec)

mysql> delimiter ;

mysql> CALL citycount('JPN', @cities); -- cities in Japan
Query OK, 1 row affected (0.00 sec)

mysql> SELECT @cities;
+---------+
| @cities |
+---------+
|     248 |
+---------+
1 row in set (0.00 sec)
```



