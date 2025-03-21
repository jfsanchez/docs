# ➿ Apache Kafka

![Logo Apache Kafka](images/kafka/Apache_Kafka_logo.svg#derecha "Logo Apache Kafka")

⚠️ **AVISO:** Apuntes en elaboración. Incompletos.

- Baseado na imaxe oficial: <https://hub.docker.com/r/apache/kafka>

## Creando o contedor

Lanzamos un novo contedor **brokerkafkiano** que escoita no porto 9092.

``` bash
docker run -d  \
  --name brokerkafkiano \
  -p 9092:9092 -p 9093:9093 \
  -e KAFKA_NODE_ID=1 \
  -e KAFKA_PROCESS_ROLES=broker,controller \
  -e KAFKA_LISTENERS=PLAINTEXT://:9092,CONTROLLER://:9093 \
  -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092 \
  -e KAFKA_CONTROLLER_LISTENER_NAMES=CONTROLLER \
  -e KAFKA_LISTENER_SECURITY_PROTOCOL_MAP=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT \
  -e KAFKA_CONTROLLER_QUORUM_VOTERS=1@localhost:9093 \
  -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 \
  -e KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR=1 \
  -e KAFKA_TRANSACTION_STATE_LOG_MIN_ISR=1 \
  -e KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS=0 \
  -e KAFKA_NUM_PARTITIONS=3 \
  --restart unless-stopped \
  apache/kafka:latest
```

⚠️ O nome **localhost** debería ser cambiado por un nome DNS ou nome presente no etc/hosts que se resolvese ben dende tódolos sitios que conecten con Kafka.


## Creando un topic

Empregando o script `kafka-topics.sh` crearemos o topic (tema) **metamorfosis**.

``` bash
docker exec -it brokerkafkiano \
  /opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server localhost:9092 \
  --create --topic metamorfosis
```

## Escribindo mensaxes ao topic (produtor)

Imos escribir unha serie de mensaxes no produtor, aínda que ninguén estea suscrito ao topic consumindo estes datos. Para lanzar un produtor (enviar datos a Kafka) empregamos o producer de consola de dentro do contedor:

``` bash
docker exec -it brokerkafkiano \
	/opt/kafka/bin/kafka-console-producer.sh \
  	--bootstrap-server localhost:9092 \
	--topic metamorfosis
```
Agora poderemos escribir algúns textos, por exemplo:

```
Hola Mundo!
Clave=Valor
1234567890
abc
```

Se queremos sair da consola, premeremos Ctrl+C.

## Recibindo mensaxes (consumidor)

Os datos anteriores seguen en Kafka e podemos ter acceso ao topic dende o inicio do mesmo:

``` bash
docker exec --interactive -it brokerkafkiano \
	/opt/kafka/bin/kafka-console-consumer.sh \
	--bootstrap-server localhost:9092 \
	--topic metamorfosis \
	--from-beginning
```

Se deixamos aberta esta consola e noutra lanzamos un produtor que envíe datos ao **metamorfosis**, deberían reflectirse nesta primeira consola en pouco tempo.

## Conceptos básicos

Imaxina un servidor de IRC ou de discord pero para aplicacións. Hay temas (ou canles) dos que queres falar e cada aplicación pode enviar ou ler mensaxes, sen que se perdan.

Se ves do mundo dos microservicios, sonarache o protocolo **MQTT**: **M**essage **Q**ueue **T**elemetry **T**ransport. Este esquema segue un sistema de publicación &harr; suscripción moi parecido ao de Apache Kafka. **MQTT** é un protocolo cunha especificación para a capa de transporte máis non o contido o u como funciona a aplicación. Os programas con soporte MQTT deberían ser compatibles entre si, por exemplo:

- <https://www.rabbitmq.com/>
- <https://mosquitto.org/>
- <https://joram.ow2.io/>

Nestes sistemas, aínda que se pode producir/consumir mensaxes a distintas velocidades, podería chegarse a perder algunha. Está pensado máis para a inxesta en tempo real.

En Kafka non se perden mensaxes, xa que se persisten (gardan) por eso as veces, algunhas persoas considérano como unha base de datos por algunhas das súas características de persistencia, recuperación de datos, particionamento, etc.

Por outra banda, Apache Kafka está deseñado para ser depregado como un cluster de varios nodos, fortalecendo así a súa estabilidade. Emprega o seu propio protocolo de rede, polo que non ten porqué ser compatible con MQTT de xeito directo (aínda que existen conectores e implementacións para conseguir esta compatibilidade).

## Partes

- Broker
- Produtor
- Consumidor

E un proxecto que se emprega do que poderíamos falar aparte:

- Zookeeper

Outros conceptos:

- Topic
- Partición
- Repartición

Un topic é como unha canle de información á que nos podemos unir. Algo así como unha sorte de canle dun IRC ou de discord.

Kafka sabe en que estado está cada consumidor e lle vai enviando as mensaxes.


## Kafka con Apache Spark

## Traballando en Python con Apache Kafka


### Instalación de paquetes

1) Instalamos os dous módulos, kafka-python é un módulo máis simple mentres que o de confluent ten 

``` bash
conda install -c conda-forge kafka-python python-confluent-kafka
```
## Traballando con Apache Nifi

Existen dous procesadores:

- **ConsumerKafka**. Permite recibir datos. Na lapela **Properties**:
    - Crear un novo **Kafka Connection Service** (se non existe)
    - **Group ID**: 1
    - **Topics**: metamorfosis

- **ProducerKafka**: Permite enviar datos. Na lapela **Properties**:
    - Crear un novo **Kafka Connection Service** (se non existe)
    - **Topic Name**: metamorfosis

Ambos precisan dun:

- **KafkaConnectionService**. Na lapela **Properties** debemos mudar:
    - **Bootstrap servers**: O nome DNS/host do servidor de Kafka. ⚠️ Ollo, se ese nome é localhost e estás noutro host, tratará de conectarse a onde estás.


## Ligazóns a máis información

### Descargas oficiais do software
- <http://kafka.apache.org>
- <https://hub.docker.com/r/apache/kafka>

### Librarías de conexión en Python
- <https://github.com/dpkp/kafka-python>
- <https://github.com/confluentinc/confluent-kafka-python>

### Configuración de autenticación e cifrado
- <https://kafka.apache.org/documentation/#security_authz>
- Titorial (non probado): <https://www.baeldung.com/ops/kafka-authentication-topics-sh>

### Outros
- <https://developer.confluent.io/get-started/python>
- <https://github.com/javicacheiro/pyspark_course/blob/master/supplementary/kafka/>
- <https://aitor-medrano.github.io/iabd/dataflow/kafka1.html>
