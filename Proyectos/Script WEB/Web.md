##  Script de Backup (`backup_web.sh`)

bash

#!/bin/bash
# Script de backup web para operadorssh1
# Backup de BD + Archivos web

# Configuración básica
FECHA=$(date +"%Y-%m-%d")
BACKUP_DIR="/home/operadorssh1/web_backup"
DB_NAME="basedatos_clase"  # Cambiar por el nombre de tu BD real
WEB_DIR="/var/www/html"

# Backup de base de datos
echo "Iniciando backup de base de datos..."
mysqldump -u root $DB_NAME > $BACKUP_DIR/database/backup_$FECHA.sql

if [ $? -eq 0 ]; then
    echo " Base de datos respaldada"
else
    echo " Error en backup de BD"
    exit 1
fi

# Backup de archivos web
echo " Iniciando backup de archivos web..."
tar -czf $BACKUP_DIR/web_files/web_backup_$FECHA.tar.gz $WEB_DIR

if [ $? -eq 0 ]; then
    echo " Archivos web respaldados"
else
    echo " Error en backup web"
    exit 1
fi

# Limpiar backups antiguos (más de 7 días)
find $BACKUP_DIR -name "*.sql" -mtime +7 -delete
find $BACKUP_DIR -name "*.tar.gz" -mtime +7 -delete

echo "Backup completado: $FECHA"

---

##  Configuración Crontab

### Editar crontab:

bash

crontab -e

### Agregar esta línea:

bash

# Backup diario a las 00:00 (medianoche)
0 0 * * * /home/operadorssh1/web_backup/backup_web.sh >> /home/operadorssh1/web_backup/logs/backup.log 2>&1

---

## 🔧 Instalación Paso a Paso

### 1. Crear estructura de directorios:

bash

mkdir -p /home/operadorssh1/web_backup/{database,web_files,logs}
cd /home/operadorssh1/web_backup

### 2. Crear el script:

bash

nano backup_web.sh

_Pegar el contenido del script y guardar_

### 3. Dar permisos de ejecución:

bash

chmod +x backup_web.sh

### 4. Configurar crontab:

bash

crontab -e

_Agregar la línea del cron y guardar_

### 5. Probar manualmente:

bash

./backup_web.sh

---

##  Verificación

### Verificar backups creados:

bash

ls -la /home/operadorssh1/web_backup/database/
ls -la /home/operadorssh1/web_backup/web_files/

### Verificar logs:

bash

tail -f /home/operadorssh1/web_backup/logs/backup.log

### Verificar configuración crontab:

bash

crontab -l

---

## Estructura Final

text

/home/operadorssh1/web_backup/
├── backup_web.sh
├── database/
│   └── backup_2024-01-15.sql
├── web_files/
│   └── web_backup_2024-01-15.tar.gz
└── logs/
    └── backup.log

---

##  Personalización Requerida

**Antes de usar, modificar en el script:**

- `DB_NAME="basedatos_clase"` → Reemplazar con el nombre real de tu base de datos
    

**Si MySQL requiere contraseña:**

bash

# Modificar la línea del mysqldump por:
mysqldump -u root -pTuPassword $DB_NAME > $BACKUP_DIR/database/backup_$FECHA.sql