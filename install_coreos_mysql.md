Step install CoreOS and Migrate MySQL
====
----
# 0. change user in database login allow any host [ '%' ]
# 1. install CoreOS
## download iso : https://stable.release.core-os.net/amd64-usr/current/coreos_production_iso_image.iso
## after boot
```sh
sudo openssl passwd -1 > cloud_config.yml
```
## edit file cloud_config.yml
```sh
vi cloud_config.yml
```
## in file cloud_config.yml
```vi
users:
    - name: username
      passwd: TEXT IN THIS FILE
      groups:
        - sudo 
        - docker
hostname: mysql-nursing
```
## install CoreOS
```sh
sudo coreos-install -d /dev/sda -C stable -c cloud_config.yml
```
# 2. install docker-compose     
```sh
sudo su -
mkdir -p /opt/bin
curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m`  > /opt/bin/docker-compose
chmod +x /opt/bin/docker-compose
```
# 3. copy /var/lib/mysql from db old host to CoreOS
# 4. remove `ib_logfile0` and `ib_logfile1`
```sh
rm ib_logfile0;
rm ib_logfile1;
```
# 5. create docker-compose.yml
> (if root is setting allow any host remove enviroment setting in file docker-compose.yml or comment by # )
## example docker-compose.yml
```yaml
mysql_server_01:
  image: mysql:5.6
  container_name: mysql-server-01
  restart: always
  environment:
    - MYSQL_ROOT_HOST=%
    - MYSQL_ROOT_PASSWORD=PASSWORD
  ports:
    - "3308:3306"
  volumes:
    - "/home/iserl/mysql_server1:/var/lib/mysql"
mysql_server_02:
  image: mysql:5.6
  container_name: mysql-server-02
  restart: always
  environment:
    - MYSQL_ROOT_HOST=%
    - MYSQL_ROOT_PASSWORD=PASSWORD
  ports:
    - "3310:3306"
  volumes:
    - "/home/iserl/mysql_server2:/var/lib/mysql"
mysql_server_03:
  image: mysql:5.6
  container_name: mysql-server-03
  restart: always
  environment:
    - MYSQL_ROOT_HOST=%
    - MYSQL_ROOT_PASSWORD=PASSWORD
  ports:
    - "3312:3306"
  volumes:
    - "/home/iserl/mysql_server3:/var/lib/mysql"
```
# 5. start with docker-compose up -d
```sh
docker-compose up -d
```
