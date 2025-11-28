### **1. INSTALACIÓN DE DOCKER EN WINDOWS**

#### **Paso a Paso:**

1. **Descargar Docker Desktop:**
    
    - Visitar [docker.com](https://www.docker.com/)
        
    - Descargar `Docker Desktop for Windows`
        
    - Ejecutar el instalador
        
2. **Habilitar WSL 2:**
    
    - Durante la instalación, verificar que WSL 2 esté habilitado
        
    - Si no lo está, seguir las instrucciones del instalador
        
3. **Configurar WSL Integration:**
    
    - Abrir Docker Desktop
        
    -  **Settings → Resources → WSL Integration**
        
    -  Activar **Ubuntu-24.04**
        
    -  Reiniciar el equipo
        

---

### **2. ¿QUÉ ES WSL Y POR QUÉ DOCKER LO NECESITA?**

**WSL (Windows Subsystem for Linux):**

- Subsistema que permite ejecutar entorno Linux en Windows
    
- Proporciona compatibilidad con herramientas y aplicaciones Linux
    

**¿Por qué Docker lo necesita?**

- Docker está basado en tecnologías de kernel Linux
    
- WSL proporciona el kernel Linux necesario en Windows
    
- Permite ejecutar contenedores Linux de forma nativa
    

---

### **3. EXAMINAR EL CATÁLOGO DE IMÁGENES**

**Desde Docker Hub:**

bash

# Buscar imágenes disponibles
docker search ubuntu
docker search nginx

**Desde navegador:**

- Visitar [hub.docker.com](https://hub.docker.com/)
    
- Buscar imágenes oficiales (verified)
    

---

### **4. CREAR CONTENEDORES UBUNTU**

#### **Contenedor básico:**

bash

docker run -it ubuntu

#### **Contenedor con nombre específico:**

bash

docker run -it --name mi-ubuntu ubuntu

---

### **5. INSTALAR PORTAINER**

#### **Desde Docker Desktop:**

1. **Buscar "portainer" en Marketplace**
    
2. **Hacer clic en "Install"**
    

#### **Desde línea de comandos:**

bash

docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer

**Acceso:** `http://localhost:9000`

---

### **6. VOLÚMENES PERSISTENTES**

#### **Crear volumen:**

bash

docker volume create mi-volumen

#### **Contenedor con volumen persistente:**

bash

docker run -it --name ubuntu-persistente -v mi-volumen:/datos ubuntu

---

### **7. CONTENEDOR HTTPD EN PUERTO 8080**

bash

docker run -d --name mi-web -p 8080:80 httpd

**Verificar:** `http://localhost:8080`

---

### **8. INSTALAR NANO Y MODIFICAR INDEX**

#### **Acceder al contenedor:**

bash

docker exec -it mi-web bash

#### **Dentro del contenedor:**

bash

apt update
apt install nano -y
cd /usr/local/apache2/htdocs
nano index.html

#### **Modificar contenido:**

html

<html>
<body>
<h1>¡Mi sitio web personalizado!</h1>
</body>
</html>

---

### **9. COMANDOS ESENCIALES - EJEMPLOS Y DESCRIPCIÓN**

#### **Descargar imágenes:**

bash

docker pull ubuntu              # Descarga última versión
docker pull ubuntu:20.04        # Descarga versión específica

#### **Crear y ejecutar contenedores:**

bash

docker run -it ubuntu                          # Contenedor interactivo
docker run -it --name mi-ubuntu ubuntu         # Con nombre específico
docker run -it -v /datos-persistentes ubuntu   # Con volumen interno
docker run -it -v mi-volumen:/datos ubuntu     # Con volumen nombrado
docker run -it -v C:\datos:/app ubuntu         # Con volumen local

#### **Gestionar volúmenes:**

bash

docker volume create mi-volumen        # Crear volumen
docker volume ls                       # Listar volúmenes

#### **Consultar información:**

bash

docker ps          # Contenedores activos
docker ps -a       # Todos los contenedores
docker images      # Imágenes descargadas

#### **Crear imágenes personalizadas:**

bash

docker commit mi-ubuntu mi-imagen-personalizada

#### **Operaciones con contenedores:**

bash

docker start mi-ubuntu              # Iniciar contenedor
docker exec -it mi-ubuntu bash      # Ejecutar comando dentro
docker start -ai mi-ubuntu          # Iniciar y conectar
docker rm mi-ubuntu                 # Eliminar contenedor
docker container prune              # Eliminar todos los contenedores detenidos

---

### **10. EJEMPLO PRÁCTICO COMPLETO**

bash

# Descargar imagen
docker pull ubuntu:22.04

# Crear volumen
docker volume create datos-app

# Crear contenedor persistente
docker run -it --name mi-app -v datos-app:/app ubuntu:22.04

# Dentro del contenedor:
apt update && apt install -y python3
echo "print('Hola desde Docker')" > /app/app.py
python3 /app/app.py