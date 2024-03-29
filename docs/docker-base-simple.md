# 🧾 Instalar docker en Debian

Precisamos un usuario con permisos de administrador ou *sudo*.

## Pasos

1. Crear a máquina en AWS / GCloud / Azure / CESGA Cloud e conectarse a ela por SSH. Elexiremos una distribución debian (recomendado a última estable).

2. Actualizamos ata o final a máquina:

~~~~
sudo apt update
sudo apt -y dist-upgrade
sudo apt -y install curl
~~~~

3. Executamos o script (guión) recomendado pola páxina oficial de docker:

~~~~
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
~~~~

4. Engadimos o noso usuario ao grupo docker (para evitar empregar sudo):

~~~~
sudo usermod -a -G docker $USER
~~~~

5. Saímos da sesión e volvemos abrila (ou abrimos unha sesión sobre a actual como se indica a continuación):

~~~~
sudo su - $USER
~~~~

6. Probamos o docker de exemplo de **hola-mundo**:
~~~~
docker run hello-world
~~~~

## Máis información

- Baseado en: <https://docs.docker.com/engine/install/debian/>

## Imaxes oficiales para docker que podes probar

Nesta páxina tes algunhas configuracións rápidas (exemplos xa feitos) baseados nestas imaxes:

- <https://hub.docker.com/_/mysql>
- <https://hub.docker.com/_/mariadb>
- <https://mariadb.com/kb/en/installing-and-using-mariadb-via-docker/>
- <https://hub.docker.com/_/microsoft-mssql-server>
- <https://hub.docker.com/_/mongo>
- <https://hub.docker.com/_/redis>
- <https://hub.docker.com/_/postgres>
- <https://hub.docker.com/_/cassandra>
