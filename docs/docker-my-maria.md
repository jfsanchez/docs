# 🧾 MySQL / MariaDB
 
 - Baseado nas imaxes oficiais:
     - MySQL: <https://hub.docker.com/_/mysql>
     - MariaDB: <https://hub.docker.com/_/mariadb>

Imos ver e instalar dous sabores deste servidor SQL tan popular.

## MySQL

### Instalación de MySQL

E recomendable crear un volume previamente cun nome para ter localizado onde temos os datos:

``` bash
docker volume create datosmysql
```

E logo crear o contedor asociado a ese volume:

``` bash
docker run -p 9906:3306 --name contedor_mysql \
  -v datosmysql:/var/lib/mysql \ # (1)!
  -e MYSQL_RANDOM_ROOT_PASSWORD=1 \ # (2)!
  -e MYSQL_DATABASE=basededatos \ # (3)!
  -e MYSQL_USER=usuario \ # (4)!
  -e MYSQL_PASSWORD=Contrasinal123. \ # (5)!
  --restart unless-stopped \ # (6)!
  -d mysql:8
```

1.  Volume para os datos.
2.  Contrasinal de root.
3.  Crea a base de datos `basededatos`.
4.  Crea o usuario `usuario` con acceso de superusaurio a `basededatos`.
5.  Establece o contrasinal de `usuario` (é preciso para que se cree o usuario).
6.  Para que inicie automáticamente o contedor tras un reinicio de docker ou da máquina.

Copia de aquí o comando para que non fallen as novas liñas e espacios:

```
docker run -p 9906:3306 --name contedor_mysql -v datosmysql:/var/lib/mysql -e MYSQL_RANDOM_ROOT_PASSWORD=1 -e MYSQL_DATABASE=basededatos -e MYSQL_USER=usuario -e MYSQL_PASSWORD=Contrasinal123. --restart unless-stopped -d mysql:8
```


**Aclaracións**:

- `-p 9906:3306` redirixe o porto `9906` do anfitrión ao porto `3306` do contedor.
- `--env` ou `-e` serven para definir variables de entorno (configuración) presentes na imaxe.
- `-v` permite asociar (montar) un directorio local a un directorio de dentro do contedor. Poderíamos asociado un directorio local `/root/mysqldatos` ao contedor en `/var/lib/mysql` co parámetro: `-v /root/mysqldatos:/var/lib/mysql`. Tamén poderíamos mapear un volume anterior con: `-v ID_DO_VOLUME:/var/lib/mysql`.
- Para saber o contrasinal de root asignado aleatoriamente debemos buscar nos logs unha liña que conteña: "[Note] [Entrypoint]: GENERATED ROOT PASSWORD:":
    ``` bash
    docker logs contedor_mysql
    ```

Por último comprobamos que teñamos correctamente asociado o volume de datos ao noso contedor:

``` bash
docker ps -a --no-trunc --format "{{.Names}}: {{.Mounts}}"
```

### Recuperar instancia de MySQL co seu volume 

Se queremos recuperar unha instancia borrada, sempre e cando non borrásemos o seu volume de datos, anterior (non fai falta especificar usuarios ou contrasinais):

``` bash
docker run -p 9906:3306 --name contedor_mysql \
  -v datosmysql:/var/lib/mysql \
  --restart unless-stopped \
  -d mysql:8
```

### (CLI) Conexión con MySQL

=== "Conectar co cliente do docker"

    ``` bash
    docker exec -it contedor_mysql mysql -uusuario -pContrasinal123.
    ```

    - Podemos engadir `-hX.X.X.X` para conectar con outro equipo.

=== "Conectar cun cliente doutro host"

    ``` bash
    mysql -hX.X.X.X -P9907 -uusuario -pContrasinal123.
    ```

    - `X.X.X.X` é a IP do servidor ao que queremos conectar.


## MariaDB

### Instalación de MariaDB

E recomendable crear un volume previamente cun nome para ter localizado onde temos os datos:

``` bash
docker volume create datosmariadb
```

``` bash
docker run -p 9907:3306 --name contedor_mariadb \
  -v datosmariadb:/var/lib/mysql \ # (1)!
  --env MARIADB_RANDOM_ROOT_PASSWORD=1 \ # (2)!
  --env MARIADB_DATABASE=demaria \ # (3)!
  --env MARIADB_USER=usuariamaria \ # (4)!
  --env MARIADB_PASSWORD=DonaMaria123456 \ # (5)!
  --restart unless-stopped \ # (6)!
  -d mariadb:latest
```

1.  Volume para os datos.
2.  Elexir un contrasinal de root aleatorio.
3.  Crea a base de datos `demaria`.
4.  Crea o usuario `usuariamaria` con acceso de superusaurio a `demaria`.
5.  Establece o contrasinal de `usuariamaria` (é preciso para que se cree o usuario).
6.  Para que inicie automáticamente o contedor tras un reinicio de docker ou da máquina.

