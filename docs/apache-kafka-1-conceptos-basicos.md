# ➿ Apache Kafka &mdash; 🐜️ Conceptos básicos

![Logo Apache Kafka](images/kafka/Apache_Kafka_logo.svg#derecha "Logo Apache Kafka")

Apacha Kafka é unha plataforma **distribuida** en **tempo real** para **streaming** de datos. Foi desenvolvida por Linkedin e posteriormente donada á [Apache Software Foundation](https://apache.org/). Está escrito en Java e Scala.

Imaxina un servidor de IRC ou de discord pero para aplicacións. Hay temas (ou canles) dos que queres falar e cada aplicación pode enviar ou ler mensaxes, sen que se perdan.

Se ves do mundo dos microservicios, sonarache o protocolo **MQTT**: **M**essage **Q**ueue **T**elemetry **T**ransport. Este esquema segue un sistema de publicación &harr; suscripción moi parecido ao de Apache Kafka. **MQTT** é un protocolo cunha especificación para a capa de transporte máis non o contido o u como funciona a aplicación. Os programas con soporte MQTT deberían ser compatibles entre si, por exemplo:

- <https://www.rabbitmq.com/>
- <https://mosquitto.org/>
- <https://joram.ow2.io/>

Nestes sistemas, aínda que se pode producir/consumir mensaxes a distintas velocidades, podería chegarse a perder algunha. Está pensado máis para a inxesta en tempo real.

En Kafka non se perden mensaxes, xa que se persisten (gardan) por eso as veces, algunhas persoas considérano como unha base de datos por algunhas das súas características de persistencia, recuperación de datos, particionamento, etc.

Por outra banda, Apache Kafka está deseñado para ser depregado como un cluster de varios nodos, fortalecendo así a súa estabilidade. Emprega o seu propio protocolo de rede, polo que non ten porqué ser compatible con MQTT de xeito directo (aínda que existen conectores e implementacións para conseguir esta compatibilidade).

A vantaxe de Apache Kafka radica na súa estabilidade xa que é un sistema que pode estar distribuido e replicado.

![Esquema Kafka](images/kafka/kafka-modelo.png "Esquema Kafka")


## Máis información

- <https://www.confluent.io/resources/kafka-the-definitive-guide/>
- <https://github.com/javicacheiro/pyspark_course/blob/master/supplementary/kafka/>

