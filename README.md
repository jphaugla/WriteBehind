# WriteBehind
redis write behind to mysql
this requires two other githubs at the same level as this github
## Initial project setup
Get this github code and other two needed githubs
```bash 
git clone git@github.com:RedisGears/rgsync.git
git clone git@github.com:RedisGears/RedisGears.git
git clone https://github.com/jphaugla/WriteBehind.git
```
## docker compose startup
```bash
docker-compose up -d 
```
### set up MySQL
```bash
docker exec -it mysql bash -c "cat /home/jovyan/RedisGears/recipes/write_behind/mysql/create-user.sql |mysql -prootpassword"
docker exec -it mysql bash -c "cat /home/jovyan/RedisGears/recipes/write_behind/mysql/create-db.sql |mysql -utest -ppasswd"
```
### run the requirements script for redis.py
```bash
docker exec -it jupyter bash -c "pip install -r ./python_src/requirements.txt"
docker exec -it jupyter bash -c "pip install -r ./rgsync/requirements.txt"
```
### run the gears script
```bash
docker exec -it jupyter bash -c "python ./RedisGears/recipes/gears.py --host redis --port 6379 ./rgsync/example.py REQUIREMENTS git+https://github.com/RedisGears/rgsync.git PyMySQL"
### open jupyter
as a security measure, a token must be provided to open jupyter.  
*  get the jupyter container logs and copy the browser address 
```
docker logs jupyter
```
(hint:  look for the phrase "copy and paste one of these URLs:")
*  open jupyter with your token (don't run this command it is an example)
http://127.0.0.1:8888/?token=6bb4cab7ecc40dbbef127491dd93a1ecde1f6f6a0be9ff24
### open redisinsights
http://localhost:8001
*  add a redis database
Click on "ADD REDIS DATABASE"
![adding redisDB](images/redisinsightAddDB.png)
###
