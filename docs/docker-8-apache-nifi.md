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