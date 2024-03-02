# 🧾 MongoDB (docker)

## Crear docker (servidor local):

~~~~
docker run --name mongo \
-e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
-e MONGO_INITDB_ROOT_PASSWORD=abc123Secret \
-v /root/mongo:/data/db \
-p 27017:27017 \
mongo
~~~~

Baseado na imaxe oficial: <https://hub.docker.com/_/mongo>


## Atlas (servidor na nube)

### Conectarse a atlas dende mongosh

Debe instalarse MongoDB como indica a documentación: engadir a chave, o repo...

~~~~
mongosh "mongodb+srv://.../" --apiVersion 1 --username USUARIO
~~~~

### Instalar cliente Atlas

~~~~
sudo apt install mongodb-atlas-cli
~~~~

### Iniciar sesión

~~~~
atlas auth login
~~~~

### Conectar a mongo dende Python

- <https://github.com/jfsanchez/SBD/blob/main/notebooks/bbdd/mongodb.ipynb>
