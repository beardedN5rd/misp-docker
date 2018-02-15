MISP Docker
===========

The files in this repository are used to create a Docker container running a [MISP](http://www.misp-project.org) ("Malware Information Sharing Platform") instance.

The MISP container needs at least a MySQL container to store the data. By default it listen to port 80. I highly recommend to serve it behind a NGinx or Apache reverse proxy.

The build is based on Ubuntu and will install all the required components. The following configuration steps are performed automatically:
* Reconfiguration of the base URL in `config.php`
* Generation of a new salt in `config.php`
* Generation of a self-signed certificate
* Optimization of the PHP environment (php.ini) to match the MISP recommended values
* Creation of the MySQL database
* Generation of the admin PGP key

# Building the MISP-image

```
# git clone https://github.com/xme/misp-docker
# cd misp-docker
# docker build -t misp .
```

# Running MISP

```
# git clone https://github.com/xme/misp-docker
# cd misp-docker
# docker-compose up
```
The containers should be build and started and MISP made available at 'http://localhost:8080'
For use apart from testing change the docker-compose.yml to your needs, especially credentials.

## Hints for running manually

You may have to set the access rights for mysql. Information can be found at https://hub.docker.com/r/mysql/mysql-server/.

Assumed the container is named "misp-db" and MYSQL_ROOT_PASSWORD is set to "Dy23qBhjZD6fJkvU",
access can be adjusted the following way:

```
# docker exec -it misp-db mysql -uroot -pDy23qBhjZD6fJkvU
mysql> USE mysql;
mysql> UPDATE user SET host='%' WHERE host='localhost';
mysql> FLUSH PRIVILEGES;
```
