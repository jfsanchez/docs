# ☁️ OpenStack: Instancias

**Alcance**:

Imos aprender a lanzar unha ou varias instancias en Openstack, un contorno de nube/cloud empregado en varias empresas e tamén no (CESGA)[https://www.cesga.es].

**Aviso previo**: Se buscas recuperar unha instancia destruída en base a un volume gardado, consulta a sección volumes &rarr; [Como lanzar unha instancia a partir dun volume](/docs/openstack-volumes/#como-lanzar-unha-instancia-a-partir-dun-volume).

Antes de comezar lembra que debes **estar conectado á VPN** en caso necesario. No caso do CESGA, este panel de control está en: [https://cloud.srv.cesga.es] noutros casos de empresas que venden o servizo, debes crear o usuario de OpenStack antes de comezar.

## Configuración previa

### Inicio de sesión no panel

Partes do panel web.

### Creación do par de chaves

Imos conectar sen contrasinal, cun par de claves pública/privada. Podes ler máis información acerca delas en: [SSH e túneles](ssh-0-chaves-tuneles.md). Esta forma de conectar é o modo recomendado. Non se recomenda empregar contrasinais para conectar a servidores.

Temos dúas formas de crear este par de chaves. O habitual sería telas xa creadas e empregar o comando `ssh-keygen` dende GNU/Linux ou dende PowerShell en Microsoft Windows. Este comando encárgase xa de crear os arquivos de chave pública e privada cos permisos adecuados. Despois de creadas, poderíamos subir a chave pública (arquivo que rematará en .pub) que estaría dentro do directorio .ssh do noso directorio de usuario.

Sen embargo, desta vez, imos facer que nos autoxenere unha clave SSH o propio panel web.

### Creación do grupo de seguridade

Cando lanzamos unha instancia, esta debe ter un firewall.


## Lanzando unha ou varias instancias

Paso a paso

Computación &rarr; Instancias &rarr; Botón "Lanzar instancia"