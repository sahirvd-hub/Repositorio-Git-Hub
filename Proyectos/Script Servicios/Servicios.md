# Guía Personal - Creación de Script de Instalación de Servicios

## Información del Desarrollador

- **Nombre:** Sahir VD
    
- **Usuario GitHub:** sahirvd-hub
    
- **Email:** sahirvd04@gmail.com
    
- **Especialidad:** Automatización de Infraestructura
    
- **Repositorio:** [https://github.com/sahirvd-hub/auto-install-scripts](https://github.com/sahirvd-hub/auto-install-scripts)
    

---

##  Fase 1: Configuración del Entorno de Desarrollo

### Verificar versión de Git

bash

git --version

_Salida esperada:_ `git version 2.43.0` o superior

### Configurar identidad global de Git

bash

git config --global user.name "sahirvd-hub"
git config --global user.email "sahirvd04@gmail.com"

### Verificar configuración

bash

git config --list

---

##  Fase 2: Estructura del Proyecto

### Crear directorio del proyecto

bash

mkdir auto-install-scripts
cd auto-install-scripts

### Estructura de archivos inicial:

text

auto-install-scripts/
├── scripts/
│   ├── servidores/
│   ├── bases-datos/
│   └── herramientas/
├── config/
├── docs/
└── tests/

---

##  Fase 3: Desarrollo del Script Principal

### Crear script de instalación (`installer.sh`)

bash

#!/bin/bash

# =============================================
# SCRIPT DE INSTALACIÓN AUTOMATIZADA - SAHIR VD
# =============================================

# Colores para output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m'

# Información del desarrollador
DEVELOPER="Sahir VD"
GITHUB_USER="sahirvd-hub"
EMAIL="sahirvd04@gmail.com"

mostrar_header() {
    echo -e "${BLUE}"
    echo "╔══════════════════════════════════════════════════════════════╗"
    echo "║           SISTEMA DE INSTALACIÓN AUTOMATIZADO                ║"
    echo "║                 Desarrollado por: $DEVELOPER                 ║"
    echo "║                   GitHub: $GITHUB_USER                       ║"
    echo "╚══════════════════════════════════════════════════════════════╝"
    echo -e "${NC}"
}

log_action() {
    echo -e "${GREEN}[✓] $1${NC}"
    logger -t "AutoInstall-Sahir" "$1"
}

log_error() {
    echo -e "${RED}[✗] $1${NC}"
    logger -t "AutoInstall-Sahir" "ERROR: $1"
}

actualizar_sistema() {
    log_action "Actualizando lista de paquetes..."
    sudo apt update
    
    log_action "Actualizando sistema..."
    sudo apt upgrade -y
}

instalar_servidor_web() {
    log_action "Instalando servidor web (Apache + Nginx)..."
    sudo apt install apache2 nginx -y
    
    # Configuración personalizada
    sudo systemctl enable apache2
    sudo systemctl enable nginx
    sudo systemctl start apache2
}

instalar_base_datos() {
    log_action "Instalando base de datos (MySQL + PostgreSQL)..."
    sudo apt install mysql-server postgresql postgresql-contrib -y
    
    # Configuración de seguridad
    sudo mysql_secure_installation
}

instalar_python_stack() {
    log_action "Instalando stack Python..."
    sudo apt install python3 python3-pip python3-venv -y
    
    # Instalar paquetes comunes
    pip3 install flask django requests numpy pandas
}

instalar_herramientas_desarrollo() {
    log_action "Instalando herramientas de desarrollo..."
    sudo apt install curl wget git vim htop net-tools -y
    
    # Docker
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    sudo usermod -aG docker $USER
}

instalar_monitoreo() {
    log_action "Instalando herramientas de monitoreo..."
    sudo apt install htop iotop nethogs -y
    
    # Configurar UFW
    sudo ufw allow ssh
    sudo ufw allow http
    sudo ufw allow https
    sudo ufw enable
}

generar_reporte() {
    log_action "Generando reporte de instalación..."
    echo "=== REPORTE DE INSTALACIÓN ===" > install_report.txt
    echo "Fecha: $(date)" >> install_report.txt
    echo "Desarrollador: $DEVELOPER" >> install_report.txt
    echo "Sistema: $(lsb_release -d | cut -f2)" >> install_report.txt
    
    # Verificar servicios instalados
    echo "--- Servicios Instalados ---" >> install_report.txt
    systemctl is-active apache2 && echo "Apache: ✅" >> install_report.txt
    systemctl is-active mysql && echo "MySQL: ✅" >> install_report.txt
    systemctl is-active postgresql && echo "PostgreSQL: ✅" >> install_report.txt
    docker --version && echo "Docker: ✅" >> install_report.txt
}

menu_principal() {
    while true; do
        mostrar_header
        echo -e "${YELLOW}Seleccione los componentes a instalar:${NC}"
        echo "1) Actualizar sistema completo"
        echo "2) Servidores web (Apache + Nginx)"
        echo "3) Bases de datos (MySQL + PostgreSQL)"
        echo "4) Stack Python y dependencias"
        echo "5) Herramientas de desarrollo"
        echo "6) Monitoreo y seguridad"
        echo "7) INSTALACIÓN COMPLETA"
        echo "8) Generar reporte"
        echo "9) Salir"
        echo
        
        read -p "Opción [1-9]: " opcion
        
        case $opcion in
            1) actualizar_sistema ;;
            2) instalar_servidor_web ;;
            3) instalar_base_datos ;;
            4) instalar_python_stack ;;
            5) instalar_herramientas_desarrollo ;;
            6) instalar_monitoreo ;;
            7) 
                actualizar_sistema
                instalar_servidor_web
                instalar_base_datos
                instalar_python_stack
                instalar_herramientas_desarrollo
                instalar_monitoreo
                generar_reporte
                ;;
            8) generar_reporte ;;
            9) 
                log_action "¡Instalación completada por $DEVELOPER!"
                exit 0
                ;;
            *) log_error "Opción inválida" ;;
        esac
        
        read -p "Presione Enter para continuar..."
    done
}

