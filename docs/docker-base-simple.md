# 🧾 Instalar docker en Debian

Basado en: <https://docs.docker.com/engine/install/debian/>

Requiere un usuario con permisos sudo.

1. Crear la máquina en AWS / GCloud / Azure / CESGA Cloud y conectarse a ella por SSH. Elegir una distribución debian.

2. Actualizar hasta el final la máquina

~~~~
sudo apt update
sudo apt -y dist-upgrade
sudo apt -y install curl
~~~~

3. Ejecutar el script recomendado por docker

~~~~
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
~~~~

4. Añadir tu usuario al grupo docker (para evitar emplear sudo)

~~~~
sudo usermod -a -G docker $USER
~~~~

5. Salir de la sesión y volver a abrirla (o abrir una sesión sobre la actual como se indica a continuación)

~~~~
sudo su - $USER
~~~~

6. Probar docker
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
