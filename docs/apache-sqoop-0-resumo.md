# 🇸 Apache sqoop

![Logotipo de Apache Sqoop](images/sqoop/Apache_Sqoop_logo.svg "Logotipo de Apache Sqoop")

[Apache sqoop](https://sqoop.apache.org/) é un proxecto xa obsoleto, a última publicación data do 18 de xaneiro de 2019. [En 2021, foi movido ao "ático" de Apache](https://attic.apache.org/projects/sqoop.html), o lugar onde se atopan os proxectos retirados ou que finalizaron o seu ciclo de vida ou non teñen suficientes desenvolvedores activos involucrados.

Este proxecto permítenos mover datos entre o HDFS (Hadoop Distributed File System) e un RDBMS (Relational Database Management System).

Hai dúas operacións básicas que nos interesan:

- **import**: Importar datos ao HDFS dende un RDBMS (dirección: do RDBMS ao HDFS).
- **export**: Exportar datos do HDFS ao RDBMS (dirección: do HDFS ao RDBMS).

## Instalación

0. Precisamos Java 1.8 ou [Amazon Corretto](https://aws.amazon.com/es/corretto).

``` bash
  wget https://corretto.aws/downloads/latest/amazon-corretto-21-x64-linux-jdk.deb
  sudo dpkg -i amazon-corretto-21-x64-linux-jdk.deb
```

1. Baixamos a versión 1.4.7:

``` bash
  wget https://archive.apache.org/dist/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz
```

2. Descomprimimos:

``` bash
  tar -xzf sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz
```

3. Metemos no PATH:

``` bash
echo PATH=\$PATH:\$HOME/sqoop-1.4.7.bin__hadoop-2.6.0/bin/ >> $HOME/.bashrc
```

4. Agora, se executamos sqoop teremos que configurar o **$HADOOP_COMMON_HOME:**

~~~
  Error: /home/user/sqoop-1.4.7.bin__hadoop-2.6.0/bin/../../hadoop does not exist!
  Please set $HADOOP_COMMON_HOME to the root of your Hadoop installation.
~~~

## Manexo

Imos ver o funcionamento con estes dous drivers:

- MySQL/MariaDB
- PostgreSQL

### Proba de conexión

Imos probar a conectar coa BBDD **world**. Todos os exemplos inclúen esa BBDD. Podes ver a [instalación de MySQL coa base de datos de proba aquí](docker-1-my-maria.md).

=== "MySQL"

    ``` bash
    sqoop list-tables --username USUARIO-BD \
       -P --connect jdbc:mysql://IP-SERVIDOR-MYSQL/world
    ```

=== "PostgreSQL"

    ``` bash
    sqoop list-tables --username USUARIO-BD \
       -P --connect jdbc:postgresql://IP-SERVIDOR-MYSQL/world
    ```

### Importar datos

Dirección: RDBMS (BBDD) &rarr; HDFS

~~~ bash
sqoop import --username USUARIO-BD --password abc123. \
  --connect jdbc:mysql://IP-SERVIDOR-MYSQL/world \
  --table country \
  --target-dir /user/USUARIO-HADOOP/world \
  --num-mappers 1
~~~

#### Importar en HIVE

##### Crear táboas en HIVE

~~~ bash
sqoop create-hive-table \
  --username USUARIO-BD --password PASSWORD-BD \
  --connect jdbc:mysql://IP-SERVIDOR-MYSQL/world \
  --table country
~~~

##### Meter os datos na estrutura creada

~~~ bash
sqoop import \
  --username USUARIO-BD --password PASSWORD-BD \
  --connect jdbc:mysql://IP-SERVIDOR-MYSQL/world \
  --table country \
  --target-dir /user/USUARIO-HADOOP/country \
  --num-mappers 1 \
  --hive-import
~~~

### Exportar datos

Dirección: HDFS &rarr; RDBMS (BBDD)

~~~ bash
sqoop export \
  --username USUARIO-BD --password PASSWORD-BD \
  --connect jdbc:mysql://IP-SERVIDOR-MYSQ/world \
  --table country \
  --export-dir /user/USUARIO-HADOOP/country \
  --input-fields-terminated-by ',' \
  --num-mappers 1
~~~

Resultado (HADOOP):

~~~ bash
hdfs dfs -ls world
~~~


## Máis información
  - <https://sqoop.apache.org/>
  - <https://attic.apache.org/projects/sqoop.html>
  - <https://bigdata.cesga.es/tutorials/sqoop.html#/>