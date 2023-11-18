# Instalar MariaDB en docker

Basado en la imagen oficial: <https://hub.docker.com/_/mariadb>

~~~~
docker run -p 9907:3306 --detach --name mariadbpractica --env MARIADB_ROOT_PASSWORD=abc123. mariadb:latest
~~~~