# Verificar si es ejecutado con sudo
if [ "$EUID" -ne 0 ]; then
    echo -e "${RED}Este script requiere privilegios de root. Ejecute con:${NC}"
    echo "sudo ./installer.sh"
    exit 1
fi

# Ejecutar menú principal
menu_principal

---

##  Fase 4: Configuración de Control de Versiones

### Inicializar repositorio Git

bash

git init
git branch -M main

### Crear archivo README.md personalizado

markdown

#  Script de Instalación Automatizada

**Desarrollador:** Sahir VD  
**Contacto:** sahirvd04@gmail.com  
**GitHub:** [sahirvd-hub](https://github.com/sahirvd-hub)

##  Descripción
Sistema de instalación automatizada para servidores Ubuntu/Linux que incluye:
- Servidores web (Apache + Nginx)
- Bases de datos (MySQL + PostgreSQL)
- Stack Python completo
- Herramientas de desarrollo
- Sistema de monitoreo

##  Características
- ✅ Interfaz de menú interactivo
- ✅ Logging de todas las acciones
- ✅ Configuraciones optimizadas
- ✅ Reportes automáticos
- ✅ Validación de permisos

##  Uso Rápido
```bash
chmod +x installer.sh
sudo ./installer.sh

## 📝 Licencia

Desarrollado por Sahir VD - Todos los derechos reservados.

text

### Crear archivo .gitignore

# Logs

*.log  
install_report.txt

# Temporales

tmp/  
temp/

# Configuraciones locales

config/local.yaml  
.env

# Backup

backup/  
*.bak

text

---

## 📤 Fase 5: Subida a GitHub

### Preparar archivos para el commit
```bash
git add .
git status

### Commit inicial

bash

git commit -m "feat: Sistema de instalación automatizada v1.0

- Menú interactivo para instalación de servicios
- Soporte para múltiples componentes
- Sistema de logging y reportes
- Configuraciones optimizadas para producción

Desarrollado por: Sahir VD
Email: sahirvd04@gmail.com"

### Configurar repositorio remoto

bash

git remote add origin https://github.com/sahirvd-hub/auto-install-scripts.git

### Verificar configuración

bash

git remote -v

### Primer push al repositorio

bash

git push -u origin main

---

##  Fase 6: Pruebas y Validación

### Script de pruebas (`test_installation.sh`)

bash

#!/bin/bash

# Script de pruebas para el instalador
echo " Ejecutando pruebas del sistema..."

# Verificar que el script principal existe
if [ -f "installer.sh" ]; then
    echo " Script principal encontrado"
else
    echo " Script principal no encontrado"
    exit 1
fi

# Verificar permisos de ejecución
if [ -x "installer.sh" ]; then
    echo " Permisos de ejecución configurados"
else
    chmod +x installer.sh
    echo " Permisos de ejecución asignados"
fi

# Verificar estructura de directorios
directories=("scripts" "config" "docs" "tests")
for dir in "${directories[@]}"; do
    if [ -d "$dir" ]; then
        echo " Directorio $dir encontrado"
    else
        echo "  Directorio $dir no existe"
    fi
done

echo " Pruebas completadas por Sahir VD"

### Ejecutar pruebas

bash

chmod +x test_installation.sh
./test_installation.sh

---

##  Fase 7: Documentación Adicional

### Crear documentación técnica (`docs/INSTALACION.md`)

markdown

#  Documentación Técnica

## Estructura del Proyecto

auto-install-scripts/  
├── installer.sh # Script principal  
├── README.md # Documentación principal  
├── test_installation.sh # Script de pruebas  
├── scripts/ # Scripts auxiliares  
│ ├── servidores/ # Configuraciones de servidores  
│ ├── bases-datos/ # Configuraciones de BD  
│ └── herramientas/ # Herramientas adicionales  
├── config/ # Archivos de configuración  
├── docs/ # Documentación  
└── tests/ # Scripts de prueba

text

## Flujo de Trabajo
1. **Desarrollo**: Crear/modificar scripts
2. **Pruebas**: Ejecutar test_installation.sh
3. **Commit**: git add . && git commit -m "mensaje"
4. **Push**: git push origin main

## Personalización
Para adaptar el script a tus necesidades:
1. Modificar las variables en `installer.sh`
2. Agregar nuevas funciones de instalación
3. Actualizar el menú principal
4. Probar los cambios localmente

---

##  Fase 8: Mantenimiento y Actualizaciones

### Comandos útiles para el desarrollo continuo

bash

# Actualizar repositorio local
git pull origin main

# Verificar estado del repositorio
git status

# Ver historial de commits
git log --oneline

# Agregar nuevos cambios
git add .
git commit -m "feat: Nueva funcionalidad agregada"
git push origin main

### Script de actualización automática (`update_script.sh`)

bash

#!/bin/bash
# Script de actualización automática
echo " Actualizando sistema de instalación..."
git pull origin main
chmod +x installer.sh test_installation.sh
echo " Sistema actualizado por Sahir VD"

---

##  Resumen de Comandos Esenciales

### Para ejecutar el proyecto completo:

bash

# Clonar repositorio
git clone https://github.com/sahirvd-hub/auto-install-scripts.git
cd auto-install-scripts

# Ejecutar instalador
sudo ./installer.sh

# Ejecutar pruebas
./test_installation.sh

### Para contribuir al proyecto:

bash

# Hacer fork del repositorio
# Clonar tu fork
git clone https://github.com/TU_USUARIO/auto-install-scripts.git

# Crear rama de feature
git checkout -b nueva-funcionalidad

# Desarrollar cambios y hacer commit
git add .
git commit -m "feat: Agrega nueva funcionalidad"

# Subir cambios
git push origin nueva-funcionalidad

# Crear Pull Request en GitHub