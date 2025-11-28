

 README – Subir un proyecto a GitHub correctamente

Este documento explica el proceso ordenado y correcto para:

1. Crearemos un repositorio local  
2. Añadiremos archivos  
3. Haremos commits  
4. Configuraremos el repositorio remoto (GitHub)  
5. Subiremos el proyecto a GitHub con `git push`

---

##  1. Inicializar el repositorio

```bash
git init
```

Esto crea la carpeta `.git` y convierte el directorio en un repositorio.

---

##  2. Comprobar el estado inicial

```bash
git status
```

Aquí verás los archivos sin seguimiento.

---

##  3. Añadir los archivos al área de preparación (staging)

```bash
git add .
```

Esto añade todos los archivos que aparecen como “sin seguimiento”.

---

##  4. Hacer el primer commit

```bash
git commit -m "Primer commit: añade estructura inicial"
```

Es el commit raíz del proyecto.

---

##  5. Cambiar el nombre de la rama por defecto a `main`

```bash
git branch -M main
```

GitHub usa `main` como rama principal.

---

##  6. Crear el repositorio en GitHub

En GitHub → **New Repository**  
*IMPORTANTE*: Crearlo **vacío**, sin README, sin .gitignore, sin licencia.

---

##  7. Añadir el repositorio remoto

Copiar la URL del repositorio recién creado.

### Si usas HTTPS:
```bash
git remote add origin https://github.com/usuario/nombre-del-repo.git
```

### Si usas SSH:
```bash
git remote add origin git@github.com:usuario/nombre-del-repo.git
```

Verifica que esté bien agregado:

```bash
git remote -v
```

---

##  8. Subir el contenido a GitHub

```bash
git push -u origin main
```

El parámetro `-u` hace que `origin/main` quede configurado como el remoto por defecto.

---

##  El flujo correcto completo

```bash
git init
git status
git add .
git commit -m "Primer commit"
git branch -M main
git remote add origin https://github.com/usuario/repositorio.git
git push -u origin main
