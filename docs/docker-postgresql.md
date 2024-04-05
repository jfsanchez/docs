# 🧾 PostgreSQL (docker)

- Baseado na imaxe oficial: <https://hub.docker.com/_/postgres>

## Instalación co docker

~~~~
docker run --name de-postre-sql -e POSTGRES_PASSWORD=Cl431Ns3gur4 \
    -p 5432:5432 -p 5433:5433 -d postgres
~~~~

Se non che funciona a conexión dende o exterior, cambia o porto a outro que non sexa coñecido. Algúns servizos poden bloquear, por seguridade, portos coñecidos.

## Conexión simple co cliente nativo

~~~~
docker exec -it de-postre-sql psql -U postgres
~~~~


## Comandos útiles

**Amosar bases de datos**

~~~~
\l
~~~~

**Seleccionar base de datos a empregar**

~~~~
\c postgres
~~~~

**Amosar táboas**

~~~~
\dt
~~~~

**Crear base de datos**

~~~~
CREATE DATABASE sobremesa;
~~~~

**Crear un usuario**

~~~~
CREATE USER lambon WITH PASSWORD 'Fl4nD3C4f3';
~~~~

**Dar permisos**

Damos os permisos sobre a BBDD ao usuario:

~~~~
GRANT ALL PRIVILEGES ON DATABASE sobremesa to lambon;
~~~~

Conectamos á base de datos:

~~~~
\c sobremesa
~~~~

Damos permiso ao esquema public:

~~~~
GRANT ALL ON SCHEMA public TO lambon;
~~~~

**Conectar co usuario, contrasinal e BBDD creadas**

Se temos aberta a conexión á BBDD, pechámola.

~~~~
docker exec -it de-postre-sql \
    psql postgresql://lambon:Fl4nD3C4f3@localhost/sobremesa
~~~~

## Conexión dende Python

 - <https://github.com/jfsanchez/SBD/blob/main/notebooks/bbdd/postgresql-psycopg2-sqlalchemy.ipynb>

## Máis información

 - <https://www.postgresql.org/>
 - <https://www.w3schools.com/postgresql/index.php>
 - <https://www.postgresql.org/docs/current/datatype.html>
