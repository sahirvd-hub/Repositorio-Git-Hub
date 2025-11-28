#  README - Configuración de Servidor Web con Git y Banderas

Este documento explica el proceso completo para configurar un servidor web Apache, gestionar diferentes versiones de banderas usando Git, y visualizar el historial con gitk.

---

##  Parte 1: Configuración Inicial del Servidor

### **1. Actualizar el sistema**

bash

sudo apt update && sudo apt upgrade -y

Actualiza todos los repositorios y paquetes del sistema.

### **2. Instalar GitK**

bash

sudo apt install gitk

Instala la herramienta visual para Git.

### **3. Instalar Apache**

bash

sudo apt install apache2

Instala el servidor web Apache.

### **4. Verificar estado de Apache**

bash

sudo systemctl status apache2

Comprueba que Apache esté instalado y funcionando correctamente.

### **5. Navegar al directorio web**

bash

cd /var/www/html

Accede al directorio principal del servidor web.

### **6. Modificar permisos del directorio**

bash

sudo chown -R tu_usuario:tu_usuario /var/www/html

**Ejemplo:**

bash

sudo chown -R operadorssh1:operadorssh1 /var/www/html

Cambia los permisos para poder trabajar en el directorio.

### **7. Inicializar repositorio Git**

bash

git init

Crea un nuevo repositorio Git en el directorio.

### **8. Crear archivo HTML inicial**

bash

sudo nano index.html

Crea el archivo principal y pega este contenido:

html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bandera</title>
    <style>
        body {
            margin: 0;
            padding: 0;
        }
        .franja {
            height: 100px;
            width: 100%;
        }
    </style>
</head>
<body>
    <div class="franja" style="background-color: red;"></div>
    <div class="franja" style="background-color: yellow;"></div>
    <div class="franja" style="background-color: red;"></div>
</body>
</html>

### **9. Configurar Git**

bash

git config --global user.email "tu_email"
git config --global user.name "tu_usuario"

Configura tu identidad para los commits.

### **10. Primer commit**

bash

git add index.html
git commit -m "Añade la bandera inicial (España)"

Guarda los cambios iniciales en el repositorio.

---

##  Parte 2: Creación de Banderas y Commits

### **Bandera de Italia**

Modifica `index.html` con colores verde, blanco y rojo:

html

<div class="franja" style="background-color: green;"></div>
<div class="franja" style="background-color: white;"></div>
<div class="franja" style="background-color: red;"></div>

bash

git add index.html && git commit -m "Cambia los colores para la bandera de Italia"

### **Bandera de Francia**

Modifica `index.html` con colores azul, blanco y rojo:

html

<div class="franja" style="background-color: blue;"></div>
<div class="franja" style="background-color: white;"></div>
<div class="franja" style="background-color: red;"></div>

bash

git add index.html
git commit -m "Cambia los colores para la bandera de Francia"

### **Bandera de Alemania**

Modifica `index.html` con colores negro, rojo y amarillo:

html

<div class="franja" style="background-color: black;"></div>
<div class="franja" style="background-color: red;"></div>
<div class="franja" style="background-color: yellow;"></div>

bash

git add index.html
git commit -m "Cambia los colores para la bandera de Alemania"

---

##  Parte 3: Uso de Ramas

### **1. Rama para bandera de Italia**

bash

git checkout -b italia
sudo nano index.html  # Modifica con colores de Italia
git add index.html
git commit -m "Crea la bandera de Italia en la rama italia"

### **2. Rama para bandera de Francia**

bash

git checkout -b francia
sudo nano index.html  # Modifica con colores de Francia
git add index.html
git commit -m "Crea la bandera de Francia en la rama francia"

### **3. Rama para bandera de Alemania**

bash

git checkout -b alemania
sudo nano index.html  # Modifica con colores de Alemania
git add index.html
git commit -m "Crea la bandera de Alemania en la rama alemania"

---

##  Parte 4: Visualización con GitK

### **1. Abrir visualizador GitK**

bash

gitk --all

Desde el directorio `/var/www/html`, ejecuta este comando para visualizar todo el historial de commits y las ramas creadas.

---

## Comandos Útiles Adicionales

### **Navegar entre ramas:**

bash

git checkout nombre_rama

### **Ver ramas existentes:**

bash

git branch

### **Ver estado actual:**

bash

git status

### **Ver historial de commits:**

bash

git log --oneline --graph --all

---

##  Verificación Final

- **Servidor web:** Accede a `http://tu_ip_servidor` para ver la bandera actual
    
- **Repositorio Git:** Usa `gitk --all` para ver el historial completo
    
- **Ramas:** Verifica con `git branch` que tienes todas las ramas creadas