# 🔵 Guía básica de dockers

- Baseado en: <https://docs.docker.com/engine/install/debian/>

## Instalación de docker en Debian

1. Crear a máquina en AWS / GCloud / Azure / CESGA Cloud ou instalación local en Microsoft Windows con WSL (Debian ou Ubuntu) e conectarse a ela por SSH. De ser unha instancia na nube, trataremos de elexir unha distribución Debian (recoméndase a última estable).

2. Actualizamos ata o final a máquina:
``` bash
sudo apt update
sudo apt -y dist-upgrade
sudo apt -y install curl
```

3. Executamos o script (guión) recomendado pola páxina oficial de docker:
``` bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
```

4. Engadimos o noso usuario ao grupo docker (para evitar empregar sudo):
``` bash
sudo usermod -a -G docker $USER
```

5. Saímos da sesión e volvemos abrila (ou abrimos unha sesión sobre a actual como se indica a continuación):
``` bash
sudo su - $USER
```

6. Probamos o docker de exemplo de **hola-mundo**:
``` bash
docker run hello-world
```
## Conceptos básicos de dockers

⚠️ Este resumo contén imprecisións porque pretende ser breve.

``` mermaid
mindmap
  root((Docker))
    Contedores
    Imaxes
    Volumes
    Redes
```
En docker existen os conceptos de: Contedores, imaxes, volumes e redes.

Cando executamos un "docker run", créase un contedor baseado nunha imaxe que se descarga de internet e arráncase. Este contedor é como unha máquina virtual xa configurada e funcionando.

Hai imaxes xa listas en internet: <https://hub.docker.com/> ou ben podemos facer a nosa.

Unha vez creado un contedor podémolo parar ou arrancar.

Podemos gardar os datos en volumes ou directorios compartidos. Se non especificamos nada a información queda no contedor ou no volume que cree por defecto (se o crea).

### Contedores
- Ver contedores en execución: ```docker ps```
- Ver tódolos contedores: ```docker ps -a```
- Crear un contedor: ```docker run hello-world```
- Parar un contedor: ```docker stop [ID ou NOME]```
- Iniciar un contedor: ```docker start [ID ou NOME]```
- Executar un comando dentro do contenedor: ```docker exec -it [ID ou NOME] [COMANDO]```
- Ver tódalas opcións do contedor: ```docker inspect [ID ou NOME]```
- Borrar un contedor: ```docker rm [ID ou NOME]```

👁️ Se queremos que os contedores volvan executarse cando a máquina se reinicie, cando no momento da creación (run) do contedor podemos especificar a opción: ```--restart unless-stopped```

### Imaxes
- Ver imaxes: ```docker image ls```
- Borrar imaxe: ```docker image rm [ID ou NOME]```

### Volumes
- Ver volume: ```docker volume ls```
- Borrar volume: ```docker volume rm [ID ou NOME]```

👁️ Se queremos ver os datos dun volume que xa non está asociado a un contedor, podemos crear un contedor temporal para velos: ```docker run -it --rm -v [ID do volume]:/vol busybox ls -l /vol```

## Estados dun contedor

``` mermaid
---
title: Estados dun contenedor docker
---
stateDiagram-v2
    [*] --> Creado
    Creado --> En_Execución
    En_Execución --> Parado_Rematado
    En_Execución --> Erro
    En_Execución --> Pausado
    Pausado --> En_Execución
    Parado_Rematado --> Erro
    Parado_Rematado --> En_Execución
    Parado_Rematado --> Borrado
    Parado_Rematado --> Morto
    Erro --> En_Reinicio
    En_Reinicio --> Erro
    En_Reinicio --> En_Execución
    Morto --> Borrado
    Borrado --> [*]

```

Realmente existen 6 estados. O estado borrado é para que se vexa mellor no diagrama máis non é un estado. O `Erro` ou `Morto` poden ser o mesmo estado nalgunhas circunstancias.

Máis información acerca dos estados en: <https://www.baeldung.com/ops/docker-container-states>.

## Imaxes oficiales para docker que podes probar

Nesta páxina tes algunhas configuracións rápidas (exemplos xa feitos) baseados nestas imaxes:

- <https://hub.docker.com/_/mysql>
- <https://hub.docker.com/_/mariadb>
- <https://mariadb.com/kb/en/installing-and-using-mariadb-via-docker/>
- <https://hub.docker.com/_/microsoft-mssql-server>
- <https://hub.docker.com/_/mongo>
- <https://hub.docker.com/_/redis>
- <https://hub.docker.com/_/postgres>
- <https://hub.docker.com/_/cassandra>
