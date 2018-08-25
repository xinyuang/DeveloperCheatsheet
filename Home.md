Linux command:  
    change manage right: `su root`
    define variable: `File = "test.txt"`  
    flush and write to file: `echo -e "content + escape " > $File`   
    continue writing to file: `echo -e "content + escape " >> $File`  

MySQL: 
`show databases;`  
`show tables;`  
`create database testDB;`  
`use testDB;`  
`select * from testTable limit 10;`  

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
