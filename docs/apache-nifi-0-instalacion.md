# 💧 Apache Nifi &mdash; Instalación

![Apache Nifi](images/nifi/Apache-nifi-logo.svg "Nifi")

Web Oficial: <https://nifi.apache.org/>

É un software adicado a automatizar o fluxo de datos entre sistemas. Tamén é considerado unha ferramenta ETL (Extract/Load/Transform).

## Instalación rápida de Apache Nifi empregando Docker

 - Baseado no repositorio do proxecto Apache coa imaxe "non" oficial de Apache Nifi: <https://hub.docker.com/r/apache/nifi/>

Imos a crear un contedor que terá acceso a un directorio compartido para meter os drivers JDBC e os datasets que precisemos.

``` bash
mkdir -p $HOME/nifi-compartido/{jdbc,datasets}

docker run --name nifi \
  -p 8443:8443 \
  -e SINGLE_USER_CREDENTIALS_USERNAME=admin \
  -e SINGLE_USER_CREDENTIALS_PASSWORD=EsteEunContrasinalMoiLongo1234567890 \
  -v $HOME/nifi-compartido:/opt/nifi/compartido \
  --restart unless-stopped -d apache/nifi:latest
```

**Datos de conexión**

- 👤 Usuario por defecto: **admin**
- 🔑 Contrasinal de exemplo: **EsteEunContrasinalMoiLongo1234567890**
- 📝 Emprega https para conectar. Exemplo: <https://localhost:8443>

⚠️ **Advertencia**: Para conectar emprega **localhost** como nome da máquina (fai un túnel SSH cando sexa preciso). Do contrario vaiche dar un erro de SNI incorrecto. Se quixeras conectar cun DNS personalizado (por exemplo: nifi.jfsanchez.es) terías que xerar un certificado SSL e mudar o arquivo de configuración de nifi.

## Descargando e configurando o driver JDBC para MySQL

 1. Imos a <https://www.mysql.com/products/connector/> e seleccionamos JDBC for MySQL (connector J) &rarr; Download.
 2. Select Operating system &rarr; Platform independent.
 3. Platform Independent (Architecture Independent) &rarr; Compressed TAR Archive &rarr; Click dereito no botón Download e copiar a URL da ligazón.

Se quixésemos unha versión antiga (por exemplo a: 8.4.0), teríamos que ir a: <https://downloads.mysql.com/archives/c-j/>

Podes empregar o seguinte script que baixa as versión anteriormente citadas, descomprímeas e garda o .jar no directorio $HOME/nifi-compartido/jdbc/.

⚠️ Se deixa de funcionar podes corrixilo buscando a nova URL do driver.

~~~~bash
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-9.2.0.tar.gz
wget https://downloads.mysql.com/archives/get/p/3/file/mysql-connector-j-8.4.0.tar.gz
tar -xzf mysql-connector-j-9.2.0.tar.gz -C $HOME/nifi-compartido/jdbc
tar -xzf mysql-connector-j-8.4.0.tar.gz -C $HOME/nifi-compartido/jdbc
mv $HOME/nifi-compartido/jdbc/mysql-connector-j-9.2.0/mysql-connector-j-9.2.0.jar $HOME/nifi-compartido/jdbc/
mv $HOME/nifi-compartido/jdbc/mysql-connector-j-8.4.0/mysql-connector-j-8.4.0.jar $HOME/nifi-compartido/jdbc/
rm -rf $HOME/nifi-compartido/jdbc/mysql-connector-j-9.2.0 $HOME/nifi-compartido/jdbc/mysql-connector-j-8.4.0
rm mysql-connector-j-8.4.0.tar.gz mysql-connector-j-9.2.0.tar.gz
~~~~

## Metendo un driver nun docker xa lanzado sen directorio compartido

⚠️ Moi probablemente **non** che faga falta se seguiches as instruccións anteriores.

Se non temos metido un cartafol compartido no noso contedor, o máis cómodo é copiar directamente o driver, non é recomendable máis funciona.

~~~~
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-9.2.0.tar.gz
tar -xzf https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-9.2.0.tar.gz
docker exec -it nifi mkdir -p /opt/nifi/jdbc
docker cp mysql-connector-j-9.2.0/mysql-connector-j-9.2.0.jar nifi:/opt/nifi/jdbc/
~~~~

## Instalación manual

Empregaremos o software integrándoo co Apache Hadoop do CESGA, de xeito que poidamos ler e escribir do HDFS.

Instalaremos sobre o noso usuario no servidor: `hadoop.cesga.es`, é conveniente que conectemos coa VPN para evitar problemas cos portos.

Cando conectamos con algún servizo do CESGA por SSH, en realidade estamos nun nodo de login, dende o que accedemos aos servizos. Esto implica que podemos chegar a ter unha IP interna diferente incluso se abrimos dúas sesións ao «mesmo» host.

Ten en conta que Nifi abrirá un porto e exporá o seu servizo https á rede que lle indiquemos. Precisarás coñecer a IP cando esteas a cambiar os arquivos de configuración.

### Aviso previo

**AVISO**: A versión 23 de amazon-corretto é necesaria para executar a última versión 2 de Apache Nifi.

### Descarga, verificación e outras operacións

Precisamos unha versión de Java máis recente, imos empregar a versión 11 de Amazon Corretto (A versión 21 de Corretto en decembro de 2023, está a dar problemas coa execución de Nifi na contorna do CESGA).

Descargamos Amazon Corretto v11 e descomprimimos:

``` bash
wget https://corretto.aws/downloads/latest/amazon-corretto-11-x64-linux-jdk.tar.gz
tar -xzf amazon-corretto-11-x64-linux-jdk.tar.gz
```

