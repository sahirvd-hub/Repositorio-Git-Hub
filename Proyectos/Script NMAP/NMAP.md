# Manual de Creación - Script de Escaneo de Red Educativo

## Objetivo

Crear un script interactivo que facilite el uso de Nmap para fines educativos, con diferentes tipos de escaneos y consideraciones éticas.

---

##  Paso 1: Preparación del Entorno

### 1.1 Crear directorio de trabajo

bash

mkdir escaneos_red
cd escaneos_red

### 1.2 Verificar/Instalar Nmap

bash

# Verificar si Nmap está instalado
which nmap

# Si no está instalado, instalarlo
sudo apt update
sudo apt install nmap -y

---

##  Paso 2: Crear el Script Principal

### 2.1 Crear el archivo del script

bash

nano escaner_red.sh

### 2.2 Contenido del script:

bash

#!/bin/bash

# Script Educativo de Escaneo de Red
# Autor: [Tu Nombre]
# Fecha: $(date +%Y-%m-%d)
# Uso exclusivo para fines educativos

# Colores para la interfaz
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Función para mostrar advertencia legal
mostrar_advertencia() {
    echo -e "${RED}"
    echo "╔══════════════════════════════════════════════════════════════╗"
    echo "║                     ADVERTENCIA LEGAL                        ║"
    echo "╠══════════════════════════════════════════════════════════════╣"
    echo "║  • Solo escanee redes y dispositivos de su propiedad        ║"
    echo "║  • Obtenga autorización explícita antes de cualquier escaneo║"
    echo "║  • El uso indebido puede tener consecuencias legales        ║"
    echo "║  • Este script es solo para fines educativos                ║"
    echo "╚══════════════════════════════════════════════════════════════╝"
    echo -e "${NC}"
    read -p "Presiona Enter para aceptar y continuar..."
    clear
}

# Función principal del menú
menu_principal() {
    while true; do
        echo -e "${BLUE}"
        echo "┌────────────────────────────────────────────────────────────┐"
        echo "│                HERRAMIENTA DE ESCANEO EDUCATIVO            │"
        echo "├────────────────────────────────────────────────────────────┤"
        echo "│ 1) Descubrimiento de hosts activos                         │"
        echo "│ 2) Escaneo rápido de puertos comunes                       │"
        echo "│ 3) Escaneo completo de puertos                             │"
        echo "│ 4) Detección de servicios y versiones                      │"
        echo "│ 5) Identificación de sistema operativo                     │"
        echo "│ 6) Escaneo de vulnerabilidades básico                      │"
        echo "│ 7) Escaneo personalizado                                   │"
        echo "│ 8) Exportar resultados                                     │"
        echo "│ 9) Información del sistema                                 │"
        echo "│ 10) Salir                                                  │"
        echo "└────────────────────────────────────────────────────────────┘"
        echo -e "${NC}"
        
        read -p "Seleccione una opción [1-10]: " opcion
        
        case $opcion in
            1) descubrimiento_hosts ;;
            2) escaneo_rapido ;;
            3) escaneo_completo ;;
            4) deteccion_servicios ;;
            5) identificacion_so ;;
            6) escaneo_vulnerabilidades ;;
            7) escaneo_personalizado ;;
            8) exportar_resultados ;;
            9) info_sistema ;;
            10) salir_script ;;
            *) echo -e "${RED}Opción inválida. Intente nuevamente.${NC}" ;;
        esac
    done
}

# Función para descubrimiento de hosts
descubrimiento_hosts() {
    echo -e "${YELLOW}[+] Descubrimiento de hosts activos${NC}"
    read -p "Ingrese la red a escanear (ej: 192.168.1.0/24): " red
    read -p "Directorio para resultados [default: ./resultados]: " directorio
    directorio=${directorio:-./resultados}
    mkdir -p "$directorio"
    
    timestamp=$(date +"%Y%m%d_%H%M%S")
    archivo_salida="$directorio/hosts_activos_$timestamp.txt"
    
    echo -e "${GREEN}[+] Ejecutando escaneo de descubrimiento...${NC}"
    nmap -sn "$red" -oN "$archivo_salida"
    
    echo -e "${GREEN}[+] Resultados guardados en: $archivo_salida${NC}"
    preguntar_continuar
}

