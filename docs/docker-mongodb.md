# 🧾 MongoDB (docker)

 - Baseado na imaxe oficial: <https://hub.docker.com/_/mongo>

## Crear docker (servidor local):

``` bash
docker run --name mongo \
  -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
  -e MONGO_INITDB_ROOT_PASSWORD=abc123Secret \
  -v /root/mongo:/data/db \
  -p 27017:27017 \
  mongo
```

## Atlas (servidor na nube)

### Conectarse a atlas dende mongosh

Debe instalarse MongoDB como indica a documentación: engadir a chave, o repo...

``` bash
mongosh "mongodb+srv://.../" --apiVersion 1 --username USUARIO
```

### Instalar cliente Atlas

``` bash
sudo apt install mongodb-atlas-cli
```

### Iniciar sesión

``` bash
atlas auth login
```

### Conectar a mongo dende Python

- <https://github.com/jfsanchez/SBD/blob/main/notebooks/bbdd/mongodb.ipynb>
