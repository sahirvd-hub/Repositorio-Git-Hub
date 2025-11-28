
#  README – Instalación de phpMyAdmin y Panel de Control Webmin en Servidor

Este documento explica el proceso completo para instalar y configurar:

1. Servicios básicos: Apache, MySQL y PHP
    
2. phpMyAdmin para gestión de bases de datos
    
3. Webmin para administración del servidor vía web



##  1. Instalación de Servicios Base

Ejecuta los siguientes comandos para instalar Apache, MySQL y PHP:

bash

sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql -y

---

##  2. Verificación de Servicios

Comprueba que los servicios estén activos:

bash

sudo systemctl status apache2
sudo systemctl status mysql
php -r "echo 'PHP is working';"; echo

---

##  3. Configuración de MySQL

Accede a MySQL y configura la política de contraseñas:

bash

sudo mysql -u root -p

Dentro de MySQL:

sql

INSTALL PLUGIN validate_password SONAME 'validate_password.so';
SET GLOBAL validate_password_policy = MEDIUM;
SHOW VARIABLES LIKE 'validate_password%';

---

##  4. Creación de Usuario para phpMyAdmin

Crea un usuario con todos los privilegios:

sql

CREATE USER 'phpmyadmin'@'localhost' IDENTIFIED BY 'TuContraseñaSegura';
GRANT ALL PRIVILEGES ON phpmyadmin.* TO 'phpmyadmin'@'localhost';
FLUSH PRIVILEGES;
EXIT;

Reinicia MySQL:

bash

sudo systemctl restart mysql

---

##  5. Instalación de phpMyAdmin

Instala phpMyAdmin y extensiones PHP necesarias:

bash

sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl

Durante la instalación:

- Selecciona **apache2** como servidor web.
    
- Elige **No** para la configuración automática de la base de datos.
    

---

##  6. Configuración de Apache para phpMyAdmin

Habilita la configuración y reinicia Apache:

bash

sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
sudo a2enconf phpmyadmin
sudo systemctl restart apache2

---

##  7. Mejora de Seguridad en MySQL

Edita el archivo de configuración de MySQL:

bash

sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

Asegúrate de que las siguientes líneas estén configuradas para solo conexiones locales:

text

bind-address = 127.0.0.1
mysqlx-bind-address = 127.0.0.1

Opcionalmente, bloquea el puerto MySQL en el firewall:

bash

sudo ufw deny 3306/tcp

---

## 8. Acceso a phpMyAdmin

Abre tu navegador y visita:

text

http://IP_DEL_SERVIDOR/phpmyadmin

Inicia sesión con el usuario `phpmyadmin` y la contraseña que definiste.

---

## 9. Instalación de Webmin (Panel de Control)

Instala curl y cambia a root:

bash

sudo apt install curl
sudo su

Descarga y ejecuta el script de instalación:

bash

curl -o setup-repos.sh https://raw.githubusercontent.com/webmin/webmin/master/setup-repos.sh
sh setup-repos.sh

Instala Webmin:

bash

sudo apt install webmin --install-recommends

---

##  10. Acceso a Webmin

Abre tu navegador y visita:

text

https://IP_DEL_SERVIDOR:10000/

Usa las credenciales de tu servidor para iniciar sesión.

---

##  Resumen Completo

Una vez completados todos los pasos, tendrás:

-  Servidor web Apache funcionando
    
-  MySQL con política de seguridad mejorada
    
-  phpMyAdmin accesible vía web
    
- Webmin para administración gráfica del servidor
    

Ambas herramientas estarán disponibles desde cualquier navegador web dentro de tu red.