# MongoDB (docker)

## Crear docker:

~~~~
docker run --name mongo 
-e MONGO_INITDB_ROOT_USERNAME=mongoadmin 
-e MONGO_INITDB_ROOT_PASSWORD=abc123Secret 
-v /root/mongo:/data/db 
-p 27017:27017 
mongo
~~~~

#### Máis información
  
  - <https://hub.docker.com/_/mongo>
