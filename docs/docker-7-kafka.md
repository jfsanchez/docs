# 🧾 Kafka

⚠️ **AVISO:** Apuntes en elaboración. Incompletos.


- Baseado na imaxe oficial: <https://hub.docker.com/r/apache/kafka>

## Creando o contedor

Lanzamos un novo contedor **kafkiano** que escoita no porto 9092.

``` bash
docker run --name kafkiano -p 9092:9092 \
  --restart unless-stopped \
  -d apache/kafka:latest
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
