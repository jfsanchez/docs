# 🧾 Apache Spark - Instalación

Con ClusterShell.

A presente instalación contempla 4 máquinas: 1 master e 3 nodos (ou 1 master e 4 nodos actuando o máster tamén como nodo). Se tes un número diferente de máquinas, deberás mudar nos comandos o [1-4] ou [2-4] ás túas necesidades.

Instalaremos Spark e miraremos os procesos con permisos de administración:

## Instalación de Clustershell

1. Activar repo:

    ``` bash
    sudo yum --enablerepo=extras install epel-release
    ```

2. Instalación de paquete:

    ``` bash
    sudo yum install clustershell
    ```

## Descargar a máquina de Java (Amazon Corretto)

Configuramos o repo de Amazon Corretto e instalamos o paquete de Java en tódolos nodos:

1. Importamos a chave do repositorio:

``` bash
  clush -l cesgaxuser -bw hadoop[1-4] \
    sudo rpm --import https://yum.corretto.aws/corretto.key
```

2. Baixamos o repositorio e o configuramos nos nodos:

``` bash
  clush -l cesgaxuser -bw hadoop[1-4] \
    sudo curl -L -o /etc/yum.repos.d/corretto.repo https://yum.corretto.aws/corretto.repo
```

3. Instalamos o paquete de Java deste novo repostorio:
   
``` bash
  clush -l cesgaxuser -bw hadoop[1-4] \
    sudo dnf install -y java-11-amazon-corretto-devel
```

4. Configuramos as variables de contorno metendo no arquivo `.bashrc` as liñas:

``` bash
JAVA_HOME='/usr/lib/jvm/java-11-amazon-corretto/'
export JAVA_HOME
export EDITOR=nano
PATH=$PATH:$JAVA_HOME/bin
export PATH
```

## Descarga de Apache Spark

Pode ser que teñas que averiguar a nova ruta de existir unha nova versión. Podes ir ao nivel superior da páxina e buscar a nova versión: <https://dlcdn.apache.org/spark/>.

Segundo as túas necesidades podes ter que escoller entre a versión con ou sen Apache Hadoop.

Outra opción se contas con pouco ancho de banda é baixar unha vez e facer un --copy  a --dest con Clustershell.

1. Baixamos Apache Spark (actualizado a 20 de abril de 2024):

``` bash
  clush -l cesgaxuser -bw hadoop[1-4] \
    sudo curl -L -O https://dlcdn.apache.org/spark/spark-3.4.3/spark-3.4.3-bin-hadoop3.tgz \
    -o /home/cesgaxuser/spark-bin-hadoop3.tar.gz
```
Gárdase no arquivo `spark-bin-hadoop3.tar.gz` para que futuras versións destes apuntes non teñan que ser mudados tódolos comandos por mor da versión.

2. Descomprimimos simultáneamente en tódolos nodos:

``` bash
  clush -l cesgaxuser -bw hadoop[1-4] \
    sudo tar xzvf spark-bin-hadoop3.tgz
```

Copiamos o template de configuración e o editamos:

``` bash
  sudo cp $HOME/spark-bin-hadoop3/conf/spark-defaults.conf.template \
    $HOME/spark-bin-hadoop3/conf/spark-defaults.conf
```

``` bash
  sudo nano $HOME/spark-bin-hadoop3/conf/spark-defaults.conf
```

Dentro do arquivo, mudamos a configuración de Apache Spark para que empregue YARN (Yet Another Resource Negociator):

```
spark.master yarn
```

No nodo master executamos o script de inicio (estamos a executar todo como root):

``` bash
sudo /home/cesgaxuser/spark-bin-hadoop3/sbin/start-master.sh
```

Nos nodos slaves executamos:

``` bash
clush -l cesgaxuser -bw hadoop[2-3] sudo /home/cesgaxuser/spark-3.3.2-bin-hadoop3/sbin/start-worker.sh spark://hadoop1:7077
```

## Instalar pySpark

Paso 1: Instalar python
``` bash
clush -l cesgaxuser -bw hadoop[1-3] sudo dnf install -y python39
```

## Lanzar pyspark

