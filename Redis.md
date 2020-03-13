https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04
https://community.pivotal.io/s/article/How-to-Backup-and-Restore-Open-Source-Redis
https://redis-py.readthedocs.io/en/latest/  
https://redis.io/commands  

`config set requirepass p@ss$12E45`  
`sudo systemctl enable redis-server.service`  
`sudo systemctl restart redis-server.service`  
`config get dir `  

`keys '*'`  
`del key`  
`save`  
`auth <password>`  

For sorted set  
`zrange 'key'  start_idx end_idx`  
`zcard 'key'`  count the number of member  


`GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269`  
`georadius 'location' -118.218 33.8626 5 km`  
