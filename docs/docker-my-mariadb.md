# 🧾 MariaDB (docker)

 - Baseado na imaxe oficial: <https://hub.docker.com/_/mariadb>

``` bash
docker run -p 9907:3306 -v /root/mariadatosdb:/var/lib/mysql/ --detach \ # (1)!
--name mariadbpracticasql --env MARIADB_USER=usuariamaria \
--env MARIADB_PASSWORD=DonaMaria123456 \ # (2)!
--env MARIADB_ROOT_PASSWORD=N0nECl4v3DEr00t \
--restart unless-stopped \ # (3)!
mariadb:latest
```

1.  `-v` para asociar o directorio local `/root/mariadatosdb` ao contedor en `/var/lib/mysql`.
2.  `--env` ou `-e` para definir variables de entorno (configuración) presentes na imaxe.
3.  Para que inicie automáticamente o contedor tras un reinicio de docker ou da máquina.

=== "Conectar co cliente do docker"

    ``` bash
    docker exec -it mariadbpracticasql mariadb -uroot -pN0nECl4v3DEr00t
    ```

    - Podemos engadir `-hX.X.X.X` para conectar con outro equipo.

=== "Conectar cun cliente doutro host"

    ``` bash
    mariadb -hX.X.X.X -P9907 -uroot -pN0nECl4v3DEr00t
    ```

    - `X.X.X.X` é a IP do servidor ao que queremos conectar.

## Comandos útiles dende consola MySQL/MariaDB

### Comandos simples

- Ver as bases de datos `show databases;`
- Seleccionar unha base de datos `use database;`
- Ver as táboas da BBDD actual seleccionada: `show tables;`
- Ver información do estado do servidor: `\s`
- Saír do cliente: `\q`. Tamén funcionaría *quit* ou Crtl+D

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
### Importar unha base de datos de proba

Estas instruccións son para un servidor MySQL executándose dentro doutra máquina (real ou virtual) en caso que instalases MySQL ou MariaDB nun docker, estas instruccións debes adaptalas. Probablemente se tes instalado con dockers seríache máis cómodo facer a importación con DBeaver.

1. Descargar a BD employees: <https://github.com/datacharmer/test_db/releases/tag/v1.0.7>

2. Copiar a BBDD ao servidor que teñamos montado (neste exemplo copiamos de local ao servidor con scp, se instalaches con docker o comando cambiará):

    ``` bash
    scp -i chave-ssh.key employees_db-full-1.0.7.tar.gz usuario@IP-DO-SERVIDOR:/tmp/
    ```

3. Conectamos co servidor (neste exemplo conectamos por SSH, pero si tes montado un docker, pode que precises engadir/mudar os comandos)

    ``` bash
    ssh -i chave-ssh.key usuario@IP-DO-SERVIDOR
	sudo su -
	cd /tmp
	tar -xvjf employees_db-full-1.0.7.tar.gz
	mysql -h localhost
		source /tmp/employees_db/employees.sql
		show tables;
    ```

Se estás a traballar coa versión: employees_db-full-1.0.6.tar.bz2 pode ser que teñas algún problema co **engine**. Neste caso, engadir "default_" diante das dúas liñas en employees.sql axuda. Fonte: [stackoverflow](https://stackoverflow.com/questions/36322903/error-1193-when-following-employees-database-install-tutorial-with-mysql-5-7-1).

**Webgrafía**:

- <https://github.com/datacharmer/test_db>
- <https://dev.mysql.com/doc/employee/en/employees-installation.html>

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


⚠️ **AVISO**: Esta configuración NON pretende ser segura, o seu obxectivo é montar de xeito rápido un contorno para a aprendizaxe. Entre outras cousas deberíamos deshabilitar o usuario root para conexións remotas, borrar as BBDD de proba e impedir o acceso directo ao servidor de base de datos.

## Adicional: Porto aberto?

En GNU/Linux podes ver qué portos están abertos con:

``` bash
netstat -atun
```

En docker podes ver as redireccións de portos don `docker inspect`.

Se ves que non tes aberto o 9906 no anfitrión (no caso de docker) ou o 3306 onde teñas MySQL, probablemente debas cambiar o bind-address na configuración de MySQL oy MariaDB.

Edita o arquivo correspondente (en MySQL: /etc/mysql/mysql.conf.d/mysqld.cnf) e mete ou descomenta esta liña:

```
bind-address por 0.0.0.0
```

Ollo! Si é que non conectas ao porto 3306 pero o ves aberto, moi probablemente estea filtrado no firewall (na computación na nube ás veces filtran ese porto aínda que tí o abras explícitamente).