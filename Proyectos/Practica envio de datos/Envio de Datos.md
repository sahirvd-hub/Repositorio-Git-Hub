## Paso 1: Instalación del Servidor Web y Base de Datos

### 1.1 Actualizar el sistema

bash

sudo apt update && sudo apt upgrade -y

### 1.2 Instalar Apache, PHP y MySQL

bash

sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql -y

### 1.3 Instalar extensiones PHP adicionales

bash

sudo apt install php-cli php-json php-curl php-mbstring -y

### 1.4 Verificar que los servicios estén activos

bash

sudo systemctl status apache2
sudo systemctl status mysql

---

##  Paso 2: Configuración de la Base de Datos

### 2.1 Acceder a MySQL

bash

sudo mysql -u root -p

_Si es la primera vez, puede que no pida contraseña_

### 2.2 Crear la base de datos y tabla

sql

CREATE DATABASE sistema_gestion;
USE sistema_gestion;

CREATE TABLE registros (
    id INT NOT NULL AUTO_INCREMENT,
    nombre_completo VARCHAR(80) NOT NULL,
    localidad VARCHAR(50) NOT NULL,
    edad INT(3) NOT NULL,
    correo VARCHAR(150) NOT NULL UNIQUE,
    fecha_ingreso DATE NOT NULL,
    estado ENUM('activo','pendiente') NOT NULL,
    PRIMARY KEY (id)
);

### 2.3 Crear usuario para la aplicación

sql

CREATE USER 'app_user'@'localhost' IDENTIFIED BY 'password_seguro';
GRANT ALL PRIVILEGES ON sistema_gestion.* TO 'app_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;

---

##  Paso 3: Estructura de Directorios

### 3.1 Crear directorio principal

bash

sudo mkdir /var/www/sistema_gestion
cd /var/www/sistema_gestion

### 3.2 Crear subdirectorios necesarios

bash

sudo mkdir archivos documentos backups
sudo chown -R www-data:www-data /var/www/sistema_gestion
sudo chmod -R 755 /var/www/sistema_gestion

---

##  Paso 4: Configuración de la Aplicación Web

### 4.1 Crear archivo de configuración de base de datos

bash

sudo nano /var/www/sistema_gestion/config.php

**Contenido de config.php:**

php

<?php
$servername = "localhost";
$username = "app_user";
$password = "password_seguro";
$dbname = "sistema_gestion";

try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch(PDOException $e) {
    die("Error de conexión: " . $e->getMessage());
}
?>

### 4.2 Crear el archivo principal index.php

bash

sudo nano /var/www/sistema_gestion/index.php

**Contenido básico de index.php:**

php

