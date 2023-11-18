# Docker: La miniguía "re"finitiva

⚠️ ⚠️ ⚠️ Esta guía está desordenada, es el resultado de varios cursos, investigación por mi cuenta e ir apuntando pasos. La idea es ir ordenándola poco a poco. Puede contener errores, repeticiones y no ser exacta.

## Componentes
- **Docker Host**: Máquina que ejecutará docker.
- **Docker CLI**: Cliente con el que conectamos y administramos dockers.
- **Rest API**: Permite administrar docker con peticiones GET/POST.
- **Docker Daemon**: El servidor de docker.

## Contenedores

Un contenedor es una capa donde se ejecutan las imágenes.

Las imágenes son la base del sistema, se las supone stateless (sin estado) y se usan para desplegar varios contenedores. Una imagen es de sólo lectura mientras que el contenedor permite escritura.

**Un contenedor es temporal**. Si queremos guardar los cambios necesitamos un volumen de datos. Además de RAM y procesador, un contenedor puede emplear: Imágenes, volúmenes y redes.

Los volúmenes permiten almacenar información a los contenedores. Esta información no debería almacenarse directamente en el contenedor, ya que de borrarse, se eliminarían los datos.

Un contenedor puede tener acceso a todos los procesadores y RAM del host (por defecto) o se pueden establecer límites.

Pueden crearse varias redes para relacionar unos contenedores con otros. Un docker puede estar en una o varias redes.

## Ejemplo: Lanzar el servidor de BBDD MariaDB oficial
~~~~
    docker run --detach --name maria1 --env MARIADB_USER=unusuario \
    --env MARIADB_PASSWORD=unacontrasena \
    --env MARIADB_ROOT_PASSWORD=clavederoot \
    mariadb:latest
~~~~

## Asociar una carpeta local a una carpeta del contenedor
Por ejemplo: `/tmp/mariadatos` al `/var/lib/mysql` del docker

Este ejemplo crea un contenedor con la última versión de mariadb y por medio de las variables de entorno que permite su imagen, le pasa un usuario, una clave, una clave de administrador y asocia una carpeta de nuestro host dentro del contenedor.

~~~~
    docker run -v /tmp/mariadatos:/var/lib/mysql/ --detach \
    --name maria2 --env MARIADB_USER=unusuario \
    --env MARIADB_PASSWORD=unacontrasena \
    --env MARIADB_ROOT_PASSWORD=clavederoot \
    mariadb:latest
~~~~

Comando `run`:

Descarga una imagen (si no existe) y crea e inicia el contenedor.

Opción `--detach`:

Libera la terminal, no nos deja la consola bloqueada, lo pasa a segundo plano.

Pasar una variable de entorno:

~~~~
    docker exec -e var='value' CONTENEDOR COMANDO
~~~~

Leyenda: -e: Environment.

## Asociar carpeta del contenedor a un volumen
~~~~
    docker run -v VOLUMENNUEVO:/var/lib/mysql/ \
    --detach --name maria3 \
    --env MARIADB_USER=unusuario \
    --env MARIADB_PASSWORD=unacontrasena \
    --env MARIADB_ROOT_PASSWORD=clavederoot \
    mariadb:latest
~~~~

## Volúmenes

Existen tres tipos básicos de volúmenes:

 - Volúmenes Host.
 - Volúmenes anónimos.
 - Volúmenes nombrados.

Por defecto se guardan en: `/var/lib/docker/volumes`

Conviene acceder a ellos sólo con herramientas de docker. <https://docs.docker.com/storage/volumes/>

Comandos útiles (se verán de nuevo en los ejemplos)
~~~~
    docker volume ls
    docker inspect VOLUMEN
~~~~

Por defecto emplearemos el driver `local` para los volúmenes, sin embargo hay otras opciones como flocker o convoy con opciones interesantes. Por ejemplo: Sacar snapshots del volumen de datos.

Se permiten pasar opciones a `volume` para especificar tamaño (o guardar el volumen de forma volátil con tmpfs)

