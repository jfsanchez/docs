# Instalar MySQL en docker

Basado en: <https://hub.docker.com/_/mysql>

1. Emplearemos el directorio /root/mysqldb para guardar la BBDD.
2. La contraseña será: abc123.

~~~~
docker run -p 3306:3306 --name mysqlpracticoso -v /root/mysqldb:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=abc123. -d mysql:8
~~~~

## Conectar al servidor MySQL desde el propio docker

~~~~
docker exec -it mysqlpracticoso mysql -hlocalhost -uroot -pabc123.
~~~~

## Conectar al servidor MySQL del docker desde el anfitrión

Instalar el paquete mariadb (idealmente debería ser mysql)

~~~~
sudo apt install mariadb-client
~~~~

Averiguar la IP del contenedor

~~~~
docker inspect mysqlpracticoso|grep IPAddress
~~~~

Conectar a la IP (imaginemos que nos ha dicho que es: 172.17.0.2)

~~~~
mysql -h172.17.0.2 -uroot -pabc123.
~~~~

## Conectar al servidor MySQL desde otro equipo

Al haber ejecutado el docker con la opción -p 3306:3306 hemos mapeado automáticamente ese puerto a nuestra máquina real.


