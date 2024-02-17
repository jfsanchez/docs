# 🧾 MariaDB (docker)

Baseado na imaxe oficial: <https://hub.docker.com/_/mariadb>

~~~~
    docker run -p 9907:3306 -v /root/mariadatosdb:/var/lib/mysql/ --detach \
    --name mariadbpracticasql --env MARIADB_USER=usuariamaria \
    --env MARIADB_PASSWORD=DonaMaria123456 \
    --env MARIADB_ROOT_PASSWORD=EsteNonPodeSerOcontrasinalDEr00t \
    mariadb:latest
~~~~

Para conectar dende outro host:

- `X.X.X.X` é a IP do servidor ao que queremos conectar.

~~~~
mariadb -hX.X.X.X -P9907 -uroot -pEsteNonPodeSerOcontrasinalDEr00t
~~~~

⚠️ **AVISO**: Esta configuración NON pretende ser segura, o seu obxectivo é montar de xeito rápido un contorno para a aprendizaxe. Entre outras cousas deberíamos deshabilitar o usuario root para conexións remotas, borrar as BBDD de proba e impredir o acceso directo ao servidor de base de datos.