# Función para escaneo rápido
escaneo_rapido() {
    echo -e "${YELLOW}[+] Escaneo rápido de puertos comunes${NC}"
    read -p "Ingrese objetivo (IP o hostname): " objetivo
    read -p "Directorio para resultados [default: ./resultados]: " directorio
    directorio=${directorio:-./resultados}
    mkdir -p "$directorio"
    
    timestamp=$(date +"%Y%m%d_%H%M%S")
    archivo_salida="$directorio/escaneo_rapido_$timestamp.txt"
    
    echo -e "${GREEN}[+] Ejecutando escaneo rápido...${NC}"
    nmap -T4 -F "$objetivo" -oN "$archivo_salida"
    
    echo -e "${GREEN}[+] Resultados guardados en: $archivo_salida${NC}"
    preguntar_continuar
}

# Función para escaneo completo
escaneo_completo() {
    echo -e "${YELLOW}[+] Escaneo completo de puertos${NC}"
    read -p "Ingrese objetivo (IP o hostname): " objetivo
    read -p "Directorio para resultados [default: ./resultados]: " directorio
    directorio=${directorio:-./resultados}
    mkdir -p "$directorio"
    
    timestamp=$(date +"%Y%m%d_%H%M%S")
    archivo_salida="$directorio/escaneo_completo_$timestamp.txt"
    
    echo -e "${GREEN}[+] Ejecutando escaneo completo...${NC}"
    nmap -p- "$objetivo" -oN "$archivo_salida"
    
    echo -e "${GREEN}[+] Resultados guardados en: $archivo_salida${NC}"
    preguntar_continuar
}

# Función para detección de servicios
deteccion_servicios() {
    echo -e "${YELLOW}[+] Detección de servicios y versiones${NC}"
    read -p "Ingrese objetivo (IP o hostname): " objetivo
    read -p "Directorio para resultados [default: ./resultados]: " directorio
    directorio=${directorio:-./resultados}
    mkdir -p "$directorio"
    
    timestamp=$(date +"%Y%m%d_%H%M%S")
    archivo_salida="$directorio/servicios_$timestamp.txt"
    
    echo -e "${GREEN}[+] Ejecutando detección de servicios...${NC}"
    nmap -sV -sC "$objetivo" -oN "$archivo_salida"
    
    echo -e "${GREEN}[+] Resultados guardados en: $archivo_salida${NC}"
    preguntar_continuar
}

# Función para identificación de SO
identificacion_so() {
    echo -e "${YELLOW}[+] Identificación de sistema operativo${NC}"
    read -p "Ingrese objetivo (IP o hostname): " objetivo
    read -p "Directorio para resultados [default: ./resultados]: " directorio
    directorio=${directorio:-./resultados}
    mkdir -p "$directorio"
    
    timestamp=$(date +"%Y%m%d_%H%M%S")
    archivo_salida="$directorio/sistema_operativo_$timestamp.txt"
    
    echo -e "${GREEN}[+] Ejecutando identificación de SO...${NC}"
    nmap -O "$objetivo" -oN "$archivo_salida"
    
    echo -e "${GREEN}[+] Resultados guardados en: $archivo_salida${NC}"
    preguntar_continuar
}

# Función para escaneo de vulnerabilidades
escaneo_vulnerabilidades() {
    echo -e "${YELLOW}[+] Escaneo de vulnerabilidades básico${NC}"
    read -p "Ingrese objetivo (IP o hostname): " objetivo
    read -p "Directorio para resultados [default: ./resultados]: " directorio
    directorio=${directorio:-./resultados}
    mkdir -p "$directorio"
    
    timestamp=$(date +"%Y%m%d_%H%M%S")
    archivo_salida="$directorio/vulnerabilidades_$timestamp.txt"
    
    echo -e "${GREEN}[+] Ejecutando escaneo de vulnerabilidades...${NC}"
    nmap --script vuln "$objetivo" -oN "$archivo_salida"
    
    echo -e "${GREEN}[+] Resultados guardados en: $archivo_salida${NC}"
    preguntar_continuar
}

