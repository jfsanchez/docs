# 💧 Apache Nifi &mdash; 🎯 Parámetros e segredos


## Asociar un contexto de parámetros a grupo de procesamento

Esto é particularmente útil para parámetros que, sin ser un segredo (como contrasinais) empréganse en varios grupos de procesamento.

Por exemplo, a ruta a onde está gardado o driver JDBC.

Escolle un grupo de procesamento no que configuraras un **DBCPConnectionPool**.

Dende o Canvas principal imos a: Menú &rarr; **Parameter Context**.

Dende esta sección da páxina prememos á dereita no botón + e introducimos:

- Lapela **Settings**:
    - **Name**: Drivers
- Lapela **Parameters**, crear dous
    - **Name**: **MYSQL8_JDBC**. Value: /opt/nifi/compartido/jdbc/mysql-connector-j-8.4.0.jar
    - **Name**: **MYSQL9_JDBC**. Value: /opt/nifi/compartido/jdbc/mysql-connector-j-9.2.0.jar

Agora imos ao grupo de procesamento e prememos Click dereito &rarr; **Configure** (opcionalmente tamén click dereito &rarr; **Configure** dende dentro do grupo, nunha parte libre do Canvas).

Dentro da lapela **Settings**, en **Parameter Context** poderemos seleccionar o contexto que temos creado: **Drivers**. Finalmente prememos en **Apply** para gardar e pechar.

Dentro do grupo de procesamento &rarr; click dereito nunha parte libre do canvas &rarr; **Controller Services** e editamos o **DBCPConnectionPool**. Na lapela **Properties** en **Database Driver Location(s)** escribimos #{ e logo prememos Ctrl+Espacio para poder ver a lista de parámetros a seleccionar. Escollemos: `#{MYSQL8_JDBC}`.

Agora, se por calquer motivo mudamos a ruta onde gardamos o JDBC e dita ruta está en varios controis, bastaría con cambiar unha soa vez o paráemtro. Ademáis, para meter esta ruta en novos controis, se nos autocompletaría.

## Xestor de parámetros / Xestión de segredos

(en elaboración)

- Xestor de contrasinais: 1Password, AWS, Google Cloud...
- DatabaseParameterProvider
- HashiCorpVaultParameterProvider

Mentres os parámetros podemos almacenalos en bases de datos, para os contrasinais é recomendable empregar un xestor de segredos específico, xa ben sexa un do estilo dos empregados por docker/kubernetes/etc ou un xestor de contrasinais que admita API.