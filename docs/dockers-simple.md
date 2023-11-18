# Instalar docker en debian

Basado en: <https://docs.docker.com/engine/install/debian/>

Requiere un usuario con permisos sudo.

0. Crear la máquina en AWS / GCloud / Azure / CESGA Cloud y conectarse a ella por SSH. Elegir una distribución debian.

1. Actualizar hasta el final la máquina

~~~~
sudo apt update
sudo apt dist-upgrade
~~~~

2. Ejecutar el script recomendado por docker

~~~~
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh --dry-run
~~~~

3. Añadir tu usuario al grupo docker (para evitar emplear sudo)

~~~~
sudo usermod -a -G docker $USER
~~~~

4. Probar docker
~~~~
docker run hello-world
~~~~

Redirigir puertos anfitrión <-> docker: <https://tecadmin.net/forwarding-ports-to-docker-containers-using-linux-firewalls/>