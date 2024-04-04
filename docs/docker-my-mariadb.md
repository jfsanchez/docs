# 🧾 MariaDB (docker)

Baseado na imaxe oficial: <https://hub.docker.com/_/mariadb>

~~~~
    docker run -p 9907:3306 -v /root/mariadatosdb:/var/lib/mysql/ --detach \
    --name mariadbpracticasql --env MARIADB_USER=usuariamaria \
    --env MARIADB_PASSWORD=DonaMaria123456 \
    --env MARIADB_ROOT_PASSWORD=EsteNonPodeSerOcontrasinalDEr00t \
    --restart unless-stopped \
    mariadb:latest
~~~~

Para conectar dende o propio docker:

~~~~
    docker exec -it mariadbpracticasql mariadb -uroot -pEsteNonPodeSerOcontrasinalDEr00t
~~~~

Para conectar dende outro host:

- `X.X.X.X` é a IP do servidor ao que queremos conectar.

~~~~
mariadb -hX.X.X.X -P9907 -uroot -pEsteNonPodeSerOcontrasinalDEr00t
~~~~

## Comandos útiles dende consola MySQL/MariaDB

### Crear usuario e conceder permisos a base de datos

~~~~
CREATE USER 'usuario-a-crear'@'%' IDENTIFIED BY 'contrasinal-abc123.';
GRANT ALL PRIVILEGES ON base-de-datos.* TO 'usuario-a-crear'@'%';
FLUSH PRIVILEGES;
~~~~

### Executar un arquivo .sql (útil para recuperar un backup)

~~~~
source /ruta/ao/arquivo.sql
~~~~

## Comando mysqldump para backup (dende shell)

### Backup de todas as BBDD:

~~~~
mysqldump -uUSUARIO -pCLAVE --all-databases > YYYY-mm-dd_mysql_backup.sql
~~~~

### Backup dunha BBDD concreta:

~~~~
mysqldump -uUSUARIO -pCLAVE --databases BASE_DATOS > YYYY-mm-dd_mysql_backup.sql
~~~~

### Conectar a MySQL dende Python

- <https://github.com/jfsanchez/SBD/blob/main/notebooks/bbdd/mysql.ipynb>


⚠️ **AVISO**: Esta configuración NON pretende ser segura, o seu obxectivo é montar de xeito rápido un contorno para a aprendizaxe. Entre outras cousas deberíamos deshabilitar o usuario root para conexións remotas, borrar as BBDD de proba e impredir o acceso directo ao servidor de base de datos.
