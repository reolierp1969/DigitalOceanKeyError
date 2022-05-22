# INSTALACION DE ODOO15 EN UN ENTORNO VIRTUAL PYTHON EN UBUNTU 20.04
Este tutorial cubre la instalación e implementación de Odoo 15 dentro de un entorno virtual Python en Ubuntu 20.04. Descargaremos Odoo del repositorio oficial de GitHub y usaremos Nginx como proxy inverso.
## 1. Instalar dependencias
El primer paso es instalar Git , Pip , Node.js y las herramientas necesarias para compilar Odoo:

**_sudo apt update_**

sudo apt install git python3-pip build-essential wget python3-dev python3-venv \\  
    python3-wheel libfreetype6-dev libxml2-dev libzip-dev libldap2-dev libsasl2-dev \\  
    python3-setuptools node-less libjpeg-dev zlib1g-dev libpq-dev \\  
    libxslt1-dev libldap2-dev libtiff5-dev libjpeg8-dev libopenjp2-7-dev \\  
    liblcms2-dev libwebp-dev libharfbuzz-dev libfribidi-dev libxcb1-dev
    
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

**_wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb  
sudo apt install ./wkhtmltox_0.12.6-1.focal_amd64.deb_**

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

Regrese a su usuario de sudo:

**_exit_**

Cree un archivo de configuración con el siguiente contenido:

**_sudo nano /etc/odoo15.conf_**

**[options]  
; This is the password that allows database operations:  
admin_passwd = my_admin_passwd  
db_host = False  
db_port = False  
db_user = odoo15  
db_password = my_admin_passwd  
addons_path = /opt/odoo15/odoo/addons,/opt/odoo15/odoo-custom-addons**  

No olvide que debe cambiarse my_admin_passwd a algo más seguro en admin_passwd y db_password.  Estas contraseñas deben ser iguales.

## 6. Cree el archivo de la unidad Systemd
Un archivo de unidad es un archivo de configuración de estilo ini que contiene información sobre un servicio.

Abra su editor de texto y cree un archivo con nombre odoo15.service con el siguiente contenido:

**_sudo nano /etc/systemd/system/odoo15.service_**

**[Unit]  
Description=Odoo15  
Requires=postgresql.service  
After=network.target postgresql.service**  
  
**[Service]  
Type=simple  
SyslogIdentifier=odoo15  
PermissionsStartOnly=true  
User=odoo15  
Group=odoo15  
ExecStart=/opt/odoo15/odoo-venv/bin/python3 /opt/odoo15/odoo/odoo-bin -c /etc/odoo15.conf  
StandardOutput=journal+console**  
  
**[Install]  
WantedBy=multi-user.target** 


Notifica a systemd que existe un nuevo archivo de unidad:

**_sudo systemctl daemon-reload_**

Inicie el servicio Odoo y habilítelo para que se inicie en el inicio ejecutando:

**_sudo systemctl enable --now odoo15_**

Verifique que el servicio esté en funcionamiento:

**_sudo systemctl status odoo15_**

Abra su navegador y escriba: http://<your_domain_or_IP_address>:8069

## 7. Prueba la instalación

Suponiendo que la instalación se haya realizado correctamente, aparecerá la pantalla inicial para la creacion de la base de datos