## Nombrado

Para acceder a los contendores o bien podemos referirnos por su nombre (crea un nombre aleatorio si no le especificamos uno con `--name`) o por su `CONTAINER ID`. En el caso del `CONTAINER ID` podemos escribir tan sólo los dos o tres primeros caracteres (siempre que no haya otro contenedor en el que coincidan estos y con ello se pueda inferir inequívocamente el contenedor referenciado).

## Ejemplos de comandos básicos

En CONTENEDOR puede valer el nombre asignado o los primeros dígitos/letras de su identificador ¿Cuantos caracteres hay que poner del identificador? Fácil, tantos como permitan identificarlo de forma única.

### Listar volúmenes:
~~~~
    docker volume ls
~~~~

### Listar imágenes:
~~~~
    docker image ls
~~~~

### Listar contenedores (en ejecución). Opciones:
~~~~
    docker ps
    docker container ls
    docker container ps
~~~~

### Listar contenedores (**en ejecución y finalizados**). Opciones:
~~~~
    docker ps -a
    docker container ls -a
    docker container ps -a
~~~~

### Parar un contenedor:
~~~~
    docker container stop CONTENEDOR
~~~~

### Inciar un contenedor (que esté previamente creado y exista):
~~~~
    docker container start CONTENEDOR
~~~~

### Volúmenes. Creación explícita
~~~~
    docker volume create mi-volumen
~~~~

### Volúmenes. Ver información
~~~~
    docker volume inspect mi-volumen
~~~~

### Asociar un volumen a un contenedor (en la creación del contenedor)
~~~~
    docker run -d \
      --name entornoDES \
      --mount source=mi-volumen,target=/app \
      nginx:latest
~~~~

### Mapear/Redirigir un puerto

Si necesitamos asignar un puerto del anfitrión al contenedor, podemos hacerlo con la opción: `-p PUERTO_ANFITRIÓN:PUERTO_INVITADO`.

~~~~
docker run -it -d -p 3001:3000 imagen/tag
~~~~

### Borrar un contenedor
~~~~
    docker container rm CONTENEDOR
~~~~

### Borrar un volumen
~~~~
    docker volume rm VOLUMEN
~~~~

Al borrar un contenedor, por defecto, no se borra el volumen asociado.

### Borrar una imagen
~~~~
    docker image rm IMAGEN
~~~~

Al borrar un contenedor, por defecto, no se borra la imagen asociada a partir de la cual se ha hecho el contenedor.

Salvo que forcemos el borrado, no se puede borrar una imagen si tiene asociado algún contenedor.

### Ejecutando un comando dentro del contenedor

Una de los casos más típicos es ejecutar un bash para cambiar o consultar algo dentro del contenedor.

Crear el contenedor y que nos devuelva una terminal:

~~~~
    docker run -it ubuntu:18.04 bash
~~~~

* `-it`: nos conecta (attach) a una sesión interactiva de forma que podamos ver e interactuar con ella).
* `run`: Descarga la imagen, crea el contenedor y lo arranca.

Conectarnos a una terminal de un contenedor ya creado:
~~~~
    docker exec -it CONTENEDOR /bin/bash
~~~~

* `exec`: Ejecuta el comando /bin/bash en el CONTENEDOR.

En el caso que no necesitemos una sesión interactiva, podemos ejecutar un comando directamente:

~~~~
    docker exec CONTENEDOR ls
~~~~

La diferencia entre `docker run` y `docker exec` es que `docker exec` ejecuta el comando que le especifiquemos en un contenedor ya creado y en ejecución, mientras que `docker run` crea y ejecuta un contenedor temporal, ejecuta el comando y para el contenedor cuando acaba.


### Bajar la imagen

Este comando baja la imagen sin usarla en ningún contenedor. Con posterioridad podrá ser utilizada.

~~~~
docker pull IMAGEN
~~~~

## Archivo Dockerfile

Ejemplo Dockerfile:

