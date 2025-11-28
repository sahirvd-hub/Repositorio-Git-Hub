### **1. INSTALACIÓN DE JQ**

#### **¿Qué es jq?**

- **jq** es un procesador de JSON ligero y flexible para la línea de comandos
    
- Permite filtrar, transformar y manipular datos JSON directamente desde terminal
    

#### **Instalación:**

bash

sudo apt update
sudo apt install jq

---

### **2. CREAR Y MANIPULAR ARCHIVOS JSON**

#### **Crear directorio y archivo JSON:**

bash

mkdir EjJSON
cd EjJSON
nano datos.json

#### **Contenido de datos.json:**

json

{
  "nombre": "Ana",
  "edad": 30,
  "skills": ["bash", "python", "jq"],
  "direccion": {
    "ciudad": "Madrid",
    "cp": "28001"
  },
  "usuarios": [
    {"nombre": "Ana", "edad": 30},
    {"nombre": "Luis", "edad": 22}
  ]
}

---

### **3. COMANDOS BÁSICOS DE JQ**

#### **Mostrar JSON formateado:**

bash

jq . datos.json

#### **Obtener campo como texto (sin comillas):**

bash

jq -r '.nombre' datos.json
# Output: Ana

#### **Obtener número:**

bash

jq '.edad' datos.json
# Output: 30

#### **Campo anidado:**

bash

jq -r '.direccion.ciudad' datos.json
# Output: Madrid

#### **Primer elemento de array:**

bash

jq -r '.skills[0]' datos.json
# Output: bash

#### **Filtrar array de objetos:**

bash

jq -r '.usuarios[] | select(.edad > 25) | .nombre' datos.json
# Output: Ana

#### **Iterar elementos:**

bash

jq -r '.skills[]' datos.json | while read -r skill; do
  echo "Skill: $skill"
done
# Output:
# Skill: bash
# Skill: python
# Skill: jq

---

### **4. SCRIPT AVANZADO: SUPERSCRIPT.SH**

#### **Crear script:**

bash

nano superScript.sh

#### **Contenido completo del script:**

bash

#!/usr/bin/env bash
#
# Script: superScript.sh
# Descripción: Comprueba si jq está instalado y lee datos JSON
#

# Colores para output
verde="\e[32m"
rojo="\e[31m"
amarillo="\e[33m"
reset="\e[0m"

# --- Comprobar si jq está instalado ---
echo -e "${amarillo}Comprobando si jq está instalado...${reset}"

if ! command -v jq &> /dev/null; then
    echo -e "${rojo}jq no está instalado.${reset}"
    
    # Detectar sistema operativo
    if grep -qi "ubuntu" /etc/os-release || grep -qi "raspbian" /etc/os-release; then
        echo -e "${amarillo}Sistema Ubuntu/Raspbian detectado. Instalando jq...${reset}"
        sudo apt update && sudo apt install -y jq
    else
        echo -e "${rojo}Este script solo funciona en Ubuntu/Raspbian${reset}"
        echo "Instala jq manualmente: sudo apt install jq"
        exit 1
    fi
    
    # Verificar instalación
    if ! command -v jq &> /dev/null; then
        echo -e "${rojo}Error: jq no se pudo instalar${reset}"
        exit 1
    fi
else
    echo -e "${verde}jq ya está instalado.${reset}"
fi

# --- Verificar archivo JSON ---
json_file="datos.json"

if [ ! -f "$json_file" ]; then
    echo -e "${rojo}No se encuentra $json_file${reset}"
    echo "Crea un archivo con este contenido:"
    cat << EOF
{
  "nombre": "Ana",
  "edad": 30,
  "skills": ["bash", "python", "jq"],
  "direccion": {"ciudad": "Madrid", "cp": "28001"}
}
EOF
    exit 1
fi

# --- Extraer datos ---
nombre=$(jq -r '.nombre' "$json_file")
edad=$(jq -r '.edad' "$json_file")
ciudad=$(jq -r '.direccion.ciudad' "$json_file")

