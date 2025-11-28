
# Instalación y Configuración de Wekan con Snap

Este repositorio contiene los pasos para **instalar y configurar Wekan** en un servidor Ubuntu utilizando Snap. Wekan es una aplicación de tablero Kanban de código abierto, similar a Trello.

---

## Requisitos Previos

- Servidor con Ubuntu 20.04 o superior.
- Usuario con permisos sudo.
- Firewall configurado (opcional, pero recomendado).

---

```bash

1. Instalar snapd

sudo apt update
sudo apt install snapd -y


2. Instalar Wekan
sudo snap install wekan


Esto instalará Wekan junto con todas sus dependencias, incluyendo MongoDB.

3. Configurar puerto y URL

Cambia TU_IP_SERVIDOR por la IP de tu servidor. En este ejemplo usamos el puerto 3001:

sudo snap set wekan port='3001'
sudo snap set wekan root-url="http://TU_IP_SERVIDOR:3001"

4. Reiniciar servicios

Para aplicar los cambios, reinicia los servicios de Wekan y MongoDB:

sudo systemctl restart snap.wekan.mongodb
sudo systemctl restart snap.wekan.wekan

5. Abrir puerto en el firewall

Si estás usando ufw, permite el tráfico en el puerto configurado:

sudo ufw allow 3001/tcp
sudo ufw reload

6. Verificar el estado de los servicios

Comprueba que Wekan y MongoDB estén activos:

systemctl status snap.wekan.wekan --no-pager
systemctl status snap.wekan.mongodb --no-pager


Comprueba que el puerto esté escuchando:

ss -tunelp | grep 3001

7. Acceder a Wekan

Abre un navegador y escribe la URL configurada:

http://TU_IP_SERVIDOR:3001

8. Crear usuario administrador

Al acceder por primera vez, crea un usuario administrador para gestionar tus tableros y usuarios.
