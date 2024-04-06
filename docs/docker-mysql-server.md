# 🧾 MySQL (docker)

 - Baseado na imaxe oficial: <https://hub.docker.com/_/mysql>

Empregaremos o directorio */root/mysqldb* da nosa máquina real para gardar a BBDD.

Faremos uso do porto 9906 no anfitrión porque en ocasións bloquéase por seguridade o porto por defecto 3306 e non se pode abrir.

O contrasinal do usuario root será: *abc123.* e terá permisos para conectar dende calquer host (perigoso e inseguro).

~~~~
docker run -p 9906:3306 --name mysqlpracticoso \
 -v /root/mysqldb:/var/lib/mysql \
 -e MYSQL_ROOT_PASSWORD=abc123. \
 -d mysql:8
~~~~

## Conectar ao servidor MySQL dende o propio docker

Non temos que especificar o porto 9906, xa que é unha redirección no propio anfitrión.

~~~~
docker exec -it mysqlpracticoso mysql -hlocalhost -uroot -pabc123.
~~~~

## Conectar al servidor MySQL del docker desde el anfitrión

Instalar o paquete mariadb (dependendo da distro pode ser que exista mariadb ou mysql como alias do comando do cliente)

~~~~
sudo apt install mariadb-client
~~~~

Averiguar o enderezo IP do contedor

~~~~
docker inspect mysqlpracticoso|grep IPAddress
~~~~

Conectar á IP (neste exemplo a IP é: 172.17.0.2, pero pode ser diferente no teu caso)

~~~~
mysql -h172.17.0.2 -uroot -pabc123.
~~~~

## Conectar ao servidor MySQL dende outro equipo

Ao ter executado o docker coa opción -p 9906:3306 temos mapeado automáticamente o porto 9906 á nosa máquina real, apuntando adentro do docker ao porto por defecto de MySQL: 3306.

~~~~
mysql -hlocalhost -P9906 -uroot -pabc123.
~~~~

Se queremos conectar dende DBeaver na nosa máquina local e temos instalado o contedor de MySQL nunha máquina remota, tampouco debemos esquecer configurar o porto:


![Configuración DBeaver](images/mysql-server-docker/dbeaver.png "Opciones de conexión en DBeaver")

## Comandos útiles dende consola MySQL/MariaDB

- Ver as bases de datos

~~~~
show databases;
~~~~

- Seleccionar unha base de datos

~~~~
use database;
~~~~

- Ver as táboas da BBDD actual seleccionada

~~~~
show tables;
~~~~

- Ver información do estado do servidor

~~~~
\s
~~~~


Saír do cliente

~~~~
\q
~~~~

Tamén funcionaría *quit* ou Crtl+D

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
