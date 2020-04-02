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
```
### open redisinsights
http://localhost:8001
*  add a redis database
Click on "ADD REDIS DATABASE"
![adding redisDB](images/redisinsightAddDB.png)
###
