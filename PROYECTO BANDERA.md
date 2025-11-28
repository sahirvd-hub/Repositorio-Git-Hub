--------------------------------------------------------------------------

**1 Actualizar todos los repositorios con el siguiente comando:**
sudo apt update && sudo apt upgrade -y
-------------------------------------------------------------------------------------------------


**2 Ahora instalaremos GITK con el siguiente comando:**
sudo apt install gitk 
-------------------------------------------------------------------------------------------------


**3 Luego instalaremos apache con el siguiente comando:**
sudo apt install apache2
-------------------------------------------------------------------------------------------------


**4 Para verificar que tenemos correctamente instalado apache y funcionado realizaremos el siguiente comando:**
sudo systemctl status apache2
-------------------------------------------------------------------------------------------------


**5 Ahora pasaremos a movermos a la carpeta /var/www/html para dirigirnos a esa carpeta realizaremos el siguiente comando:**
cd /var/www/html 
-------------------------------------------------------------------------------------------------


**6 Modificaremos los permisos del directorio con el siguiente comando:** 
sudo chown -R tu_usuario:tu_usuario /var/www/html
-------------------------------------------------------------------------------------------------
**EJEMPLO:**
sudo chown -R operadorssh1:operadorssh1 /var/www/html
-------------------------------------------------------------------------------------------------


**7 Inicializaremos el repositorio para eso aplicaremos el siguiente comando:**
git init
-------------------------------------------------------------------------------------------------


**8 Crearemos un nano con el siguiente comando:**
sudo nano index.html 
-------------------------------------------------------------------------------------------------
**Pegaremos este contenido** y guardamos

<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Bandera</title>  
    <style>  
        body {  
            margin: 0;  
            padding: 0;  
        }  
        .franja {  
            height: 100px;  
            width: 100%;  
        }  
    </style>  
</head>  
<body>  
    <div class="franja" style="background-color: red;"></div>  
    <div class="franja" style="background-color: yellow;"></div>  
    <div class="franja" style="background-color: red;"></div>  
</body>  
</html>
--------------------------------------------------------------------------

**9 Este paso es importante porque subiremos el contenido en git hub para eso realizaremos el siguiente comando:
git config --global user.email "tu_email"
------------------------------------------------------------------------------------------------
git config --global user.name "tu_usuario"
------------------------------------------------------------------------------------------------


**10 Ahora realizaremos un commit:****
**Añadiremos los cambios al área de preparación con el comando:**
git add index.html
-------------------------------------------------------------------------------------------------
**Luego realizar un commit con el siguiente comando:**
git commit -m "Añade la bandera inicial (España)"
-------------------------------------------------------------------------------------------------


### **Parte 2: Creación de banderas y commits**

**1 Cambiaremos los colores para representar la bandera de otro país:**

**Modificaremos el archivo index.html. Por ejemplo, para la bandera de Italia (verde, blanca y roja)**
**Guarda los cambios y haz un commit:**
git add index.html 
-------------------------------------------------------------------------------------------------
**Realiza un commit:**
git commit -m "Cambia los colores para la bandera de Italia"
-------------------------------------------------------------------------------------------------
**También podremos hacerlo todo esto en un mismo comando solo tendremos que agregar (&&)**
git add index.html && git commit -m "Cambia los colores para la bandera de Italia"
-------------------------------------------------------------------------------------------------


**2 Realizaremos lo mismo pero con otra bandera como por ejemplo Francia**

**Modificaremos el archivo index.html. Para la bandera de Francia (azul, blanca y roja)**
**Luego guardamos los cambios y haz un commit:**
git add index.html 
-------------------------------------------------------------------------------------------------
**Realiza un commit:**
git commit -m "Cambia los colores para la bandera de Francia"
-------------------------------------------------------------------------------------------------


**3 Realizaremos lo mismo pero con otra bandera como por ejemplo Alemania**

**Modificaremos el archivo index.html. Para la bandera de Francia (negro, roja y amarillo)**
**Luego guardamos los cambios y haz un commit:**
git add index.html 
-------------------------------------------------------------------------------------------------
**Realiza un commit:**
git commit -m "Cambia los colores para la bandera de Alemania"
-------------------------------------------------------------------------------------------------


### **Parte 3: Uso de ramas**

**1 Crearemos una rama para la bandera de Italia:**
git checkout -b italia
-------------------------------------------------------------------------------------------------
**sudo nano index.html**
git add index.html
-------------------------------------------------------------------------------------------------
git commit -m "Crea la bandera de Italia en la rama italia"
-------------------------------------------------------------------------------------------------


**2 Crearemos una rama para la bandera de Francia:**
git checkout -b francia
-------------------------------------------------------------------------------------------------
**sudo nano index.html**
git add index.html
-------------------------------------------------------------------------------------------------
git commit -m "Crea la bandera de Francia en la rama francia"
-------------------------------------------------------------------------------------------------


**3 Crearemos una rama para la bandera de Alemania:**
git checkout -b alemania
-------------------------------------------------------------------------------------------------
**sudo nano index.html**
git add index.html
-------------------------------------------------------------------------------------------------
git commit -m "Crea la bandera de Alemania en la rama alemania"
-------------------------------------------------------------------------------------------------


### **Parte 4: Visualización con gitk**

**1 Por ultimo abriremos gitk para visualizar tu repositorio es decir que deberías estar en /var/www/html para luego ejecutar el siguiente comando: 

git --all
-------------------------------------------------------------------------------------------------
**Ahi observaras el historial de commits y las ramas que hemos creado.**
