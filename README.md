# WriteBehind
redis write behind to mysql
this requires two other githubs at the same level as this github
## Initial project setup
Get this github code and other two needed githubs
```bash 
git clone --single-branch --branch 0.1 git@github.com:RedisGears/rgsync.git
git clone --single-branch --branch 0.9 git@github.com:RedisGears/RedisGears.git
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
```
### run the gears script
Edit rgsync/example.py for correct connections.
Find this line
```bash
connection = MySqlConnection('demouser', 'Password123!', 'localhost:3306/test')
```
and replace with this line
```bash
connection = MySqlConnection('test', 'passwd!', 'mysql:3306/test')
```
```bash
docker exec -it jupyter bash -c "python ./RedisGears/recipes/gears.py --host redis --port 6379 ./rgsync/example.py REQUIREMENTS git+https://github.com/RedisGears/rgsync.git#0.1 PyMySQL"
```
### NOT WORKING
I ran out of time to look at this and left it with this error
```bash
Traceback (most recent call last):
  File "./RedisGears/recipes/gears.py", line 47, in <module>
    reply = r.execute_command(*q)
  File "/opt/conda/lib/python3.7/site-packages/redis/client.py", line 878, in execute_command
    return self.parse_response(conn, command_name, **options)
  File "/opt/conda/lib/python3.7/site-packages/redis/client.py", line 892, in parse_response
    response = connection.read_response()
  File "/opt/conda/lib/python3.7/site-packages/redis/connection.py", line 752, in read_response
    raise response
redis.exceptions.ResponseError: Could not satisfy requirments
```
