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
