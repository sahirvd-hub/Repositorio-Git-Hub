#  Mi Proyecto de Contenedores - Sistema de Gestión

##  Información Personal del Proyecto

- **Nombre:** Sahir VD
    
- **Usuario GitHub:** sahirvd-hub
    
- **Email:** sahirvd04@gmail.com
    
- **Especialidad:** Desarrollo Backend y DevOps
    
- **Proyecto:** Sistema de Gestión de Inventario
    

---

## Reto 1: Configuración Inicial del Contenedor

### Paso 1: Descargar imagen de Ubuntu

bash

docker pull ubuntu:22.04

### Paso 2: Ejecutar contenedor interactivo

bash

docker run -it --name sahir-contenedor ubuntu:22.04

### Paso 3: Instalar dependencias dentro del contenedor

bash

apt update
apt install python3 python3-pip -y
apt install python3-requests -y
apt install mysql-client python3-mysql.connector -y

### Paso 4: Crear imagen personalizada

bash

docker commit sahir-contenedor sahir-ubuntu-python:1.0

---

##  Reto 2: Configuración de Volúmenes

### Estructura de directorios:

text

sahir-proyecto-docker/
├── datos_app/
├── dockerfiles/
│   └── Dockerfile
├── scripts/
└── database/

### Montaje de volumen:

bash

docker run -d --name app-inventario \
  -v $(pwd)/datos_app:/app/datos \
  sahir-ubuntu-python:1.0 tail -f /dev/null

### Prueba del volumen:

bash

docker exec -it app-inventario bash
echo "Proyecto de Sahir VD - sahirvd04@gmail.com" > /app/datos/info_proyecto.txt
cat /app/datos/info_proyecto.txt

---

##  Reto 3: Configuración de Git

### Configurar usuario global:

bash

git config --global user.name "sahirvd-hub"
git config --global user.email "sahirvd04@gmail.com"

### Inicializar repositorio:

bash

git init
git branch -M main
git remote add origin https://github.com/sahirvd-hub/sistema-inventario.git

---

##  Reto 4: Base de Datos MySQL

### Crear contenedor MySQL:

bash

docker run -d \
  --name mysql-inventario \
  -e MYSQL_ROOT_PASSWORD=SahirPassword2024! \
  -e MYSQL_DATABASE=inventario_db \
  -p 3306:3306 \
  mysql:8.0

### Conectar a MySQL:

bash

docker exec -it mysql-inventario mysql -u root -p

### Crear tabla de productos:

sql

USE inventario_db;

CREATE TABLE productos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    categoria VARCHAR(50),
    cantidad INT DEFAULT 0,
    precio DECimal(10,2),
    proveedor VARCHAR(80),
    fecha_actualizacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

### Insertar datos de ejemplo:

sql

INSERT INTO productos (nombre, categoria, cantidad, precio, proveedor) VALUES
('Laptop Dell XPS', 'Tecnología', 15, 1299.99, 'Dell Inc.'),
('Mouse Inalámbrico', 'Accesorios', 50, 29.99, 'Logitech'),
('Teclado Mecánico', 'Accesorios', 25, 89.99, 'Razer'),
('Monitor 24"', 'Tecnología', 30, 199.99, 'Samsung'),
('Tablet Android', 'Tecnología', 20, 299.99, 'Samsung');

---

##  Reto 5: Script Python para MySQL

### Crear script `gestion_inventario.py`:

python

import mysql.connector

def conectar_bd():
    return mysql.connector.connect(
        host='localhost',
        user='root',
        password='SahirPassword2024!',
        database='inventario_db'
    )

def mostrar_inventario():
    conexion = conectar_bd()
    cursor = conexion.cursor()
    cursor.execute("SELECT * FROM productos")
    
    productos = cursor.fetchall()
    
    print("\n=== SISTEMA DE GESTIÓN DE INVENTARIO ===")
    print("Desarrollado por: Sahir VD")
    print("GitHub: sahirvd-hub")
    print("=" * 60)
    print("ID | NOMBRE           | CATEGORÍA   | CANT | PRECIO   | PROVEEDOR")
    print("-" * 60)
    
    for producto in productos:
        print(f"{producto[0]:2} | {producto[1]:15} | {producto[2]:11} | {producto[3]:4} | ${producto[4]:7.2f} | {producto[5]}")
    
    cursor.close()
    conexion.close()

if __name__ == "__main__":
    mostrar_inventario()

---

##  Reto 6: Entorno Virtual y Dependencias

### Crear entorno virtual:

bash

python3 -m venv venv
source venv/bin/activate

### Instalar dependencias:

bash

pip install mysql-connector-python
pip install tabulate

### Archivo `requirements.txt`:

text

mysql-connector-python==8.0.33
tabulate==0.9.0

---

##  Reto 7: Mejorar Presentación de Datos

### Script mejorado `inventario_app.py`:

python

from tabulate import tabulate
import mysql.connector

def obtener_datos_inventario():
    conexion = mysql.connector.connect(
        host='localhost',
        user='root',
        password='SahirPassword2024!',
        database='inventario_db'
    )
    
    cursor = conexion.cursor()
    cursor.execute("SELECT id, nombre, categoria, cantidad, precio, proveedor FROM productos")
    datos = cursor.fetchall()
    
    cursor.close()
    conexion.close()
    return datos

