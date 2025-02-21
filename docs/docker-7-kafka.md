# 🧾 Kafka

⚠️ **AVISO:** Apuntes en elaboración. Incompletos.


- Baseado na imaxe oficial: <https://hub.docker.com/r/apache/kafka>

## Creando o contedor

Lanzamos un novo contedor **kafkiano** que escoita no porto 9092.

``` bash
docker run -d  \
  --name broker \
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

## Creando un topic

Empregando o script `kafka-topics.sh` crearemos o topic (tema) **topicprimeiro**.

``` bash
docker exec -it kafkiano \
  /opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server localhost:9092 \
  --create --topic topicprimeiro
```

## Escribindo mensaxes ao topic (produtor)

Imos escribir unha serie de mensaxes no produtor, aínda que ninguén estea suscrito ao topic consumindo estes datos. Para lanzar un produtor (enviar datos a Kafka) empregamos o producer de consola de dentro do contedor:

``` bash
docker exec -it kafkiano \
	/opt/kafka/bin/kafka-console-producer.sh \
  	--bootstrap-server localhost:9092 \
	--topic topicprimeiro
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
docker exec --interactive -it kafkiano \
	/opt/kafka/bin/kafka-console-consumer.sh \
	--bootstrap-server localhost:9092 \
	--topic topicprimeiro \
	--from-beginning
```

Se deixamos aberta esta consola e noutra lanzamos un produtor que envíe datos ao **topicprimeiro**, deberían reflectirse nesta primeira consola en pouco tempo.

## Traballando en Python con Apache Kafka

## Ligazóns a máis información

### Descargas oficiais do software
- <http://kafka.apache.org>
- <https://hub.docker.com/r/apache/kafka>

### Librarías de conexión en Python
- <https://github.com/dpkp/kafka-python>
- <https://github.com/confluentinc/confluent-kafka-python>

### Outros
- <https://developer.confluent.io/get-started/python>

