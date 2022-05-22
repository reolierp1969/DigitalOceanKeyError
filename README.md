# INSTALACION DE ODOO15 EN UN ENTORNO VIRTUAL PYTHON EN UBUNTU 20.04
Este tutorial cubre la instalación e implementación de Odoo 15 dentro de un entorno virtual Python en Ubuntu 20.04. Descargaremos Odoo del repositorio oficial de GitHub y usaremos Nginx como proxy inverso.
## 1. Instalar dependencias
El primer paso es instalar Git , Pip , Node.js y las herramientas necesarias para compilar Odoo:

**_sudo apt update_**

**_sudo apt install git python3-pip build-essential wget python3-dev python3-venv \
    python3-wheel libfreetype6-dev libxml2-dev libzip-dev libldap2-dev libsasl2-dev \
    python3-setuptools node-less libjpeg-dev zlib1g-dev libpq-dev \
    libxslt1-dev libldap2-dev libtiff5-dev libjpeg8-dev libopenjp2-7-dev \
    liblcms2-dev libwebp-dev libharfbuzz-dev libfribidi-dev libxcb1-dev_**
    
## 2. Crea un usuario del sistema
Ejecutar Odoo como root presenta un gran riesgo de seguridad. Crearemos un nuevo usuario y grupo del sistema con el directorio de inicio /opt/odoo15 que ejecutará el servicio Odoo. Para hacer esto, ejecute el siguiente comando:

**_sudo useradd -m -d /opt/odoo15 -U -r -s /bin/bash odoo15_**

Puede nombrar al usuario como desee, siempre que cree un usuario de PostgreSQL con el mismo nombre.

## 3. Instalar y configurar PostgreSQL
Odoo usa PostgreSQL como el backend de la base de datos. PostgreSQL está incluido en los repositorios estándar de Ubuntu. La instalación es sencilla:

**_sudo apt install postgresql_**

Una vez que el servicio esté instalado, cree un usuario de PostgreSQL con el mismo nombre que el usuario del sistema creado anteriormente. En este ejemplo, eso es odoo15:

**_sudo su - postgres -c "createuser -s odoo15"_**