<?php
session_start();
require_once 'config.php';
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Sistema de Gestión</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .menu { background: #f0f0f0; padding: 10px; margin-bottom: 20px; }
        .menu a { margin-right: 15px; text-decoration: none; color: #333; }
        .form-group { margin-bottom: 10px; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
    </style>
</head>
<body>
    <div class="menu">
        <a href="index.php">Inicio</a>
        <a href="registros.php">Registros</a>
        <a href="archivos.php">Archivos</a>
        <a href="panel.php">Panel</a>
    </div>
    <h1>Sistema de Gestión de Registros</h1>
    <!-- El contenido específico irá aquí -->
</body>
</html>

---

##  Paso 5: Desarrollo de Funcionalidades

### 5.1 Crear módulo de registros (registros.php)

php

<?php
require_once 'config.php';

// Procesar formulario
if ($_POST) {
    $stmt = $conn->prepare("INSERT INTO registros (nombre_completo, localidad, edad, correo, fecha_ingreso, estado) VALUES (?, ?, ?, ?, ?, ?)");
    $stmt->execute([
        $_POST['nombre_completo'],
        $_POST['localidad'],
        $_POST['edad'],
        $_POST['correo'],
        $_POST['fecha_ingreso'],
        $_POST['estado']
    ]);
}

// Mostrar formulario y listado
?>
<!-- Formulario de registro -->
<form method="POST">
    <input type="text" name="nombre_completo" placeholder="Nombre completo" required>
    <input type="text" name="localidad" placeholder="Localidad" required>
    <input type="number" name="edad" placeholder="Edad" min="18" max="99" required>
    <input type="email" name="correo" placeholder="Correo electrónico" required>
    <input type="date" name="fecha_ingreso" required>
    <select name="estado" required>
        <option value="activo">Activo</option>
        <option value="pendiente">Pendiente</option>
    </select>
    <button type="submit">Registrar</button>
</form>

<!-- Listado de registros -->
<?php
$stmt = $conn->prepare("SELECT * FROM registros");
$stmt->execute();
$registros = $stmt->fetchAll();
?>

<table>
    <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Localidad</th>
        <th>Edad</th>
        <th>Correo</th>
        <th>Estado</th>
    </tr>
    <?php foreach ($registros as $registro): ?>
    <tr>
        <td><?= $registro['id'] ?></td>
        <td><?= $registro['nombre_completo'] ?></td>
        <td><?= $registro['localidad'] ?></td>
        <td><?= $registro['edad'] ?></td>
        <td><?= $registro['correo'] ?></td>
        <td><?= $registro['estado'] ?></td>
    </tr>
    <?php endforeach; ?>
</table>

### 5.2 Crear módulo de archivos (archivos.php)

php

<?php
if ($_POST && isset($_POST['contenido'])) {
    $nombre_archivo = "documento_" . time() . ".txt";
    $ruta = "documentos/" . $nombre_archivo;
    file_put_contents($ruta, $_POST['contenido']);
    echo "<p>Archivo creado: $nombre_archivo</p>";
}
?>
<form method="POST">
    <textarea name="contenido" placeholder="Contenido del archivo..." rows="5" cols="50"></textarea><br>
    <button type="submit">Crear Archivo</button>
</form>

### 5.3 Crear panel de control (panel.php)

php

<?php
session_start();

if (!isset($_SESSION['tareas'])) {
    $_SESSION['tareas'] = [];
}

if ($_POST) {
    if (isset($_POST['nueva_tarea']) && strlen($_POST['nueva_tarea']) <= 25) {
        array_unshift($_SESSION['tareas'], $_POST['nueva_tarea']);
    } elseif (isset($_POST['completar'])) {
        array_shift($_SESSION['tareas']);
    } elseif (isset($_POST['limpiar'])) {
        $_SESSION['tareas'] = [];
    }
}
?>

<form method="POST">
    <input type="text" name="nueva_tarea" placeholder="Nueva tarea (max 25 chars)" maxlength="25">
    <button type="submit">Añadir</button>
    <button type="submit" name="completar">Completar</button>
    <button type="submit" name="limpiar">Limpiar Todo</button>
</form>

<h3>Tareas Pendientes:</h3>
<ul>
<?php foreach ($_SESSION['tareas'] as $index => $tarea): ?>
    <li><?= ($index + 1) . ". " . $tarea ?></li>
<?php endforeach; ?>
</ul>

---

##  Paso 6: Configuración de Apache

### 6.1 Crear virtual host

bash

sudo nano /etc/apache2/sites-available/sistema_gestion.conf

**Contenido:**

apache

<VirtualHost *:80>
    ServerName localhost
    DocumentRoot /var/www/sistema_gestion
    
    <Directory /var/www/sistema_gestion>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/sistema_error.log
    CustomLog ${APACHE_LOG_DIR}/sistema_access.log combined
</VirtualHost>

### 6.2 Habilitar el sitio

bash

sudo a2ensite sistema_gestion.conf
sudo a2dissite 000-default.conf
sudo systemctl reload apache2

---

## Paso 7: Configuración de Seguridad

### 7.1 Configurar firewall

bash

sudo ufw allow 80/tcp
sudo ufw allow ssh
sudo ufw enable

### 7.2 Asegurar MySQL

bash

sudo mysql_secure_installation

### 7.3 Configurar permisos seguros

bash

sudo chown -R www-data:www-data /var/www/sistema_gestion
sudo find /var/www/sistema_gestion -type f -exec chmod 644 {} \;
sudo find /var/www/sistema_gestion -type d -exec chmod 755 {} \;

---

##  Paso 8: Verificación Final

### 8.1 Probar la aplicación

bash

# Verificar que Apache esté escuchando
sudo netstat -tulpn | grep :80

# Probar PHP
php -r "echo 'PHP funciona correctamente';"

### 8.2 Acceder a la aplicación

Abre tu navegador y visita:

text

http://tu_ip_servidor

---

##  Solución de Problemas Comunes

### Error de conexión a base de datos:

- Verificar credenciales en config.php
    
- Confirmar que MySQL esté ejecutándose
    
- Comprobar permisos del usuario de la base de datos
    

### Error de permisos de archivos:

bash

sudo chown -R www-data:www-data /var/www/sistema_gestion

### Error de sintaxis PHP:

bash

php -l /var/www/sistema_gestion/index.php

---

##  Estructura Final del Proyecto

text

/var/www/sistema_gestion/
├── config.php
├── index.php
├── registros.php
├── archivos.php
├── panel.php
├── documentos/
├── archivos/
└── backups/

---

**🎉 ¡Instalación Completada!** El sistema está listo para usar con todas las funcionalidades implementadas.