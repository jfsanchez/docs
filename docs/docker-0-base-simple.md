# 🧾 Instalar docker en Debian

Basado en: <https://docs.docker.com/engine/install/debian/>

Requiere un usuario con permisos sudo.

0. Crear la máquina en AWS / GCloud / Azure / CESGA Cloud y conectarse a ella por SSH. Elegir una distribución debian.

1. Actualizar hasta el final la máquina

~~~~
sudo apt update
sudo apt -y dist-upgrade
sudo apt -y install curl
~~~~

2. Ejecutar el script recomendado por docker

~~~~
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
~~~~

3. Añadir tu usuario al grupo docker (para evitar emplear sudo)

~~~~
sudo usermod -a -G docker $USER
~~~~

4. Sair da sesión e volver a abrila (ou abrir unha sesión enriba como se indica)

~~~~
sudo su - $USER
~~~~

4. Probar docker
~~~~
docker run hello-world
~~~~

Imágenes oficiales de docker:

- <https://hub.docker.com/_/mysql>
- <https://hub.docker.com/_/mariadb>
- <https://mariadb.com/kb/en/installing-and-using-mariadb-via-docker/>
- <https://hub.docker.com/_/microsoft-mssql-server>
- <https://hub.docker.com/_/mongo>
- <https://hub.docker.com/_/redis>
- <https://hub.docker.com/_/postgres>
- <https://hub.docker.com/_/cassandra>
