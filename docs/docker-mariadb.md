# 🧾 MariaDB (docker)

Basado en la imagen oficial: <https://hub.docker.com/_/mariadb>

~~~~
    docker run -p 9907:3306 -v /root/mariadatosdb:/var/lib/mysql/ --detach \
    --name mariadblapractica --env MARIADB_USER=usuariamaria \
    --env MARIADB_PASSWORD=donamaria123456 \
    --env MARIADB_ROOT_PASSWORD=estanopuedeserla2clavederoot \
    mariadb:latest
~~~~

Para conectar desde otro host:

- `X.X.X.X` es la IP del servidor al que queremos conectar.

~~~~
mysql -hX.X.X.X -P9907 -uroot -pestanopuedeserla2clavederoot
~~~~

⚠️ **AVISO**: Esta configuración NO pretende ser segura, su objetivo es montar de forma rápida un entorno para aprendizaje. De hecho debería deshabilitarse el usuario root en remoto, borrarse las BBDD de prueba e impedir el acceso directo a la base de datos entre otros.
