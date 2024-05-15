# ☁️ OpenStack: Instancias

Imos aprender a lanzar unha ou varias instancias en Openstack, un contorno de nube/cloud empregado en varias empresas e tamén no [CESGA](https://www.cesga.es).

Se buscas **recuperar** unha instancia destruída en base a un volume gardado, consulta a sección **☁️ OpenStack: Volumes** &rarr; [Como lanzar unha instancia a partir dun volume](/docs/openstack-volumes/#como-lanzar-unha-instancia-a-partir-dun-volume).

## Configuración previa

### Acerca de Openstack

Antes de comezar lembra que debes **estar conectado á VPN** en caso necesario. No caso do CESGA, este panel de control está en: <https://cloud.srv.cesga.es> noutros casos de empresas que venden o servizo, debes crear o usuario de OpenStack antes de comezar.

Inicia sesión no panel de control.

![OpenStack. Inicio de sesión](images/openstack/login.png "OpenStack. Inicio de sesión")

Se estás no contorno do CESGA lembra empregar o dominio `hpc` e autenticar mediante `KeyStone Credentials`.

Por si tes curiosidade, Openstack ten moitos compoñentes, imos relacionarnos a través do interfaz web con todos eles, para que te fagas unha idea:

- **[Keystone](https://www.openstack.org/software/releases/caracal/components/keystone)**: Provee autenticación mediante diversos mecanismos (como usuario e contrasinal). Soporta: LDAP, OAuth, OpenID Connect, SAML e SQL
- **[Barbicam](https://www.openstack.org/software/releases/caracal/components/barbican)**: Ofrece almacenamento seguro de chaves, credenciais, certificados X509, chaves de cifrado...
- **[Ceilometer](https://www.openstack.org/software/releases/caracal/components/ceilometer)**: Para monitorizar os recursos e ver que pasa. Saca métricas e garda o emprego histórico de recursos.
- **[Horizon](https://www.openstack.org/software/releases/caracal/components/horizon)**: O contorno de usuario (GUI). Básicamente o panel de control ou *dashboard* que manexamos.
- **[Nova](https://governance.openstack.org/tc/reference/projects/nova.html)**: Para acceso a recursos de computación. Unha especie de meta-hypervisor que soporta: KVM, LXC (libvirt), QEMU, VMWare, Virtuozzo, zVM e Ironic. Fonte: [docs openstack](https://docs.openstack.org/nova/latest/admin/configuration/hypervisors.html).
- **[Neutron](https://www.openstack.org/software/releases/caracal/components/neutron)**: Xestiona as diferentes redes.
- **[Swift](https://www.openstack.org/software/releases/caracal/components/swift)**: Permite o almacenamento de obxectos e provee de tolerancia a fallos.
- **[Cinder](https://www.openstack.org/software/releases/caracal/components/cinder)**: Provee almacenamento en bloques.
- **[Glance](https://www.openstack.org/software/releases/caracal/components/glance)**: Almacena e recupera imaxes do disco da máquina virtual. Permite recuperar os datos dende distintas ubicacións.
- **[Placement](https://www.openstack.org/software/releases/caracal/components/placement)**: Acceso API ao inventario e uso de recursos. Axuda a outros servizos a aprovisionar recursos.
- **[Ironic](https://www.openstack.org/software/releases/caracal/components/ironic)**: Permite o aprovisionamento de recursos hardware directamente, máquinas virtuais ou contedores.
- **[Designate](https://www.openstack.org/software/releases/caracal/components/designate)**: Servizo de DNS.
- **[Octavia](https://www.openstack.org/software/releases/caracal/components/octavia)**: Balanceador de carga.
- **[AODH](https://www.openstack.org/software/releases/caracal/components/aodh)**: Servizo de alarmas. Provee disparadores e regras.
- **[Heat](https://www.openstack.org/software/releases/caracal/components/heat)**: Para orquestación.
- **[Magnum](https://www.openstack.org/software/releases/caracal/components/magnum)**:
- **[Manilla](https://www.openstack.org/software/releases/caracal/components/manila)**
- **[Trove](https://www.openstack.org/software/releases/caracal/components/trove)**:
- **[Zaqar](https://www.openstack.org/software/releases/caracal/components/zaqar)**: 
- **[Mistral](https://www.openstack.org/software/releases/caracal/components/mistral)**:
- **[Zun](https://www.openstack.org/software/releases/caracal/components/zun)**:


Podes atopar máis información básica no artigo: <https://www.redhat.com/es/topics/openstack> e se queres facer unha instalación de Openstack, tamén podes consultar [este outro artigo en inglés de Daniel Persson](https://danielpersson.dev/2022/07/25/manually-install-openstack/).

Partes do panel web.

### Creación do par de chaves

Imos conectar sen contrasinal, cun par de claves pública/privada. Podes ler máis información acerca delas en: [🔑 SSH e túneles](ssh-0-chaves-tuneles.md). Esta forma de conectar é o modo recomendado. Non se recomenda empregar contrasinais para conectar a servidores.

Temos dúas formas de crear este par de chaves. O habitual sería telas xa creadas e empregar o comando `ssh-keygen` dende GNU/Linux ou dende PowerShell en Microsoft Windows. Este comando encárgase xa de crear os arquivos de chave pública e privada cos permisos adecuados. Despois de creadas, poderíamos subir a chave pública (arquivo que rematará en .pub) que estaría dentro do directorio .ssh do noso directorio de usuario.

Sen embargo, desta vez, imos facer que nos autoxenere unha clave SSH o propio panel web.

### Creación do grupo de seguridade

Cando lanzamos unha instancia, esta debe ter un firewall.


## Lanzando unha ou varias instancias

Paso a paso

Computación &rarr; Instancias &rarr; Botón "Lanzar instancia"