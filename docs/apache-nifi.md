# Apache Nifi

Web Oficial: <https://nifi.apache.org/>
![Apache Nifi](images/nifi/Apache-nifi-logo.svg "Nifi")

É un software adicado a automatizar o fluxo de datos entre sistemas.

Empregaremos o software integrándoo co Apache Hadoop do CESGA, de xeito que poidamos ler e escribir do HDFS.

## Instalación

Instalaremos sobre o noso usuario no servidor: `hadoop.cesga.es`, é conveniente que conectemos coa VPN para evitar problemas cos portos.

Cando conectamos con algún servizo do CESGA por SSH, en realidade estamos nun nodo de login, dende o que accedemos aos servizos. Esto implica que podemos chegar a ter unha IP interna diferente incluso se abrimos dúas sesións ao «mesmo» host.

Ten en conta que Nifi abrirá un porto e exporá o seu servizo https á rede que lle indiquemos. Precisarás coñecer a IP cando esteas a cambiar os arquivos de configuración.

### Descarga, verificación e outras operacións

Precisamos unha versión de Java máis recente, imos empregar a versión 11 de Amazon Corretto (A versión 21 de Corretto en decembro de 2023, está a dar problemas coa execución de Nifi na contorna do CESGA).

Descargamos Amazon Corretto v11 e descomprimimos:

~~~~
wget wget https://corretto.aws/downloads/latest/amazon-corretto-11-x64-linux-jdk.tar.gz
tar -xzf amazon-corretto-11-x64-linux-jdk.tar.gz
~~~~

Creamos un directorio local bin no que poñeremos tódalas ferramentas necesarias:

~~~~
mkdir bin
~~~~

Descargamos Apache Nifi 1.24.0 e o seu arquivo de firma (asc):

~~~~
wget https://dlcdn.apache.org/nifi/1.24.0/nifi-1.24.0-bin.zip --no-check-certificate
wget https://dlcdn.apache.org/nifi/1.24.0/nifi-1.24.0-bin.zip.asc --no-check-certificate
~~~~

Comprobar a firma (e por tanto a integridade do arquivo e que non foi alterado) é unha boa práctica, así que primeiro baixamos a chave SSH coa que foi firmado o arquivo:

~~~~
gpg --keyserver pgpkeys.mit.edu --recv-key 0C07C6D5
~~~~

E verificamos que coincide:

~~~~
gpg --verify nifi-1.24.0-bin.zip.asc nifi-1.24.0-bin.zip
~~~~

Se todo coincide dirá "Good signature from ...". En caso de non coincidir a sinatura, debemos comprobar de novo os arquivos, volvelos baixar, revisar o sitio oficial e buscar outra descarga, etc.

Descomprimimos Apache Nifi:

~~~~
unzip nifi-1.24.0-bin.zip
~~~~

Agora imos copiar tanto Amazon Corretto como Apache Nifi dentro do directorio bin que temos creado

~~~~
mv amazon-corretto-11.0.21.9.1-linux-x64 nifi-1.24.0 bin/
~~~~

Finalmente facemos un pouco de limpieza:

~~~~
rm nifi-1.24.0-bin.zip nifi-1.24.0-bin.zip.asc
~~~~

### Configuración

Debemos configurar varias partes para que funcione:

- Variables do contorno: PATH e JAVA_HOME
- Apache Nifi (env e arquivo de configuración)

#### Configuración do PATH

Imos configurar as variables de contorno: `PATH` e `JAVA_HOME`. Aínda que non é absolutamente necesario (posto que imos sobreescribir estes cambios tamén en Nifi) si que é conveniente por si empregamos outros programas que queremos que fagan uso de esta versión de OpenJDK.

Editamos o arquivo: $HOME/.bash_profile e engadimos ao final as liñas:

~~~~
PATH=$HOME/bin:$HOME/bin/amazon-corretto-11.0.21.9.1-linux-x64/bin:$PATH
JAVA_HOME=$HOME/bin/amazon-corretto-11.0.21.9.1-linux-x64/

export JAVA_HOME
export PATH
~~~~

Agora temos dúas opcións: Ou sair e volver a entrar (logout e login) ou empregar o comando . ou source co arquivo .bash_profile:

~~~~
. .bash_profile
~~~~

ou

~~~~
source .bash_profile
~~~~

#### Configuración de Apache Nifi

Precisamos mudar dous arquivos:

- bin/nifi-env.sh
- conf/nifi.properties

Editamos primeiro nifi-env.sh:

~~~~
nano $HOME/bin/nifi-1.24.0/bin/nifi-env.sh
~~~~

Teremos que indicarlle que máquina de Java coller (descomentamos se fai falta o JAVA_HOME e poñémolo como segue):

~~~~
export JAVA_HOME="$HOME/bin/amazon-corretto-11.0.21.9.1-linux-x64/"
~~~~

Agora debemos configurar no arquivo `nifi.properties` o porto https, a IP na que vai a escoitar e o interfaz por defecto. Por defecto Apache Nifi abre un porto de xestión aleatorio, máis non abre o porto para a interfaz web.

A instalación é dependente da IP do nodo de login, polo que debemos consultala. A que se pon aquí dase como exemplo e debes mirar a túa.

Miramos a ip co comando `ifconfig`, en concreto interésanos a IPv4:

~~~~
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9000
        inet 10.10.10.101  netmask 255.0.0.0  broadcast 0.0.0.0
        inet6 fe80::0001:0203:0405:0001  prefixlen 64  scopeid 0x20<link>
        ether 00:01:02:03:04:05  txqueuelen 1000  (Ethernet)
        RX packets 324227052  bytes 234877693356 (218.7 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 242422788  bytes 830555348369 (773.5 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
~~~~

Debemos escoller un porto non ocupado por ninguén. Recomendación: Colle un porto algo, por exemplo 648XX e substitúe XX polo teu número de usuario.

Anota nalgún sitio a IP do comando anterior o porto que acabas de escoller. Vou empregar de exemplo a IP: 10.10.10.101 e o porto 64801. Ollo! emprega os datos correctos ou non che funcionará.

Editamos o arquivo nifi.properties:

~~~~
nano $HOME/bin/nifi-1.24.0/conf/nifi.properties
~~~~

E cubrimos cos datos anteriores as seguintes variables no arquivo:

~~~~
nifi.web.https.host=10.10.10.101
nifi.web.https.port=64801
~~~~

## Inicio de Nifi

Dentro de nifi hai un directorio bin que contén os scripts de lanzamento. En cocnreto interésanos:

- bin/nifi.sh

Entramos dentro do directorio:

~~~~
cd $HOME/bin/nifi-1.24.0/bin
~~~~

E executamos:

~~~~
./nifi.sh start
~~~~

Agora debemos consultar o usuario e clave por defecto en:

- logs/nifi-app.log

Buscaremos o texto "Generated":

~~~~
cat $HOME/bin/nifi-1.24.0/logs/nifi-app.log| grep Generated
~~~~

Con eses datos xa podemos entrar nun navegador web na IP do nodo do paso anterior (no meu exemplo: https://10.10.10.101:64801). Por favor non esquezas o **https**.

Cando remates, **antes de desconectarte do servidor de SSH**, non esquezas executar un:

~~~~
./nifi.sh stop
~~~~
