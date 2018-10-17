Step install CoreOS and Migrate MySQL
====
----
Table of Content
================
- [0. change user in database login allow any host [ '%' ]](#0-change-user-in-database-login-allow-any-host--------)
- [1. install CoreOS](#1-install-coreos)
  * [download iso : https://stable.release.core-os.net/amd64-usr/current/coreos_production_iso_image.iso](#download-iso---https---stablereleasecore-osnet-amd64-usr-current-coreos-production-iso-imageiso)
  * [after boot](#after-boot)
  * [edit file cloud_config.yml](#edit-file-cloud-configyml)
  * [in file cloud_config.yml](#in-file-cloud-configyml)
  * [install CoreOS](#install-coreos)
- [2. install docker-compose](#2-install-docker-compose)
- [3. copy /var/lib/mysql from db old host to CoreOS](#3-copy--var-lib-mysql-from-db-old-host-to-coreos)
  * [in old host](#in-old-host)
  * [in new host](#in-new-host)
- [4. remove ib log file](#4-remove-ib-log-file)
- [5. create docker-compose.yml](#5-create-docker-composeyml)
  * [example docker-compose.yml](#example-docker-composeyml)
- [6. start with docker-compose up -d](#6-start-with-docker-compose-up--d)
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
#cloud-config
users:
    - name: username
      passwd: TEXT IN THIS FILE
      groups:
        - sudo 
        - docker
hostname: mysql-newhost
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
## in old host
stop service mysql
```sh
service mysql stop
```
cd to /var/lib/
```sh
cd /var/lib/
```
tar file
```sh
tar -zcvf mysql_backup.tar.gz mysql
```
send file to core os host
```sh
scp mysql_backup.tar.gz username@IPNEWHOST:PATH TO SAVEFILE (Ex. /home/username/)
```
## in new host
un tar file
```sh
tar -zxvf mysql_backup.tar.gz
```
change name folder
```sh
cp -rf mysql mysql_server1
```
# 4. remove ib log file
```sh
rm mysql_server1/ib_logfile0;
rm mysql_server1/ib_logfile1;
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
```
# 6. start with docker-compose up -d
```sh
docker-compose up -d
```

> **Test Connection** 
```sh
mysql -h IPNEWSERVER -P port -u root -pPASSWORD 
```