# Función para escaneo personalizado
escaneo_personalizado() {
    echo -e "${YELLOW}[+] Escaneo personalizado${NC}"
    read -p "Ingrese objetivo (IP o hostname): " objetivo
    read -p "Ingrese opciones personalizadas de Nmap: " opciones
    read -p "Directorio para resultados [default: ./resultados]: " directorio
    directorio=${directorio:-./resultados}
    mkdir -p "$directorio"
    
    timestamp=$(date +"%Y%m%d_%H%M%S")
    archivo_salida="$directorio/personalizado_$timestamp.txt"
    
    echo -e "${GREEN}[+] Ejecutando escaneo personalizado...${NC}"
    nmap $opciones "$objetivo" -oN "$archivo_salida"
    
    echo -e "${GREEN}[+] Resultados guardados en: $archivo_salida${NC}"
    preguntar_continuar
}

# Función para exportar resultados
exportar_resultados() {
    echo -e "${YELLOW}[+] Exportar resultados${NC}"
    read -p "Ingrese archivo a exportar: " archivo_entrada
    read -p "Formato de salida (xml, html, grepable): " formato
    
    if [[ ! -f "$archivo_entrada" ]]; then
        echo -e "${RED}[!] Archivo no encontrado${NC}"
        return
    fi
    
    case $formato in
        xml)
            nombre_salida="${archivo_entrada%.*}.xml"
            nmap -oX "$nombre_salida" -iL "$archivo_entrada" 2>/dev/null
            ;;
        html)
            nombre_salida="${archivo_entrada%.*}.html"
            nmap -oX temp.xml -iL "$archivo_entrada" 2>/dev/null
            xsltproc temp.xml -o "$nombre_salida" 2>/dev/null
            rm -f temp.xml
            ;;
        grepable)
            nombre_salida="${archivo_entrada%.*}.grep"
            nmap -oG "$nombre_salida" -iL "$archivo_entrada" 2>/dev/null
            ;;
        *)
            echo -e "${RED}[!] Formato no válido${NC}"
            return
            ;;
    esac
    
    echo -e "${GREEN}[+] Archivo exportado: $nombre_salida${NC}"
    preguntar_continuar
}

# Función para información del sistema
info_sistema() {
    echo -e "${YELLOW}[+] Información del sistema${NC}"
    echo "=== Versión de Nmap ==="
    nmap --version
    echo ""
    echo "=== Interfaces de red ==="
    ip addr show | grep inet
    echo ""
    echo "=== Espacio en disco ==="
    df -h
    preguntar_continuar
}

# Función para salir
salir_script() {
    echo -e "${GREEN}[+] Gracias por usar la herramienta educativa${NC}"
    echo -e "${YELLOW}[!] Recuerde: Use estas herramientas de forma responsable${NC}"
    exit 0
}

# Función para preguntar continuar
preguntar_continuar() {
    echo ""
    read -p "Presione Enter para continuar..."
    clear
}

# Ejecución principal
clear
mostrar_advertencia
menu_principal

---

## 🔧 Paso 3: Configurar Permisos y Ejecutar

### 3.1 Dar permisos de ejecución

bash

chmod +x escaner_red.sh

### 3.2 Ejecutar el script

bash

./escaner_red.sh

---

## Paso 4: Ejemplos de Uso

### Ejemplo 1: Descubrir hosts en red local

text

Seleccione: 1
Red: 192.168.1.0/24
Directorio: ./escaneos_2024

### Ejemplo 2: Escanear un servidor web

text

Seleccione: 4
Objetivo: 192.168.1.100
Directorio: ./resultados_servidor

### Ejemplo 3: Escaneo personalizado

text

Seleccione: 7
Objetivo: 10.0.0.50
Opciones: -sS -p 22,80,443 -A

---

##  Estructura de Directorios Resultante

text

escaneos_red/
├── escaner_red.sh
└── resultados/
    ├── hosts_activos_20241201_143022.txt
    ├── escaneo_rapido_20241201_143145.txt
    ├── servicios_20241201_143210.txt
    └── vulnerabilidades_20241201_143235.txt