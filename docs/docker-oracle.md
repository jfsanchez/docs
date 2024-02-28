# 🔮 Oracle Free (docker)

⚠️ **AVISO:** Apuntes en elaboración. Incompletos.

## Instalación

    docker pull container-registry.oracle.com/database/free:latest
    docker run -p 5560:5560 -d --name oracle_free  container-registry.oracle.com/database/free 

## Conexión

    docker exec -it oracle_free sqlplus / as sysdba

Para conectar a Oracle, dependendo se o temos instalado directamente ou nun docker, podemos empregar os seguintes comandos.

### Comandos directos:
    sqlplus sys@localhost:1521/FREEPDB1 as sysdba
    sqlplus sys@localhost:1521/FREE as sysdba

### Comandos para docker:
    $ docker exec -it dbname sqlplus / as sysdba
    $ docker exec -it dbname sqlplus sys/cdb-user-password@cdb-sid as sysdba
    $ docker exec -it dbname sqlplus system/cdb-user-password@cdb-sid
    $ docker exec -it dbname sqlplus pdbadmin/pdb-user-password@pdbname


## Conexión dende Python

~~~~
import oracledb

conn = oracledb.connect(user="[Username]", password="[Password]", dsn="localhost:1521/FREEPDB1")
with conn.cursor() as cur:
   cur.execute("SELECT 'Hello World!' FROM dual")
   res = cur.fetchall()
   print(res)
~~~~


## Limitacións de Oracle Free:

- Uso máximo de memoria RAM: 2 GB
- Cores que emprega como máximo: 2
- Tamaño máximo de BD: 12 GB
- Unha soa instancia por medio lóxico.
   

## Máis información:

- <https://www.oracle.com/es/database/free/get-started/>
- <https://docs.oracle.com/en/database/oracle/oracle-database/21/deeck/index.html>
- <https://docs.oracle.com/en/database/oracle/oracle-database/23/xeinl/licensing-restrictions.html>
