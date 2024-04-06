# 🧾 MariaDB (docker)

 - Baseado na imaxe oficial: <https://hub.docker.com/_/mariadb>

~~~~
docker run -p 9907:3306 -v /root/mariadatosdb:/var/lib/mysql/ --detach \
--name mariadbpracticasql --env MARIADB_USER=usuariamaria \
--env MARIADB_PASSWORD=DonaMaria123456 \
--env MARIADB_ROOT_PASSWORD=N0nECl4v3DEr00t \
--restart unless-stopped \
mariadb:latest
~~~~

=== "Cliente no docker"

    ``` bash
    docker exec -it mariadbpracticasql mariadb -uroot -pN0nECl4v3DEr00t
    ```

    - Podemos engadir `-hX.X.X.X` para conectar con outro equipo.

=== "Cliente noutro host"

    ``` bash
    mariadb -hX.X.X.X -P9907 -uroot -pN0nECl4v3DEr00t
    ```

    - `X.X.X.X` é a IP do servidor ao que queremos conectar.

## Comandos útiles dende consola MySQL/MariaDB

- Ver as bases de datos

``` sql
show databases;
```

- Seleccionar unha base de datos

``` sql
use database;
```

- Ver as táboas da BBDD actual seleccionada

``` sql
show tables;
```

- Ver información do estado do servidor

``` sql
\s
```


Saír do cliente

``` sql
\q
```

Tamén funcionaría *quit* ou Crtl+D

### Crear usuario e conceder permisos a base de datos

``` sql
CREATE USER 'usuario-a-crear'@'%' IDENTIFIED BY 'contrasinal-abc123.';
GRANT ALL PRIVILEGES ON base-de-datos.* TO 'usuario-a-crear'@'%';
FLUSH PRIVILEGES;
```

### Executar un arquivo .sql (útil para recuperar un backup)

``` sql
source /ruta/ao/arquivo.sql
```

## Comando mysqldump para backup (dende shell)

=== "Backup dunha BBDD"

    ``` bash
    mysqldump -uUSUARIO -pCLAVE --databases BASE_DATOS > YYYY-mm-dd_mysql_backup.sql
    ```
=== "Backup de todas as BBDD"

    ``` bash
    mysqldump -uUSUARIO -pCLAVE --all-databases > YYYY-mm-dd_mysql_backup.sql
    ```

### Conectar a MySQL dende Python

- <https://github.com/jfsanchez/SBD/blob/main/notebooks/bbdd/mysql.ipynb>


⚠️ **AVISO**: Esta configuración NON pretende ser segura, o seu obxectivo é montar de xeito rápido un contorno para a aprendizaxe. Entre outras cousas deberíamos deshabilitar o usuario root para conexións remotas, borrar as BBDD de proba e impredir o acceso directo ao servidor de base de datos.