~~~~
FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
ESXPOSE 3000
CMD ["node", "server.js"]
~~~~

### FROM
- Inicia una nueva etapa de construcción:
`FROM <image>[:<tag>][AS <NAME>]`

- `AS <NAME>` nombra la etapa de compilación.
- Puede aparecer varias veces (varias imágenes o indicar dependencia).
- Los tags son opcionales, por defecto `latest`.

Se pueden tener múltiples fases de construcción (multistage). Por ejemplo: La primera baja de un repo, baja dependencias, ejecuta maven/ant/compila y la segunda con el resultado generado (por ejemplo un jar) puede construir una imagen mínima con ese jar y una MV de java para su ejecución.

Es decir, podríamos en este ejemplo tener una máquina para compilar un programa (con todas las dependencias nesarias) y otra con las dependencias mínimas para ejecutar ese programa.

### RUN
Ejecuta un comando en la capa actual. Por defecto se emplea:
- En GNU/Linux: `/bin/sh -c`
- En Microsoft Windows: `cmd /S /C`

### COPY/ADD
`COPY/ADD [--chown=<user>:<group>] <src> ... <dest>`

 - `<src>`: El origen del archivo.
 - `<dest>`: Indica la ruta **dentro** del contenedor.

- GNU/Linux admite parámetro `chown`.

- `ADD` permite copiar un archivo desde una URL. Si src es un archivo comprimido, lo descomprime en destino.

### ENTRYPOINT
El ejecutable del contenedor
Ejemplo: `ENTRYPOINT ["executable","param1","param2"]`

Si luego hay un CMD, se le pasa al entrypoint.

### CMD
Sólo puede haber un CMD en el `Dockerfile`. Si hay más de uno, sólo se aplicará el último.

Indica el comando que levanta el servicio. Debe levantarse en primer plano para mantener vivo el contenedor.

Ejemplo: `CMD ["executable","param1","param2"]`

### ENV
Sirve para indicar variables de entorno con valores por defecto. El usuario puede cambiar estas variables para personalizar los ajustes a su gusto.

### Otros
El archivo `.dockerignore` permite que no se copien al contenedor los archivos y/o directorios especificados como no necesarios para acelerar el proceso de construcción.

## Contenedores
- Variables de entorno: `-e`
- Limitar recursos

Limitar memoria: 
~~~
--memory 200m (200 MiB)
~~~

Limitar al procesador 0 (el primero): 
~~~
--cpuset-cpus 0
~~~

Renombrar un docker
~~~
docker rename 
~~~

Copiar archivos desde/hacia contenedor/host

~~~~
docker cp
~~~~

## Docker logs
Nos va a permitir ver los logs de un contenedor.
- `-f` permite verlos en tiempo real
- `-t` añade el timestamp al log (aunque la aplicación de log no lo haga). Útil para tiempos sincronizados.

## Crear unha imagen a partir de un contenedor


### Construir una imagen

~~~~
docker build -t imagen/tag .
~~~~

### Docker commit
Permite crear una imagen a partir de un contenedor.

### Ejecutar una imagen en un contenedor

~~~~
    docker run -it -d imagen/tag
~~~~

### Imágenes propias

Si creas una cuenta en dockerhub puedes subir tu imagen:

~~~~
    docker push imagen/tag
~~~~

Con eso puedes bajar la imagen desde cualquier sitio:

~~~~
    docker pull usuario/imagen/tag
~~~~

En base a una imagen se pueden crear varios contenedores, por ejemplo:

~~~~
    docker run -it -d -p 3001:3000 imagen/tag
    docker run -it -d -p 3002:3000 imagen/tag
~~~~


Cuando eliminamos un contenedor, su sistema de archivos e información también son eliminadas.

Si se quieren guardar los datos al eliminar el docker, debemos guardar los volúmenes de docker (normalmente se crean mediante opción de forma explícita o se puede asociar un directorio).

También se pueden crear redes para comunicar distintos dockers entre ellas.