# Leer array de skills
mapfile -t skills < <(jq -r '.skills[]' "$json_file")

# --- Mostrar resultados ---
echo -e "\n${verde}=== Datos del JSON ===${reset}"
echo "Nombre: $nombre"
echo "Edad: $edad"
echo "Ciudad: $ciudad"
echo "Skills:"
for skill in "${skills[@]}"; do
    echo "- $skill"
done

echo -e "\n${verde}Lectura completada${reset}"

#### **Dar permisos y ejecutar:**

bash

chmod +x superScript.sh
./superScript.sh

---

### **5. POKÉDEX CON JQ**

#### **Crear archivo Pokédex:**

bash

nano pokedex.json

#### **Contenido de pokedex.json:**

json

{
  "pokemons": [
    {"numero": 1, "nombre": "Bulbasaur", "tipo": "Planta/Veneno", "nivel": 5},
    {"numero": 4, "nombre": "Charmander", "tipo": "Fuego", "nivel": 5},
    {"numero": 7, "nombre": "Squirtle", "tipo": "Agua", "nivel": 5},
    {"numero": 25, "nombre": "Pikachu", "tipo": "Eléctrico", "nivel": 8}
  ]
}

#### **Script pokedex.sh:**

bash

#!/usr/bin/env bash
#
# Script: pokedex.sh
# Descripción: Buscar Pokémon usando jq
#

# Colores
verde="\e[32m"
rojo="\e[31m"
amarillo="\e[33m"
reset="\e[0m"

# --- Comprobar jq ---
echo -e "${amarillo}Comprobando jq...${reset}"

if ! command -v jq &> /dev/null; then
    echo -e "${rojo}jq no está instalado${reset}"
    if grep -qi "ubuntu" /etc/os-release || grep -qi "raspbian" /etc/os-release; then
        echo "Instalando jq..."
        sudo apt update && sudo apt install -y jq
    else
        echo "Instala jq manualmente"
        exit 1
    fi
else
    echo -e "${verde}jq instalado${reset}"
fi

# --- Verificar archivo Pokédex ---
json_file="pokedex.json"
if [ ! -f "$json_file" ]; then
    echo -e "${rojo}No se encuentra $json_file${reset}"
    exit 1
fi

# --- Buscar Pokémon ---
read -p "Introduce nombre o número del Pokémon: " pokemon_input

if [ -z "$pokemon_input" ]; then
    echo -e "${rojo}El campo no puede estar vacío${reset}"
    exit 1
fi

# --- Buscar coincidencias ---
result=$(jq -r --arg input "$pokemon_input" '
.pokemons[] | 
select(.numero == ($input | tonumber?) or .nombre == $input) |
"Número: \(.numero)\nNombre: \(.nombre)\nTipo: \(.tipo)\nNivel: \(.nivel)"
' "$json_file")

# --- Mostrar resultados ---
if [ -z "$result" ]; then
    echo -e "${rojo}No se encontró: $pokemon_input${reset}"
else
    echo -e "\n${verde}=== Pokémon Encontrado ===${reset}"
    echo -e "$result"
fi

#### **Ejecutar Pokédex:**

bash

chmod +x pokedex.sh
./pokedex.sh

**Ejemplo de uso:**

text

Introduce nombre o número del Pokémon: 25

=== Pokémon Encontrado ===
Número: 25
Nombre: Pikachu
Tipo: Eléctrico
Nivel: 8

---

### **6. RESUMEN DE COMANDOS JQ ÚTILES**

|Comando|Descripción|Ejemplo|
|---|---|---|
|`jq . archivo.json`|Formatear JSON|`jq . datos.json`|
|`jq -r '.campo'`|Raw output (sin comillas)|`jq -r '.nombre'`|
|`jq '.array[]'`|Iterar array|`jq '.skills[]'`|
|`jq '.objeto.campo'`|Campo anidado|`jq '.direccion.ciudad'`|
|`jq 'select()'`|Filtrar datos|`jq 'select(.edad > 25)'`|
|`jq 'map()'`|Transformar array|`jq 'map(.nombre)'`|
