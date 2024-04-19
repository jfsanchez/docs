# 🧾 Apache Spark - Instalación

Con ClusterShell.

Instalaremos Spark e miraremos os procesos con permisos de administración:

``` bash
sudo yum --enablerepo=extras install epel-release
```

``` bash
sudo yum install clustershell
```

``` bash
clush -l cesgaxuser -bw hadoop[1-3] sudo rpm --import https://yum.corretto.aws/corretto.key
```

``` bash
clush -l cesgaxuser -bw hadoop[1-3] sudo curl -L -o /etc/yum.repos.d/corretto.repo https://yum.corretto.aws/corretto.repo
clush -l cesgaxuser -bw hadoop[1-3] sudo dnf install -y java-11-amazon-corretto-devel
```

``` bash
clush -l cesgaxuser -bw hadoop[1-3] sudo curl -L -O https://dlcdn.apache.org/spark/spark-3.3.2/spark-3.3.2-bin-hadoop3.tgz
```

``` bash
clush -l cesgaxuser -bw hadoop[1-3] sudo tar xzvf spark-3.3.2-bin-hadoop3.tgz
```

Copiamos el siguiente template y editamos el siguiente fichero:
``` bash
sudo cp /home/cesgaxuser/spark-3.3.2-bin-hadoop3/conf/spark-defaults.conf.template /home/cesgaxuser/spark-3.3.2-bin-hadoop3/conf/spark-defaults.conf
sudo nano /home/cesgaxuser/spark-3.3.2-bin-hadoop3/conf/spark-defaults.conf
```

Dentro metemos la siguiente línea:
``` bash
spark.master yarn
```

No nodo master executamos:
``` bash
sudo /home/cesgaxuser/spark-3.3.2-bin-hadoop3/sbin/start-master.sh
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
