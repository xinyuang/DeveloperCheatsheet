MySQL:   
`show databases;`  
`show tables;`  
`create database testDB;`  
`use testDB;`  

  
#login  
`mysql -u <username> -p`

#set new password  
`set password = password('yourpw');`
`flush privileges;`

#creat&show&midify data  
`create database <yourdb>;`
`use <yourdb>;`
`create table <yourtb>;`
`drop table if exists <yourtb>;`
`rename table <yourtb> to <newtb>;`
`alter table <yourtb> add (column) <command>;`
`alter table <yourtb> drop <command>;`
`alter table <yourtb> change <command>;`  
#modify table  
`update <yourtb> set <column1=''> where <column2=''>;`
`delete from <yourtb> where <column1=''>;`
`select*(column) from <yourtb> where <column1=''>;`
`insert into <yourtb> (colomn1,column2) values ('a','b');`



#excute multiple command in a sql file  
`touch <cmds.sql>;`
`source <cmds.sql>;`

#export file using terminal  
`mysql -u <username> -p -e "<command;>" > <yourfile.txt>`

#simple example for creating a table  
`create table db_album.Album ( 
albumid INT NOT NULL AUTO_INCREMENT, 
PRIMARY KEY (albumid), 
title VARCHAR(50) NOT NULL,
lastupdated TIMESTAMP null on update current_timestamp,
created TIMESTAMP default current_timestamp,
username VARCHAR(20) NOT NULL,
FOREIGN KEY (username) 
REFERENCES User (username) 
on delete cascade)ENGINE=InnoDB;`  

# config mySQL
`set innodb_lock_wait_timeout=30;`  
`SET GLOBAL sql_mode = 'NO_ENGINE_SUBSTITUTION';`  
`SET FOREIGN_KEY_CHECKS=0;`  
`show processlist;`

# query mySQL  
`select sum(Totaltime) as ttl_time,avg(speed) as spd from demo_table where name like '%tom%';`  
`select * from testTable limit 10;`  
`select empno,ename,sal,sal*12 from emp order by sal*12 desc;`  
`select avg(sal) from emp;`  
`select distinct title, deptno from emp;`  
`select worker,deptname from emp,dept where emp.deptno=dept.deptno;`   
`select b.ename, e.ename from emp b, emp e where b.empno = e.mgr;`  
edit SQL inquiry: `ed`  
execute last SQL inquiry: `/`  
add alias to sal*12:   
`select empno,ename, deptno, sal,sal*12 annlsal from emp order by deptno, annlsal desc;`  
inquiry interval:  
`select * from emp where sal between 1000 and 2000;`  
`SELECT users.name AS user, products.name AS favorite FROM users INNER JOIN products ON users.fav = products.id`  
`UPDATE customers SET address = %s WHERE address = %s`  
`  
select *  
from  
    tableA a  
        inner join  
    tableB b  
        on a.common = b.common  
        inner join   
    TableC c  
        on b.common = c.common`  