# 🧾 Redis

 - Baseado na imaxe oficial: <https://hub.docker.com/r/redis/redis-stack-server>

Creamos o volume para persisitir os datos (**opcional**):

``` bash
docker create volume redis-data
```

### Creamos o docker:

``` bash
docker run -d --name redis-stack \
    -v redis-data:/data \ # (1)!
    -e REDIS_ARGS="--requirepass 123quetal123" \ # (2)!
    -p 6379:6379 -p 8001:8001 \
    redis/redis-stack:latest
```

1.  Nome do volume, no caso que queiramos persistir os datos.
2.  Contrasinal.

Copia de aquí o comando se tes problemas coas novas liñas:

``` bash
docker run -d --name redis-stack -v redis-data:/data -e REDIS_ARGS="--requirepass 123quetal123" -p 6379:6379 -p 8001:8001 redis/redis-stack:latest
```

### (CLI) Conectando a redis dende o propio docker:

``` bash
docker exec -it redis-stack-server \
    redis-cli -h localhost -p 6379 -a 123quetal123
```

Se non especificamos o contrasinal con `-a 123quetal123` en liña de comandos, para autenticarnos deberemos poñer o comando: `AUTH 123quetal123` dentro do cliente de redis.

Dentro da consola de texto de redis, creamos un usuario:

```
acl setuser usuarioredis >contrasinal123inseguro on allchannels allkeys +get +set +del +info +scan
```

``` bash
docker exec -it redis-stack-server \
    redis-cli --user usuarioredis --pass contrasinal123inseguro
```

### (GUI) Conexión contra redisinsight

Se empregaches a configuración de docker de enriba, só tes que conectar á IP da máquina do docker ao porto 8081. Por exemplo: <http://localhost:8081>. Inicia sesión co usuario `usuarioredis` e o contrasinal `contrasinal123inseguro`.

## Outras variables de contorno do docker

Segundo a documentación da imaxe oficial, temos acceso a modificar as seguintes variables de contorno:

- **REDIS_ARGS**: Redis
- **REDISEARCH_ARGS**: RediSearch
- **REDISJSON_ARGS**: RedisJSON
- **REDISGRAPH_ARGS**: RedisGraph
- **REDISTIMESERIES_ARGS**: RedisTimeSeries
- **REDISBLOOM_ARGS**: RedisBloom

Por exemplo, se quixéramos persistir os datos no **RedisTimeSeries**, podemos engadir unha variable de contorno á creación do docker:

`-e REDISTIMESERIES_ARGS="RETENTION_POLICY=20"`

## Comandos útiles:

 - **Autenticarse**: `AUTH contrasinal`
 - **Probar se estamos conectados**: `PING`
 - **Almacenar unha clave (KEY-VALUE)**: `set clave valor`
 - **Recuperar unha clave**: `get clave`
 - **Establecer ou mudar o contrasinal**: `config set requirepass 123quetal123`
 - **Crear un usuario**: `acl setuser ...`
 - **Pedir clave no CLI**: `config set requirepass 123quetal123`

## Uso con Python

**Instalar** as librarías con conda para poder conectar a redis:

``` bash
!conda install -y -c conda-forge redis-py sqlalchemy
```

**Código** de python:

``` python title="redis.py"
#from redis import Redis
empregamos_docker=True

import redis

data = {
    'dog': {
        'scientific-name' : 'Canis familiaris'
    }
}

r = redis.Redis(password="123quetal123")
#r.auth("123quetal123")
r.ping()
#Non instalado no escenario a extensión JSON

if (empregamos_docker):
    r.json().set('doc', '$', data)
    doc = r.json().get('doc', '$')
    dog = r.json().get('doc', '$.dog')
    scientific_name = r.json().get('doc', '$..scientific-name')
    print(scientific_name)
```

**Podes descargar o notebook de**:

- <https://github.com/jfsanchez/SBD/blob/main/notebooks/bbdd/redis.ipynb>

