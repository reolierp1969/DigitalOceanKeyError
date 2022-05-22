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

## 4. Instalar wkhtmltopdf
wkhtmltopdf es un conjunto de herramientas de línea de comandos de código abierto para renderizar páginas HTML en PDF y varios formatos de imagen. Para imprimir informes PDF en Odoo, deberá instalar el paquete wkhtmltox.

La versión de wkhtmltopdf incluida en los repositorios de Ubuntu no admite encabezados ni pies de página. La versión recomendada para Odoo es 0.12.5. Descargaremos e instalaremos el paquete desde Github:

## 5. Instalar y configurar Odoo 15
Instalaremos Odoo desde la fuente dentro de un entorno virtual de Python aislado.
Primero, cambie al usuario "odoo15":

**_sudo su - odoo15_**

Clona el código fuente de Odoo 15 desde GitHub:

**_git clone https://www.github.com/odoo/odoo --depth 1 --branch 15.0 /opt/odoo15/odoo_**

Cree un nuevo entorno virtual de Python para Odoo:

**_cd /opt/odoo15_**

**_python3 -m venv odoo-venv_**

Activar el entorno virtual:

**_source odoo-venv/bin/activate_**

Las dependencias de Odoo se especifican en el archivo require.txt. Instale todos los módulos de Python necesarios con pip3:

**_pip3 install wheel_**

**_pip3 install -r odoo/requirements.txt_**

Una vez hecho esto, desactive el entorno escribiendo:

**_deactivate_**

Cree un nuevo directorio, un directorio separado para los complementos de terceros:

**_mkdir /opt/odoo15/odoo-custom-addons_**

Luego agregaremos este directorio al parámetro addons_path. Este parámetro define una lista de directorios en la que Odoo busca módulos.




Introducción
Odoo es un popular conjunto de aplicaciones comerciales de código abierto que ayuda a las empresas a administrar y ejecutar sus negocios. Incluye una amplia gama de aplicaciones como CRM, comercio electrónico, creación de sitios web, facturación, contabilidad, fabricación, almacén, gestión de proyectos, inventario y más, todo perfectamente integrado.

Odoo se puede instalar de diferentes formas, según el caso de uso y las tecnologías disponibles. La forma más fácil y rápida de instalar Odoo es utilizar los repositorios APT oficiales de Odoo .


La instalación de Odoo en un entorno virtual o la implementación como contenedor Docker le brinda más control sobre la aplicación y le permite ejecutar múltiples instancias de Odoo en el mismo sistema.


Este artículo cubre la instalación e implementación de Odoo 15 dentro de un entorno virtual Python en Ubuntu 20.04. Descargaremos Odoo del repositorio oficial de GitHub y usaremos Nginx como proxy inverso.

Instalar dependencias
El primer paso es instalar Git , Pip , Node.js y las herramientas necesarias para compilar Odoo:

sudo apt update
sudo apt install git python3-pip build-essential wget python3-dev python3-venv \
    python3-wheel libfreetype6-dev libxml2-dev libzip-dev libldap2-dev libsasl2-dev \
    python3-setuptools node-less libjpeg-dev zlib1g-dev libpq-dev \
    libxslt1-dev libldap2-dev libtiff5-dev libjpeg8-dev libopenjp2-7-dev \
    liblcms2-dev libwebp-dev libharfbuzz-dev libfribidi-dev libxcb1-dev
Crea un usuario del sistema

Ejecutar Odoo como root presenta un gran riesgo de seguridad. Crearemos un nuevo usuario y grupo del sistema con el directorio de inicio /opt/odoo15que ejecutará el servicio Odoo. Para hacer esto, ejecute el siguiente comando:

sudo useradd -m -d /opt/odoo15 -U -r -s /bin/bash odoo15
Puede nombrar al usuario como desee, siempre que cree un usuario de PostgreSQL con el mismo nombre.

Instalar y configurar PostgreSQL
Odoo usa PostgreSQL como el backend de la base de datos. PostgreSQL está incluido en los repositorios estándar de Ubuntu. La instalación es sencilla:

sudo apt install postgresql
Una vez que el servicio esté instalado, cree un usuario de PostgreSQL con el mismo nombre que el usuario del sistema creado anteriormente. En este ejemplo, eso es odoo15:

sudo su - postgres -c "createuser -s odoo15"
Instalar wkhtmltopdf

wkhtmltopdf es un conjunto de herramientas de línea de comandos de código abierto para renderizar páginas HTML en PDF y varios formatos de imagen. Para imprimir informes PDF en Odoo, deberá instalar el paquete wkhtmltox.

La versión de wkhtmltopdf incluida en los repositorios de Ubuntu no admite encabezados ni pies de página. La versión recomendada para Odoo es 0.12.5. Descargaremos e instalaremos el paquete desde Github:

sudo wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.bionic_amd64.deb
Una vez descargado el archivo, instálelo escribiendo:

sudo apt install ./wkhtmltox_0.12.5-1.bionic_amd64.deb
Instalar y configurar Odoo 15
Instalaremos Odoo desde la fuente dentro de un entorno virtual de Python aislado.

Primero, cambie al usuario "odoo15":


sudo su - odoo15
Clona el código fuente de Odoo 15 desde GitHub:

git clone https://www.github.com/odoo/odoo --depth 1 --branch 15.0 /opt/odoo15/odoo
Cree un nuevo entorno virtual de Python para Odoo:

cd /opt/odoo15
python3 -m venv odoo-venv
Activar el entorno virtual:

source odoo-venv/bin/activate
Las dependencias de Odoo se especifican en el archivo require.txt. Instale todos los módulos de Python necesarios con pip3:

pip3 install wheel
pip3 install -r odoo/requirements.txt
Si se produce un error de compilación durante la instalación, asegúrese de que Installing Prerequisitesestén instaladas todas las dependencias necesarias enumeradas en la sección .

Una vez hecho esto, desactive el entorno escribiendo:

deactivate
Cree un nuevo directorio, un directorio separado para los complementos de terceros:

mkdir /opt/odoo15/odoo-custom-addons
Luego agregaremos este directorio al parámetro addons_path. Este parámetro define una lista de directorios en la que Odoo busca módulos.

Regrese a su usuario de sudo:

**_exit_**