Creamos un directorio local bin no que poñeremos tódalas ferramentas necesarias:

``` bash
mkdir bin
```

Descargamos Apache Nifi 1.24.0 e o seu arquivo de firma (asc):

``` bash
wget https://dlcdn.apache.org/nifi/1.24.0/nifi-1.24.0-bin.zip --no-check-certificate
wget https://dlcdn.apache.org/nifi/1.24.0/nifi-1.24.0-bin.zip.asc --no-check-certificate
```

Comprobar a firma (e por tanto a integridade do arquivo e que non foi alterado) é unha boa práctica, así que primeiro baixamos a chave SSH coa que foi firmado o arquivo:

``` bash
gpg --keyserver pgpkeys.mit.edu --recv-key 0C07C6D5
```

E verificamos que coincide:

``` bash
gpg --verify nifi-1.24.0-bin.zip.asc nifi-1.24.0-bin.zip
```

Se todo coincide dirá "Good signature from ...". En caso de non coincidir a sinatura, debemos comprobar de novo os arquivos, volvelos baixar, revisar o sitio oficial e buscar outra descarga, etc.

Descomprimimos Apache Nifi:

``` bash
unzip nifi-1.24.0-bin.zip
```

Agora imos copiar tanto Amazon Corretto como Apache Nifi dentro do directorio bin que temos creado

``` bash
mv amazon-corretto-11.0.21.9.1-linux-x64 nifi-1.24.0 bin/
```

Finalmente facemos un pouco de limpieza:

``` bash
rm nifi-1.24.0-bin.zip nifi-1.24.0-bin.zip.asc
```

### Configuración

Debemos configurar varias partes para que funcione:

- Variables do contorno: PATH e JAVA_HOME
- Apache Nifi (env e arquivo de configuración)

#### Configuración do PATH

Imos configurar as variables de contorno: `PATH` e `JAVA_HOME`. Aínda que non é absolutamente necesario (posto que imos sobreescribir estes cambios tamén en Nifi) si que é conveniente por si empregamos outros programas que queremos que fagan uso de esta versión de OpenJDK.

Editamos o arquivo: $HOME/.bash_profile e engadimos ao final as liñas:

``` bash title="$HOME/.bash_profile"
export PATH=$HOME/bin:$HOME/bin/amazon-corretto-11.0.21.9.1-linux-x64/bin:$PATH
export JAVA_HOME=$HOME/bin/amazon-corretto-11.0.21.9.1-linux-x64/
```

Agora temos dúas opcións: Ou sair e volver a entrar (logout e login) ou empregar o comando . ou source co arquivo .bash_profile:

``` bash
source ~/.bash_profile
```

ou

```
. ~/.bash_profile
```

#### Configuración de Apache Nifi

Precisamos mudar dous arquivos:

- bin/nifi-env.sh
- conf/nifi.properties

Editamos primeiro `nifi-env.sh`:

``` bash
nano $HOME/bin/nifi-1.24.0/bin/nifi-env.sh
```

Teremos que indicarlle que máquina de Java coller (descomentamos se fai falta o JAVA_HOME e poñémolo como segue):

``` bash
export JAVA_HOME="$HOME/bin/amazon-corretto-11.0.21.9.1-linux-x64/"
```

Agora debemos configurar no arquivo `nifi.properties` o porto https, a IP na que vai a escoitar e o interfaz por defecto. Por defecto Apache Nifi abre un porto de xestión aleatorio, máis non abre o porto para a interfaz web.

A instalación é dependente da IP do nodo de login, polo que debemos consultala. A que se pon aquí dase como exemplo e debes mirar a túa.

Miramos a ip co comando `ifconfig`, en concreto interésanos a IPv4:

```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9000
        inet 10.10.10.101  netmask 255.0.0.0  broadcast 0.0.0.0
        inet6 fe80::0001:0203:0405:0001  prefixlen 64  scopeid 0x20<link>
        ether 00:01:02:03:04:05  txqueuelen 1000  (Ethernet)
        RX packets 324227052  bytes 234877693356 (218.7 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 242422788  bytes 830555348369 (773.5 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Debemos escoller un porto non ocupado por ninguén. Recomendación: Colle un porto algo, por exemplo 648XX e substitúe XX polo teu número de usuario.

Anota nalgún sitio a IP do comando anterior o porto que acabas de escoller. Vou empregar de exemplo a IP: 10.10.10.101 e o porto 64801. Ollo! emprega os datos correctos ou non che funcionará.

Editamos o arquivo `nifi.properties`:

``` bash
nano $HOME/bin/nifi-1.24.0/conf/nifi.properties
```

E cubrimos cos datos anteriores as seguintes variables no arquivo:

``` title="conf/nifi.properties"
nifi.web.https.host=10.10.10.101
nifi.web.https.port=64801
```

## Inicio de Nifi

Dentro de nifi hai un directorio bin que contén os scripts de lanzamento. En cocnreto interésanos:

- bin/nifi.sh

Entramos dentro do directorio:

``` bash
cd $HOME/bin/nifi-1.24.0/bin
```

E executamos:

``` bash
./nifi.sh start
```

Agora debemos consultar o usuario e clave por defecto en:

- logs/nifi-app.log

Buscaremos o texto "Generated":

``` bash
cat $HOME/bin/nifi-1.24.0/logs/nifi-app.log| grep Generated
```

Con eses datos xa podemos entrar nun navegador web na IP do nodo do paso anterior (no meu exemplo: https://10.10.10.101:64801). Por favor non esquezas o **https**.

Cando remates, **antes de desconectarte do servidor de SSH**, non esquezas executar un:

``` bash
./nifi.sh stop
```
