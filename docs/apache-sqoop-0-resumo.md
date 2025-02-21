# 🇸 Apache sqoop

![Logotipo de Apache Sqoop](images/sqoop/Apache_Sqoop_logo.svg "Logotipo de Apache Sqoop")

[Apache sqoop](https://sqoop.apache.org/) é un proxecto xa obsoleto, a última publicación data do 18 de xaneiro de 2019. [En 2021, foi movido ao "ático" de Apache](https://attic.apache.org/projects/sqoop.html), o lugar onde se atopan os proxectos retirados ou que finalizaron o seu ciclo de vida ou non teñen suficientes desenvolvedores activos involucrados.

Este proxecto permítenos mover datos entre o HDFS (Hadoop Distributed File System) e un RDBMS (Relational Database Management System).

Hai dúas operacións básicas que nos interesan:

- **import**: Importar datos ao HDFS dende un RDBMS (dirección: do RDBMS ao HDFS).
- **export**: Exportar datos do HDFS ao RDBMS (dirección: do HDFS ao RDBMS).

## Instalación

0. Precisamos Java 1.8 ou [Amazon Corretto](https://aws.amazon.com/es/corretto).

  wget https://corretto.aws/downloads/latest/amazon-corretto-21-x64-linux-jdk.deb
  sudo dpkg -i amazon-corretto-21-x64-linux-jdk.deb

1. Baixamos a versión 1.4.7:

  wget https://archive.apache.org/dist/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz

2. Descomprimimos:

  tar -xzf sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz

3.

## Manexo

Imos ver o funcionamento con estes dous drivers:

- MySQL/MariaDB
- PostgreSQL

## Máis información
  - <https://sqoop.apache.org/>
  - <https://attic.apache.org/projects/sqoop.html>
  - <https://bigdata.cesga.es/tutorials/sqoop.html#/>