Copia de aquí o comando para que non fallen as novas liñas e espacios:

```
docker run -p 9907:3306 --name contedor_mariadb -v datosmariadb:/var/lib/mysql --env MARIADB_RANDOM_ROOT_PASSWORD=1 --env MARIADB_DATABASE=demaria --env MARIADB_USER=usuariamaria --env MARIADB_PASSWORD=DonaMaria123456 --restart unless-stopped -d mariadb:latest
```

**Aclaracións**:

- `-p 9906:3306` redirixe o porto `9906` do anfitrión ao porto `3306` do contedor.
- `--env` ou `-e` serven para definir variables de entorno (configuración) presentes na imaxe.
- `-v` permite asociar (montar) un directorio local a un directorio de dentro do contedor. Poderíamos asociado un directorio local `/root/mariadbdatos` ao contedor en `/var/lib/mysql` co parámetro: `-v /root/mariadbdatos:/var/lib/mysql`
- A imaxe xa executa o script: `/usr/bin/mariadb-secure-installation` que equivale ao `mysql_secure_installation`.
- Para saber o contrasinal de root asignado aleatoriamente debemos buscar nos logs unha liña que conteña: "[Note] [Entrypoint]: GENERATED ROOT PASSWORD:":
    ``` bash
    docker logs contedor_mariadb
    ```
Por último comprobamos que teñamos correctamente asociado o volume de datos ao noso contedor:

``` bash
docker ps -a --no-trunc --format "{{.Names}}: {{.Mounts}}"
```

### Recuperar instancia de MariaDB co seu volume 

Se queremos recuperar unha instancia borrada, sempre e cando non borrásemos o seu volume de datos, anterior (non fai falta especificar usuarios ou contrasinais):

``` bash
docker run -p 9907:3306 --name contedor_mariadb \
  -v datosmariadb:/var/lib/mysql \
  --restart unless-stopped \
  -d mariadb:latest
```


### (CLI) Conexión con MariaDB

=== "Conectar co cliente do docker"

    ``` bash
    docker exec -it contedor_mariadb mariadb -uusuariamaria -pDonaMaria123456
    ```

    - Podemos engadir `-hX.X.X.X` para conectar con outro equipo.

=== "Conectar cun cliente doutro host"

    ``` bash
    mariadb -hX.X.X.X -P9907 -uusuariamaria -pDonaMaria123456
    ```

    - `X.X.X.X` é a IP do servidor ao que queremos conectar.

## (GUI) Conectar a MariaDB/MySQL con DBeaver

Se queremos conectar dende DBeaver na nosa máquina local e temos instalado o contedor de MariaDB/MySQL nunha máquina remota, tampouco debemos esquecer configurar o porto:

![Configuración DBeaver](images/mysql-server-docker/dbeaver.png "Opciones de conexión en DBeaver")

Na lapela Driver properties lembra mudar o valor de **allowPublicKeyRetrieval** a **TRUE** posto que é necesario no caso de empregar cifrado. Segundo a configuración, pode ser necesario.

Podes acceder a un manual máis detallado en [🦫 DBeaver e túneles SSH](https://jfsanchez.es/docs/dbeaver-tunel-ssh/) onde tamén aprenderás como realizar un túnel SSH. Este túnel pode ser necesario si o servidor de base de datos está detrás dun firewall.


## Comandos útiles dende consola MySQL/MariaDB

### Comandos simples

- Ver as bases de datos:
    ``` sql
    show databases;
    ```
- Seleccionar unha base de datos:
     ``` sql
     use database;
     ```
- Ver as táboas da BBDD actual seleccionada:
     ``` sql
     show tables;
     ```
- Ver información do estado do servidor:
    ```\s```
- Saír do cliente. Tamén funcionaría: ```quit``` ou ```Crtl+D```:
    ```\q```

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

Estas instruccións son para un servidor MariaDB/MySQL executándose dentro doutra máquina (real ou virtual) en caso que instalases MariaDB/MySQL nun docker, estas instruccións debes adaptalas. Probablemente se tes instalado con dockers seríache máis cómodo facer a importación con DBeaver.

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

No caso de instalación con docker, se ves que non tes aberto o 9906/9907 (segundo o exemplo) no anfitrión ou o 3306 onde teñas MySQL, probablemente debas cambiar o bind-address na configuración de MySQL oy MariaDB.

Edita o arquivo correspondente (en MySQL: /etc/mysql/mysql.conf.d/mysqld.cnf) e mete ou descomenta esta liña:

```
bind-address por 0.0.0.0
```

Ollo! Si é que non conectas ao porto 3306 pero o ves aberto, moi probablemente estea filtrado no firewall (na computación na nube ás veces filtran ese porto aínda que tí o abras explícitamente).