Podemos utilizar `docker-compose` (o `docker compose`, dependiendo de la versión) cuando queremos que varios contenedores de docker se inicien al mismo tiempo (y además establecer dependencias entre ellos).

Para administrar muchos contenedores, podemos emplear orquestadores (por ejemplo Kubernetes).

Ejemplos útiles de dockers:
- Imagen de gitlab: https://docs.gitlab.com/ee/install/docker.html
- Imagen de tensorflow/serving: https://www.tensorflow.org/install/docker?hl=es-419

## Pets vs cattle (mascotas versus ganado)
Si un servidor se cae, debería ser reemplazo automáticamente y de forma transparente al usuario (los servidores son ganado).

Para esto ayuda tener la aplicación distribuída en microservicios.

Si la caída de un servidor origina un problema probablemente tengas una mascota, puesto que seguramente conozcas a ese servidor por su nombre: Afrodita, Zeus, MrBurns... lo mantengas y arregles cuando falla y planifiques su mantenimiento como pieza crítica en tu organización.

Si por el contrario tienes un grupo de servidores sin nombre o con nombres: svr1, svr2... del modo en que se ponen etiquetas en las orejas del ganado, a los que aplicas configuraciones automáticas (probablemente a todos las mismas) cuando uno cae, simplemente lo reemplazas por otro, como el ganado. Ejemplos de esto pueden ser: Clusters NO-SQL, servidores de bigdata, clusters de búsqueda, etc.

Las organizaciones que poseen hardware en sus instalaciones, tienen hoy día varios servidores en su rack y necesitan automatizar su instalación, configuración y uso. Existen herrameintas como Kubernetes, Pupper, Chef o Ansible que permiten automatizar y sacarle más rendimiento a nuestro ganado.

Más información:

- <https://www.hava.io/blog/cattle-vs-pets-devops-explained>
- <https://www.ilimit.com/blog/orquestracion-cloud-pets-cattles/>

## Imágenes de docker

- Docker Hub
- Dockerfile: From, Run, Copy, Add, Entrypoint, Cmd y .dockerignore
- Danling Images: Imágenes no referenciadas, sin nombre o tag pero ocupan espacio.

## Redes en docker
Existen 5 tipos de driver:
- none
- bridge
- host
- macvlan
- ipvlan
- overlay

<https://docs.docker.com/network/>

Por defecto se usa bridge (crea el dispositivo: docker0).\
No se puede hacer ping por nombre de contenedor (habrá que poner la IP).

~~~~
    docker network create NOMBRE --subnet 192.168.0.0/24 --gateway 192.168.0.1

    docker network ls

    docker run -d (--network NOMBRE) nginx (--ip 192.168.0.200) .
~~~~

En las redes que nosotros creamos se permite hacer ping por nombre de host.

Añade contenedor a una red:
~~~~
    docker network connect NOMBRE contenedor
~~~~

## Estadísticas
Nos permiten ver estadísticas del contenedor: RAM, procesador en uso, uso de red y si tienen algún límite de uso de RAM.

~~~~
    docker stats
~~~~

## Docker Compose ó docker-compose

Para organizar/orquestar contenedores. Crear contenedores que trabajen juntos.
Es decir, aplicaciones multicontenedor para deplegar rápidamente.
Usa docker-compose.yml. Emplea formato YAML.

### Etiquetas en docker-compose.yml
- version
- services
- volumes
- networks

## Moviendo dockers
- **Opción 1**: Exportando e importando
~~~~
    docker export container-name | gzip > container-name.gz
    zcat container-name.gz | docker import - container-name
~~~~

- **Opción 2**: Migrando la imagen
~~~~
    docker commit container-id image-name
~~~~

- **Opción 3**: Salvando y creando imágenes
~~~~
    docker save image-name > image-name.tar

    cat image-name.tar | docker load
~~~~

