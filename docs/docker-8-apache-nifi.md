# 🧾 Apache Nifi

## Apache Nifi empregando Docker

 - Baseado no repositorio do proxecto Apache coa imaxe "non" oficial de Apache Nifi: <https://hub.docker.com/r/apache/nifi/>

## Instalación con docker
``` bash
docker run --name nifi \
  -p 8443:8443 \
  -d \
  -e SINGLE_USER_CREDENTIALS_USERNAME=admin \
  -e SINGLE_USER_CREDENTIALS_PASSWORD=EsteEunContrasinalMoiLongo1234567890 \
  apache/nifi:latest
```

## Datos de conexión

- Usuario por defecto: admin
- Contrasinal de exemplo: EsteEunContrasinalMoiLongo1234567890
- Emprega https para conectar. Exemplo: https://localhost:8443

⚠️ **Advertencia**: Para conectar emprega **localhost** como nome da máquina (fai un túnel SSH cando sexa preciso). Do contrario vaiche dar un erro de SNI incorrecto. Se quixeras conectar cun DNS personalizado (por exemplo: nifi.jfsanchez.es) terías que xerar un certificado SSL e mudar o arquivo de configuración de nifi.

