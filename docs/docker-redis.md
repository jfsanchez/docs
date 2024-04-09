# 🧾 Redis (docker)

 - Baseado na imaxe oficial: <https://hub.docker.com/r/redis/redis-stack-server>

Se non queremos poñer contrasinal:

``` bash
docker run -d --name redis-stack-server \
    -p 6379:6379 redis/redis-stack-server:latest
```

Con contrasinal:

``` bash
docker run -d --name redis-stack-server -p 6379:6379 \
    -e REDIS_ARGS="--requirepass 123quetal123" \
    redis/redis-stack-server:latest
```

Para conectarse dende docker co cliente por defecto:

Sen contrasinal:
``` bash
docker exec -it redis-stack-server redis-cli
```

Despois, para autenticarnos (se temos configurado contrasinal):

~~~~
AUTH 123quetal123
~~~~

Directamente co contrasinal na liña de comandos (inseguro):

``` bash
docker exec -it redis-stack-server \
    redis-cli -h localhost -p 6379 -a 123quetal123
```

## Comandos útiles:

 - **Autenticarse**: *AUTH contrasinal*
 - **Probar se estamos conectados**: *PING*
 - **Almacenar unha clave (KEY-VALUE)**: *set clave valor*
 - **Recuperar unha clave**: *get clave*
 - **Establecer ou mudar o contrasinal**: *config set requirepass 123quetal123*

## Máis información para uso con Python:

- <https://github.com/jfsanchez/SBD/blob/main/notebooks/bbdd/redis.ipynb>