- **Opción 4**: Hacer backup del volumen de datos y restaurarlo
~~~~
    docker run --rm --volumes-from datavolume-name -v $(pwd):/backup image-name tar cvf  backup.tar /path-to-datavolume

    docker run --rm --volumes-from datavolume-name -v $(pwd):/backup image-name bash -c "cd /path-to-datavolume && tar xvf /backup/backup.tar --strip 1"
~~~~

Si queremos mover varios todos los dockers, otra opción sería copiar el directorio de trabajo de docker:

`/var/lib/docker`

Habría que tener en cuenta que debemos:

1. Tener la misma versión de docker en los host de origen y destino.
2. Preservar los permisos y propietarios de archivos y carpetas.
3. Parar el servicio docker antes de copiar nada: `service socker stop`
4. Si hay otras rutas a carpetas, deben existir, tener los permisos adecuados y asegurarnos que no dan otros problemas.

Lo lógico en estos casos es planificar la migración, probarla y ejecutar finalmente un script de migración con los pasos necesarios para minimizar el tiempo de esta migración.

## Extensiones para VSCODE
- Una extensión para gobernarlas a todas: Remote development
- <https://code.visualstudio.com/remote/advancedcontainers/develop-remote-host>
- <https://code.visualstudio.com/docs/devcontainers/containers#installation>
- <https://code.visualstudio.com/docs/remote/ssh#_connect-to-a-remote-host>


## Contenedores interesantes para probar
- rednode
- jenkins
- tensorflow

Ejemplo con docker compose. Moodle:

Bajar el `docker-compose.yml` de moodle hecho por bitnami.

~~~~
curl -sSL https://raw.githubusercontent.com/bitnami/containers/main/bitnami/moodle/docker-compose.yml > docker-compose.yml
~~~~

Lanzalo:

~~~~
    docker-compose up -d
~~~~

## Herramientas relacionadas con dockers
- **Docker Desktop**: Cliente oficial de docker en modo gráfico. Para Windows/Linux/Mac. Es de pago para empresas grandes.
- **Portainer.io**: Panel de control web para contenedores (de pago). Gratis 5 contenedores con registro previo
- **Podman.io**: "Sustituto" de docker. Se ejecuta como programa (y no demonio o servicio). Con o sin root ejecuta los contenedores usando libpod (imágenes, volúmenes, contenedores "pod"). También tiene su versión podman desktop, que es opensource. Mantenido por RedHat. Herramientas relacionadas: Buildah, Skopeo. <https://www.redhat.com/es/topics/containers/what-is-podman> 
- **Rancher Desktop**: Cliente open-source para gestión de Containers y Kubernetes para Windows, Linux y Mac.
- **vaultproject.io**: Hashicorp vault. Permite almacenar credenciales y usarlas como parte del proceso CD/CI para no tener que almacenarlas en el repositorio directamente.

## PLONK Stack

Consiste en los siguientes elementos:

- **Prometheus.io**: Monitorea y gestiona alertas. Soporta métricas en tiempo real y usa una BBDD con series temporales.

- **Linux**:

- **OpenFaaS**: "Function as a service". Haciendo uso de una arquitectura de microservicios podemos desarrollar, ejecutar y administrar funcionalidades en una aplicación olvidándonos de la parte de la infraestructura ("serverless"). Ejemplos en los que se puede aplicar esta técnica son: procesos batch (por lotes), procesamiento en stream (flujo), procesos ETL, IoT (Internet de las cosas), apps para móviles, aplicaciones web, APIs, etc. La diferencia con PaaS es que el desarrollador/admnistrador ni siquiera debe preocuparse por el escalado o aumentar los servidores.

- **NATS**: Mensajería de alto rendimiento. Comunica sistemas/procesos/servicios. Partes: NATS Core, Jet Stream.

- **Kubernetes**: Orquestador para desplegar los contenedores en servidores.

**Más información**: <https://www.openfaas.com/blog/plonk-stack/>

## Mirar también...
- <https://www.youtube.com/watch?v=MIl-LJodYUU>
- <https://bobcares.com/blog/move-docker-container-to-another-host/>
