# 🧾 MySQL (docker)

Basado en la imagen oficial: <https://hub.docker.com/_/mysql>

1. Emplearemos el directorio /root/mysqldb de nuestra máquina real para guardar la BBDD.
2. Usaremos el puerto 9906 en el anfitrión porque en ocasiones se bloquea por seguridad el puerto 3306.
3. La contraseña del usuario root será: abc123. y tendrá permisos para conectar desde cualquier host.

~~~~
docker run -p 9906:3306 --name mysqlpracticoso \
 -v /root/mysqldb:/var/lib/mysql \
 -e MYSQL_ROOT_PASSWORD=abc123. \
 -d mysql:8
~~~~

## Conectar al servidor MySQL desde el propio docker

No hay que especificar el puerto 9906, ya que es una redirección en el propio anfitrión.

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

Al haber ejecutado el docker con la opción -p 9906:3306 hemos mapeado automáticamente el puerto 9906 a nuestra máquina real, apuntando dentro del docker al puerto por defecto de MySQL 3306.

~~~~
mysql -hlocalhost -P9906 -uroot -pabc123.
~~~~

Si queremos conectar desde DBeaver en nuestra máquina local y hemos instalado el contenedor de MySQL en una máquina remota, tampoco debemos olvidarnos de configurar el puerto:


![Configuración DBeaver](images/mysql-server-docker/dbeaver.png "Opciones de conexión en DBeaver")

⚠️ **AVISO**: Esta configuración NO pretende ser segura, su objetivo es montar de forma rápida un entorno para aprendizaje.
