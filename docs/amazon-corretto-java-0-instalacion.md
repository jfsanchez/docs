# Java / Amazon Corretto / OpenJDK

![Logo OpenJDK](images/openjdk/OpenJDK_logo.svg#derecha "Logo OpenJDK")

## Un pouco de historia

De xeito moi breve e explicando anos de controversia, poderíamos dicir que a primeira versión de Java (Oak) foi desenvolvida por Sun Microsystems, empresa comprada por Oracle en 2009/2010. Trala compra, houbo [denuncias a grandes empresas que empregaban a API de Java como Google no seu Android](https://es.wikipedia.org/wiki/Caso_Oracle_contra_Google). Tamén houbo cambios no sistema de licenciamento. En 2017 houbo cambios importantes no modelo de actualizacións e diversos problemas.

Entre tanto, no 2007 xa se creara un intento de máquina virtual de Java libre chamada [OpenJDK](https://openjdk.org/), o cal viu o seu uso aínda máis potenciado nos últimos anos. A historia é moito máis complexa e pode lerse na [páxina da Wikipedia adicada ao OpenJDK](https://es.wikipedia.org/wiki/OpenJDK).

Amazon fixo unha distribución optimizada para nube, nas súas palabras:

  "*[Amazon Corretto](https://aws.amazon.com/es/corretto/) es una distribución sin costo, multiplataforma y lista para producción de Open Java Development Kit (OpenJDK). Corretto cuenta con soporte a largo plazo que incluirá mejoras de rendimiento y correcciones de seguridad. Amazon ejecuta Corretto internamente en miles de servicios de producción. Corretto está certificado como compatible con el estándar Java SE. Con Corretto, puede desarrollar y ejecutar aplicaciones Java en sistemas operativos conocidos, como Linux, Windows y macOS*".


## Instalación de Amazon Corretto

### Opción 1: GNU/Debian

**Descargamos** e **instalamos** Java 1.8 ou [Amazon Corretto](https://aws.amazon.com/es/corretto):

  ``` bash
  sudo apt update
  sudo apt -y dist-upgrade
  sudo apt install java-common
  wget https://corretto.aws/downloads/latest/amazon-corretto-21-x64-linux-jdk.deb
  sudo dpkg -i amazon-corretto-21-x64-linux-jdk.deb
  rm amazon-corretto-21-x64-linux-jdk.deb
  ```

O paquete mete o binario dentro dun directorio que xa está no PATH, polo que poderemos executar:

  ``` bash
  java --version
  ```

**Configuración do JAVA_HOME**

Atopar o `JAVA_HOME` é tarefa sinxela. Se executamos o comando `type java` para saber onde reside o binario de java e imos averiguando a onde está apuntado o enlace simbólico con `ls -l RUTA`, sacaremos esta conclusión:

`/usr/bin/java` &rarr; `/etc/alternatives/java` &rarr; `/usr/lib/jvm/java-21-amazon-corretto/bin/java`.

Engadimos o **JAVA_HOME**. É moi conveniente para que os programas atopen o contorno de OpenJDK.


``` bash
echo "export JAVA_HOME='/usr/lib/jvm/java-21-amazon-corretto/'" >> $HOME/.profile
source ~/.profile
```


### Opción 2: Rocky Linux

``` bash
sudo dnf update -y
sudo curl -L -o /etc/yum.repos.d/corretto.repo https://yum.corretto.aws/corretto.repo
sudo dnf install -y java-21-amazon-corretto-devel
```

Engadimos o **JAVA_HOME**. É moi conveniente para que os programas atopen o contorno de OpenJDK.

``` bash
echo "export JAVA_HOME='/usr/lib/jvm/java-21-amazon-corretto/'" >> $HOME/.profile
source ~/.profile
```

### Opción 3: Xenérica GNU/Linux

Descargamos Amazon Corretto v11 no noso `$HOME` e descomprimimos:

``` bash
mkdir -p $HOME/bin
cd
wget https://corretto.aws/downloads/latest/amazon-corretto-11-x64-linux-jdk.tar.gz
tar -xzf amazon-corretto-11-x64-linux-jdk.tar.gz
mv amazon-corretto-11*-linux-x64/ bin/amazon-corretto-latest
rm amazon-corretto-11-x64-linux-jdk.tar.gz
```

**Configuración do PATH e do JAVA_HOME**

Imos configurar as variables de contorno: `PATH` e `JAVA_HOME`. O `PATH` é para que atope o comando java sen ter que teclear a ruta completa. O `JAVA_HOME` e moi conveniente para que os programas atopen o contorno de OpenJDK.

``` bash
echo "export PATH=$HOME/bin:$HOME/bin/amazon-corretto-latest/bin:$PATH" >> $HOME/.profile
echo "export JAVA_HOME=$HOME/bin/amazon-corretto-latest" >> $HOME/.profile
. ~/.profile
```

## Comandos útiles

- `jps`: [Ver os procesos en Java](https://docs.oracle.com/en/java/javase/21/docs/specs/man/jps.html).
- `keytool`: [Administrar os certificados empregados por Java nun almacén](https://docs.oracle.com/en/java/javase/21/docs/specs/man/keytool.html).


## Máis información

- Implementación libre de Java: <https://openjdk.org/>
- Amazon Corretto (baseada no OpenJDK): <https://aws.amazon.com/es/corretto/>
- Java de Oracle: <https://www.oracle.com/java/technologies/downloads/>