def calcular_estadisticas(productos):
    total_productos = len(productos)
    valor_total = sum([p[3] * p[4] for p in productos])  # cantidad * precio
    productos_bajos = len([p for p in productos if p[3] < 10])
    
    return total_productos, valor_total, productos_bajos

def main():
    print(" SISTEMA DE GESTIÓN DE INVENTARIO")
    print("   Desarrollado por: Sahir VD")
    print("   Contacto: sahirvd04@gmail.com\n")
    
    productos = obtener_datos_inventario()
    
    headers = ["ID", "Producto", "Categoría", "Cantidad", "Precio", "Proveedor"]
    tabla = []
    
    for producto in productos:
        precio_formateado = f"${producto[4]:,.2f}"
        tabla.append([producto[0], producto[1], producto[2], producto[3], precio_formateado, producto[5]])
    
    print(tabulate(tabla, headers=headers, tablefmt="grid"))
    
    # Estadísticas
    total_productos, valor_total, productos_bajos = calcular_estadisticas(productos)
    
    print(f"\n ESTADÍSTICAS DEL INVENTARIO:")
    print(f"   • Total de productos: {total_productos}")
    print(f"   • Valor total del inventario: ${valor_total:,.2f}")
    print(f"   • Productos con stock bajo (<10): {productos_bajos}")

if __name__ == "__main__":
    main()

---

##  Reto 8: Integración con MongoDB

### Crear contenedor MongoDB:

bash

docker run -d \
  --name mongo-inventario \
  -p 27017:27017 \
  -v $(pwd)/mongo_data:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=sahir_admin \
  -e MONGO_INITDB_ROOT_PASSWORD=MongoSahir2024! \
  mongo:6.0

### Script para MongoDB `mongo_inventario.py`:

python

from pymongo import MongoClient
from tabulate import tabulate

def conectar_mongo():
    client = MongoClient('mongodb://sahir_admin:MongoSahir2024!@localhost:27017/')
    return client.inventario_db

def main():
    print(" MONGODB - SISTEMA DE INVENTARIO")
    print("   Usuario: sahirvd-hub")
    
    db = conectar_mongo()
    
    # Datos de ejemplo para MongoDB
    productos_mongo = [
        {
            "codigo": "MBP-001",
            "nombre": "MacBook Pro",
            "categoria": "Laptops",
            "stock": 8,
            "precio": 1999.99,
            "caracteristicas": ["16GB RAM", "512GB SSD", "Touch Bar"]
        },
        {
            "codigo": "IPH-001",
            "nombre": "iPhone 15",
            "categoria": "Smartphones", 
            "stock": 25,
            "precio": 999.99,
            "caracteristicas": ["128GB", "5G", "Face ID"]
        }
    ]
    
    # Insertar datos
    db.productos.delete_many({})  # Limpiar primero
    resultado = db.productos.insert_many(productos_mongo)
    
    print(f" Insertados {len(resultado.inserted_ids)} productos en MongoDB")
    
    # Mostrar datos
    datos = list(db.productos.find({}, {'_id': 0, 'caracteristicas': 0}))
    print("\n PRODUCTOS EN MONGODB:")
    print(tabulate(datos, headers="keys", tablefmt="grid"))

if __name__ == "__main__":
    main()

---

##  Reto 9: Docker Hub

### Login en Docker Hub:

bash

docker login

### Etiquetar y subir imagen:

bash

docker tag sahir-ubuntu-python:1.0 sahirvd-hub/ubuntu-python-inventario:1.0
docker push sahirvd-hub/ubuntu-python-inventario:1.0

### Para usar en otra máquina:

bash

docker pull sahirvd-hub/ubuntu-python-inventario:1.0

---

##  Estructura Final del Proyecto

text

sahir-proyecto-docker/
├── datos_app/
│   └── info_proyecto.txt
├── scripts/
│   ├── gestion_inventario.py
│   ├── inventario_app.py
│   └── mongo_inventario.py
├── database/
│   └── config.json
├── dockerfiles/
│   └── Dockerfile
├── mongo_data/
├── venv/
├── requirements.txt
└── README.md

---

##  Comandos de Despliegue Rápido

### Script de instalación completo:

bash

#!/bin/bash
echo " Iniciando despliegue del sistema de inventario de Sahir VD..."

# Descargar imágenes
docker pull ubuntu:22.04
docker pull mysql:8.0
docker pull mongo:6.0

# Crear contenedores
docker run -d --name mysql-inventario \
  -e MYSQL_ROOT_PASSWORD=SahirPassword2024! \
  -e MYSQL_DATABASE=inventario_db \
  -p 3306:3306 mysql:8.0

docker run -d --name mongo-inventario \
  -e MONGO_INITDB_ROOT_USERNAME=sahir_admin \
  -e MONGO_INITDB_ROOT_PASSWORD=MongoSahir2024! \
  -p 27017:27017 mongo:6.0

echo " Sistema desplegado correctamente por Sahir VD"
echo " MySQL disponible en puerto 3306"
echo " MongoDB disponible en puerto 27017"
echo " Contacto: sahirvd04@gmail.com"