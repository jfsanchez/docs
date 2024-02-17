# 🧾 MS-SQL (docker)

Microsoft SQL Server en Ubuntu (con docker).

Baseado na imaxe oficial: <https://hub.docker.com/_/microsoft-mssql-server>

- Usuario por defecto (admin): sa
- Contrasinal de exemplo: Abc12300. **Advertencia**: O contrasinal debe ter alomenos unha letra maiúscula, unha minúscula, un número e alomenos oito caracteres, do contrario o docker finalizará.
- Porto ao que conectarse de forma exterior: `41433`: Elíxese este porto posto que o habitual `1433` está bloqueado no contorno que empregamos por algúns filtros automáticos que non se pode abrir no grupo de seguridade.

~~~~
docker run \
 -e "ACCEPT_EULA=Y" \
 -e "MSSQL_SA_PASSWORD=Abc12300" \
 -e "MSSQL_PID=Evaluation" \
 -p 41433:1433  \
 --name sqlpreview \
 --hostname sqlpreview \
 -d mcr.microsoft.com/mssql/server:2022-preview-ubuntu-22.04
~~~~

