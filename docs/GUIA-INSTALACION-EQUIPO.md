# Guía de instalación para el equipo

Para: Sebastián, Javik y Dahiana.
Objetivo: tener en tu computador lo necesario para clonar el proyecto, ejecutarlo y hacer tus propios commits. Los commits de cada integrante en GitHub son la evidencia del trabajo en equipo, que vale 5 de 20 puntos.

Tiempo aproximado: 45 minutos. Las instrucciones son para Windows. Si usas Mac o Linux, avísale a Fernando.

---

## 1. Git for Windows

1. Descárgalo de https://git-scm.com/downloads/win e instálalo.
2. Opciones importantes del instalador:
   - **Default editor:** Visual Studio Code (si lo tienes) o Notepad.
   - **Initial branch name:** "Override" → `main`.
   - **PATH:** "Git from the command line and also from 3rd-party software".
3. Abre una terminal **nueva** (PowerShell) y configura tu nombre y correo. Usa el **mismo correo** con el que vas a crear o ya tienes tu cuenta de GitHub:
   ```powershell
   git --version
   git config --global user.name "Tu Nombre"
   git config --global user.email "tu-correo@ejemplo.com"
   git config --global init.defaultBranch main
   ```

## 2. Python 3.13

1. Descárgalo de https://www.python.org/downloads/windows/. Elige el último **3.13.x**, no el 3.14, con el "Windows installer (64-bit)".
2. En la primera pantalla marca **"Add python.exe to PATH"** y luego "Install Now".
3. Comprueba en una terminal nueva:
   ```powershell
   py --version
   python --version
   ```
   Las dos deben mostrar `Python 3.13.x`. Si `python` abre la Microsoft Store, ve a *Configuración → Aplicaciones → Configuración avanzada de aplicaciones → Alias de ejecución de aplicaciones* y desactiva los de `python.exe` y `python3.exe`.

## 3. MySQL Community Server 8.4 LTS y MySQL Workbench

1. Servidor: descárgalo de https://dev.mysql.com/downloads/mysql/ (versión **8.4.x LTS**, "Windows (x86, 64-bit), MSI Installer"). Si te pide iniciar sesión, elige "No thanks, just start my download".
2. Instalación **Typical**. En MySQL Configurator:
   - *Development Computer*, puerto **3306**.
   - Define una contraseña de `root` **solo tuya**. No la compartas por chat ni la subas al repositorio.
   - Déjalo como servicio de Windows con inicio automático.
3. Workbench: descárgalo de https://dev.mysql.com/downloads/workbench/ e instálalo. Si te pide Visual C++ Redistributable, instálalo primero.
4. En Workbench crea una conexión: host `127.0.0.1`, puerto `3306`, usuario `root` → *Test Connection*.
5. Comprueba en PowerShell:
   ```powershell
   Get-Service -Name 'MySQL*'
   ```
   Debe decir `Running`.

## 4. Cuenta de GitHub

1. Créala en https://github.com/signup con el **mismo correo** que pusiste en `git config`.
2. Activa la verificación en dos pasos (*Settings → Password and authentication*).
3. **Envíale a Fernando tu nombre de usuario de GitHub** (solo el usuario) para que te invite como colaborador del repositorio `festival-vallenato-votaciones`. Acepta la invitación que te llegará al correo.

## 5. Opcional: Visual Studio Code

https://code.visualstudio.com/. Te recomendamos la extensión "Python" de Microsoft.

---

## Recomendaciones

- Guarda los proyectos en una carpeta **fuera de OneDrive**, por ejemplo `C:\dev\`. OneDrive bloquea archivos de `.venv` y `.git` y causa errores.
- Si quieres dejar evidencia propia, toma capturas de `git --version`, `py --version` y Workbench conectado.
- Cuando el repositorio esté listo, el `README.md` explicará cómo clonarlo y ejecutarlo.
