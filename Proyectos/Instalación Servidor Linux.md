#  README - Comandos Esenciales para Servidor Linux

Este documento contiene comandos organizados por categorías para la instalación y configuración de un servidor Linux. Cada comando incluye una breve descripción de su función.

---

##  1. Actualización del Sistema

bash

sudo apt update
# Actualiza la lista de paquetes disponibles

sudo apt upgrade -y
# Instala las actualizaciones de todos los paquetes

sudo apt install -y curl wget git
# Instala herramientas básicas útiles

---

##  2. Usuarios y Privilegios

bash

sudo adduser nombre_usuario
# Crea un nuevo usuario

sudo usermod -aG sudo nombre_usuario
# Agrega el usuario al grupo sudo para permisos administrativos

sudo passwd nombre_usuario
# Cambia o asigna contraseña al usuario

---

##  3. Configuración de Red

bash

ip a
# Muestra las interfaces de red y direcciones IP

sudo nano /etc/netplan/01-netcfg.yaml
# Edita la configuración de red para IP estática

sudo netplan apply
# Aplica los cambios de red

---

##  4. OpenSSH (Acceso Remoto)

bash

sudo apt install -y openssh-server
# Instala el servidor SSH

sudo systemctl enable ssh
# Habilita SSH para inicio automático

sudo systemctl start ssh
# Inicia el servicio SSH

sudo nano /etc/ssh/sshd_config
# Configura ajustes como puerto, desactivar root login, etc.

sudo systemctl restart ssh
# Reinicia SSH para aplicar cambios

ssh-keygen -t ed25519
# Genera claves SSH locales

ssh-copy-id usuario@IP
# Copia clave pública al servidor para acceso sin contraseña

---

##  5. Firewall UFW

bash

sudo apt install -y ufw
# Instala el firewall UFW

sudo ufw allow 22/tcp
# Permite el acceso SSH

sudo ufw allow 80/tcp
# Permite tráfico HTTP

sudo ufw allow 443/tcp
# Permite tráfico HTTPS

sudo ufw enable
# Habilita el firewall

sudo ufw status verbose
# Muestra reglas activas

---

##  6. Servidor Web (Nginx)

bash

sudo apt install -y nginx
# Instala el servidor web Nginx

sudo systemctl enable nginx
# Habilita Nginx

sudo systemctl start nginx
# Arranca Nginx

sudo nano /etc/nginx/sites-available/misitio
# Crea configuración para sitio personalizado

sudo ln -s /etc/nginx/sites-available/misitio /etc/nginx/sites-enabled/
# Habilita la configuración del sitio

sudo nginx -t
# Prueba la sintaxis del archivo de configuración

sudo systemctl restart nginx
# Aplica los cambios

---

##  7. PHP y Módulos

bash

sudo apt install -y php-fpm php-mysql
# Instala PHP y módulo para bases de datos MySQL/MariaDB

sudo systemctl restart php*-fpm
# Reinicia PHP-FPM

---

##  8. MariaDB (Base de Datos)

bash

sudo apt install -y mariadb-server
# Instala MariaDB

sudo mysql_secure_installation
# Asistente para proteger la base de datos

sudo mariadb
# Entra a la consola SQL

**Comandos dentro de MariaDB:**

sql

CREATE DATABASE miapp;
# Crea una base de datos

CREATE USER 'miusuario'@'localhost' IDENTIFIED BY 'contraseña';
# Crea un usuario para la BD

GRANT ALL PRIVILEGES ON miapp.* TO 'miusuario'@'localhost';
# Da permisos al usuario

FLUSH PRIVILEGES;
# Aplica los cambios en permisos

EXIT;

---

##  9. Certbot (HTTPS)

bash

sudo apt install -y certbot python3-certbot-nginx
# Instala Certbot

sudo certbot --nginx -d dominio.com -d www.dominio.com
# Obtiene certificado SSL/HTTPS

---

##  10. SFTP Seguro

bash

sudo nano /etc/ssh/sshd_config
# Permite configurar usuarios restringidos a SFTP

sudo systemctl restart ssh
# Aplica cambios

---

## 11. Seguridad Adicional

bash

sudo apt install -y fail2ban
# Instala fail2ban para bloquear intentos de login maliciosos

sudo apt install -y unattended-upgrades
# Permite actualizaciones automáticas

---

## 12. Backups

bash

rsync -avz /var/www/ usuario@IP:/ruta/backup/
# Copia archivos de manera segura a otro servidor

---

##  13. Ver Servicios y Puertos

bash

sudo ss -tulpn
# Muestra servicios escuchando en puertos

journalctl -xe
# Muestra errores del sistema

sudo tail -n 50 /var/log/auth.log
# Muestra últimos registros de autenticación

---

##  Orden Recomendado de Ejecución

1. **Actualización del sistema** (Sección 1)
    
2. **Configuración de usuarios** (Sección 2)
    
3. **Configuración de red** (Sección 3)
    
4. **Instalación de SSH** (Sección 4)
    
5. **Configuración de firewall** (Sección 5)
    
6. **Servicios web** (Secciones 6, 7)
    
7. **Base de datos** (Sección 8)
    
8. **Certificados SSL** (Sección 9)
    
9. **Seguridad adicional** (Sección 11)