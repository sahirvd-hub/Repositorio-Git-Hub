#  Instalación y Configuración de Wekan con Snap

Este documento contiene los pasos completos para **instalar y configurar Wekan** en un servidor Ubuntu utilizando Snap. Wekan es una aplicación de tablero Kanban de código abierto, similar a Trello.

---

##  Requisitos Previos

- **Sistema Operativo:** Ubuntu 20.04 o superior
    
- **Permisos:** Usuario con privilegios sudo
    
- **Red:** Acceso a internet para descargar paquetes
    
- **Firewall:** Configurado (opcional pero recomendado)
    

---

## Pasos de Instalación Rápida

### **1. Actualizar sistema e instalar Snap**

bash

sudo apt update
sudo apt install snapd -y

Actualiza los repositorios e instala el gestor de paquetes Snap.

### **2. Instalar Wekan**

bash

sudo snap install wekan

Instala Wekan junto con todas sus dependencias, incluyendo MongoDB.

### **3. Configurar puerto y URL**

bash

sudo snap set wekan port='3001'
sudo snap set wekan root-url="http://192.168.1.100:3001"

**Nota:** Reemplaza `192.168.1.100` con la IP real de tu servidor.

### **4. Reiniciar servicios**

bash

sudo systemctl restart snap.wekan.mongodb
sudo systemctl restart snap.wekan.wekan

Reinicia los servicios para aplicar los cambios de configuración.

### **5. Configurar firewall**

bash

sudo ufw allow 3001/tcp
sudo ufw reload

Habilita el puerto 3001 en el firewall.

---

##  Verificación de la Instalación

### **Comprobar estado de servicios**

bash

systemctl status snap.wekan.wekan --no-pager
systemctl status snap.wekan.mongodb --no-pager

Verifica que ambos servicios estén activos y funcionando.

### **Verificar puerto en escucha**

bash

ss -tunelp | grep 3001

Confirma que el puerto 3001 esté escuchando conexiones.

---

##  Acceso a Wekan

### **URL de acceso:**

text

http://192.168.1.100:3001

### **Primer acceso:**

1. Abre tu navegador web
    
2. Navega a la URL configurada
    
3. **Registra el primer usuario administrador**
    
4. Comienza a crear tus tableros y organizar tareas
