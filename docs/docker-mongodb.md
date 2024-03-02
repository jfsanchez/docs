# 🧾 MongoDB (docker)

## Crear docker:

~~~~
docker run --name mongo \
-e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
-e MONGO_INITDB_ROOT_PASSWORD=abc123Secret \
-v /root/mongo:/data/db \
-p 27017:27017 \
mongo
~~~~

## Atlas

### Instalar cliente Atlas

~~~~
sudo apt install mongodb-atlas-cli
~~~~

### Iniciar sesión

~~~~
atlas auth login
~~~~

### Conectarse a atlas desde mongosh

~~~~
mongosh "mongodb+srv://.../" --apiVersion 1 --username USUARIO
~~~~


#### Máis información
  
  - <https://hub.docker.com/_/mongo>
