


Comandos esenciales para instalar y configurar un servidor Linux 
----------------------------------------------------------------------------------------------

Este documento contiene SOLO comandos y una breve descripción de para qué sirve cada uno.  
Puedes guardarlo como .txt en tu Bloc de notas.

============================================================
1) Actualización inicial del sistema
============================================================
sudo apt update
    # Actualiza la lista de paquetes disponibles.

sudo apt upgrade -y
    # Instala las actualizaciones nuevas de todos los paquetes.

sudo apt install -y curl wget git
    # Instala herramientas básicas útiles en cualquier servidor.

============================================================
2) Usuarios y privilegios
============================================================
sudo adduser nombre_usuario
    # Crea un nuevo usuario.

sudo usermod -aG sudo nombre_usuario
    # Agrega el usuario al grupo sudo para darle permisos administrativos.

sudo passwd nombre_usuario
    # Cambia o asigna contraseña al usuario.

============================================================
3) Configuración de red
============================================================
ip a
    # Muestra las interfaces de red y sus direcciones IP.

sudo nano /etc/netplan/01-netcfg.yaml
    # Edita la configuración de red para establecer IP estática si es necesario.

sudo netplan apply
    # Aplica los cambios de red.

============================================================
4) OpenSSH (acceso remoto)
============================================================
sudo apt install -y openssh-server
    # Instala el servidor SSH.

sudo systemctl enable ssh
    # Habilita SSH para que inicie automáticamente.

sudo systemctl start ssh
    # Inicia el servicio SSH.

sudo nano /etc/ssh/sshd_config
    # Configura ajustes como puerto, desactivar root login, etc.

sudo systemctl restart ssh
    # Reinicia SSH para aplicar cambios.

ssh-keygen -t ed25519
    # Genera claves SSH locales.

ssh-copy-id usuario@IP
    # Copia clave pública al servidor para usar acceso sin contraseña.

============================================================
5) Firewall UFW
============================================================
sudo apt install -y ufw
    # Instala el firewall UFW.

sudo ufw allow 22/tcp
    # Permite el acceso SSH.

sudo ufw allow 80/tcp
    # Permite tráfico HTTP.

sudo ufw allow 443/tcp
    # Permite tráfico HTTPS.

sudo ufw enable
    # Habilita el firewall.

sudo ufw status verbose
    # Muestra reglas activas.

============================================================
6) Servidor web (Nginx)
============================================================
sudo apt install -y nginx
    # Instala el servidor web Nginx.

sudo systemctl enable nginx
    # Habilita Nginx.

sudo systemctl start nginx
    # Arranca Nginx.

sudo nano /etc/nginx/sites-available/misitio
    # Crea configuración para un sitio personalizado.

sudo ln -s /etc/nginx/sites-available/misitio /etc/nginx/sites-enabled/
    # Habilita la configuración del sitio.

sudo nginx -t
    # Prueba la sintaxis del archivo de configuración.

sudo systemctl restart nginx
    # Aplica los cambios.

============================================================
7) PHP y módulos
============================================================
sudo apt install -y php-fpm php-mysql
    # Instala PHP y módulo para bases de datos MySQL/MariaDB.

sudo systemctl restart php*-fpm
    # Reinicia PHP-FPM.

============================================================
8) MariaDB (Base de datos)
============================================================
sudo apt install -y mariadb-server
    # Instala MariaDB.

sudo mysql_secure_installation
    # Asistente para proteger la base de datos.

sudo mariadb
    # Entra a la consola SQL.

CREATE DATABASE miapp;
    # Crea una base de datos.

CREATE USER 'miusuario'@'localhost' IDENTIFIED BY 'contraseña';
    # Crea un usuario para la BD.

GRANT ALL PRIVILEGES ON miapp.* TO 'miusuario'@'localhost';
    # Da permisos al usuario.

FLUSH PRIVILEGES;
    # Aplica los cambios en permisos.

EXIT;

============================================================
9) Certbot (HTTPS)
============================================================
sudo apt install -y certbot python3-certbot-nginx
    # Instala Certbot.

sudo certbot --nginx -d dominio.com -d www.dominio.com
    # Obtiene certificado SSL/HTTPS.

============================================================
10) SFTP seguro
============================================================
sudo nano /etc/ssh/sshd_config
    # Permite configurar usuarios restringidos a SFTP.

sudo systemctl restart ssh
    # Aplica cambios.

============================================================
11) Seguridad adicional
============================================================
sudo apt install -y fail2ban
    # Instala fail2ban para bloquear intentos de login maliciosos.

sudo apt install -y unattended-upgrades
    # Permite actualizaciones automáticas.

============================================================
12) Backups
============================================================
rsync -avz /var/www/ usuario@IP:/ruta/backup/
    # Copia archivos de manera segura a otro servidor.

============================================================
13) Ver servicios y puertos
============================================================
sudo ss -tulpn
    # Muestra los servicios que están escuchando en puertos.

journalctl -xe
    # Muestra errores del sistema.

sudo tail -n 50 /var/log/auth.log
    # Ve los últimos registros de autenticación.

============================================================

FIN DEL README.
"""

path = "/mnt/data/README_Comandos_Servidor_Linux.txt"
with open(path, "w", encoding="utf-8") as f:
    f.write(content)

print("Archivo creado en:", path)
print("Descargar desde: sandbox:/mnt/data/README_Comandos_Servidor_Linux.txt")
