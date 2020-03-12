https://redis-py.readthedocs.io/en/latest/  
https://redis.io/commands  

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