``` bash
/home/cesgaxuser/spark-3.3.2-bin-hadoop3/bin/pyspark --master spark://hadoop1:7077
```

## Configurar para que o worker arranque no inicio do servidor:

``` bash
sudo crontab -e
```

Pulsar tecla INS (para habilitar inserción de texto no editor vi) e escribir a liña:

```
@reboot /home/cesgaxuser/spark-3.3.2-bin-hadoop3/sbin/start-worker.sh spark://hadoop1:7077
```

E agora pulsar a tecla ESC e despois o texto (sen as comillas) ":wq!" e logo premer ENTER.

E no master o mesmo, pero con comando master:

``` bash
sudo crontab -e
```

E meter no arquivo:

```
@reboot /home/cesgaxuser/spark-3.3.2-bin-hadoop3/sbin/start-master.sh
```

hdfs dfs -mkdir /user/

 hdfs dfs -mkdir /user/cesgaxuser

 hdfs dfs -put hadoop-3.2.4.tar.gz

 hdfs dfs -ls

No nodo master:

crontab -e

e meter esta linea ao final para tamén iniciar yarn:

@reboot /home/cesgaxuser/hadoop-3.2.4/sbin/start-yarn.sh

sudo reboot

yarn top

En .bashrc debaixo de HADOOP_HOME, meter esta liña:

export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop

En .bashrc, abaixo de todo, meter estas liñas ao final:

export SPARK_HOME=/home/cesgaxuser/spark-3.3.2-bin-hadoop3
export LD_LIBRARY_PATH=${HADOOP_HOME}/lib/native:$LD_LIBRARY_PATH

-----

Haberá que documentar (na práctica):

yarn top

yarn node -list

yarn application (-list/-kill)

jps -> De java (non spark ou hadoop)

------

Lanzar exemplos:

yarn jar hadoop-3.2.4/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.4.jar wordcount "books/*" output

---

Configurando Spark para que funcione con Hadoop:

Editamos o arquivo .bashrc e lle engadimos ao final:

export PATH=$PATH:$SPARK_HOME/sbin/:$SPARK_HOME/bin/

Ao final do arquivo, depois de todas estas configuracións, debe quedar así:

JAVA_HOME='/usr/lib/jvm/java-11-amazon-corretto/'
export JAVA_HOME

HADOOP_HOME='/home/cesgaxuser/hadoop-3.2.4'
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop

export PATH=${PATH}:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin
export HADOOP_HOME

export EDITOR=nano

export SPARK_HOME=/home/cesgaxuser/spark-3.3.2-bin-hadoop3
export LD_LIBRARY_PATH=${HADOOP_HOME}/lib/native:$LD_LIBRARY_PATH
export PATH=$PATH:$SPARK_HOME/sbin/:$SPARK_HOME/bin/

Copiamos o arquivo de template por defecto de Spark para empregalo:

cp spark-3.3.2-bin-hadoop3/conf/spark-defaults.conf.template spark-3.3.2-bin-hadoop3/conf/spark-defaults.conf

Editamos o arquivo: spark-3.3.2-bin-hadoop3/conf/spark-defaults.conf

Metemos a liña:

spark.master                     yarn

Reiniciamos con clush tódolos nodos.

Executamos dende o master con spark-submit un traballo, que debería enviarse ao hadoop.

 spark-submit --deploy-mode client --class org.apache.spark.examples.SparkPi $SPARK_HOME/examples/jars/spark-examples_2.12-3.3.2.jar 2

Miramos nos logs de hadoop que se executara.

Dende o nodo master temos (por mor de clustershell) python 3.6 pero temos python 3.9 no resto de nodos.

Instalamos python3.9:

sudo dnf install python39

E metemos a versión 3.9 por defecto no master.

sudo /usr/sbin/alternatives --config python

sudo /usr/sbin/alternatives --config python3

![PySpark](images/spark/pyspark.png "PySpark")


Para ler arquivos do HDFS dende yarn/jupyterlab hai que poñer a ruta completa:

df = spark.read.csv("hdfs://host:9000/user/cesgaxuser/arquivo.csv")

Ollo! se probas dende pyspark, que acceda ao cluster e non cree un.
