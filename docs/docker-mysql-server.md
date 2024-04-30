# 🧾 MySQL (docker)

 - Baseado na imaxe oficial: <https://hub.docker.com/_/mysql>

Empregaremos o directorio */root/mysqldb* da nosa máquina real para gardar a BBDD.

Faremos uso do porto 9906 no anfitrión porque en ocasións bloquéase por seguridade o porto por defecto 3306 e non se pode abrir.

O contrasinal do usuario root será: *abc123.* e terá permisos para conectar dende calquer host (perigoso e inseguro).

``` bash
docker run -p 9906:3306 --name mysqlpracticoso \
 -v /root/mysqldb:/var/lib/mysql \
 -e MYSQL_ROOT_PASSWORD=abc123. \
 -d mysql:8
```
## Conectar co servidor MySQL en modo texto (CLI)

=== "Co cliente do docker"
    Non temos que especificar o porto 9906, xa que é unha redirección no propio anfitrión.
    
    ``` bash
    docker exec -it mysqlpracticoso mysql -hlocalhost -uroot -pabc123.
    ```

=== "Dende o anfitrión"
    Instalar o paquete mariadb (dependendo da distro pode ser que exista mariadb ou mysql como alias do comando do cliente)
    
    ``` bash
    sudo apt install mariadb-client
    ```
    
    Averiguar o enderezo IP do contedor
    
    ``` bash
    docker inspect mysqlpracticoso|grep IPAddress
    ```
    
    Conectar á IP (neste exemplo a IP é: 172.17.0.2, pero pode ser diferente no teu caso)

    ``` bash
    mysql -h172.17.0.2 -uroot -pabc123.
    ```

=== "Dende outro equipo"
    
    Ao ter executado o docker coa opción -p 9906:3306 temos mapeado automáticamente o porto 9906 á nosa máquina real, apuntando adentro do docker ao porto por defecto de MySQL: 3306.

    ``` bash
    mysql -hlocalhost -P9906 -uroot -pabc123.
    ```

## Conectar co servidor MySQL en modo gráfico (GUI) con DBeaver

Se queremos conectar dende DBeaver na nosa máquina local e temos instalado o contedor de MySQL nunha máquina remota, tampouco debemos esquecer configurar o porto:


![Configuración DBeaver](images/mysql-server-docker/dbeaver.png "Opciones de conexión en DBeaver")

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

### Backup de todas as BBDD:

``` bash
mysqldump -uUSUARIO -pCLAVE --all-databases > YYYY-mm-dd_mysql_backup.sql
```

### Backup dunha BBDD concreta:

``` bash
mysqldump -uUSUARIO -pCLAVE --databases BASE_DATOS > YYYY-mm-dd_mysql_backup.sql
```

### Conectar a MySQL dende Python

- <https://github.com/jfsanchez/SBD/blob/main/notebooks/bbdd/mysql.ipynb>

⚠️ **AVISO**: Esta configuración NON pretende ser segura, o seu obxectivo é montar de xeito rápido un contorno para a aprendizaxe. Entre outras cousas deberíamos deshabilitar o usuario root para conexións remotas, borrar as BBDD de proba e impredir o acceso directo ao servidor de base de datos.

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