# Bitácora — Entrega 2 (Desarrollo inicial)

Sistema Web de Votaciones — Festival de la Leyenda Vallenata 2027.
Registro de cada paso: fecha, responsable, qué se hizo, comandos, problemas y capturas.

---

## Paso 1 — Instalación del software

- **Fecha de inicio:** 2026-10-16
- **Responsable:** Fernando Coronell Molina (en su computador, Windows 11 Pro)

### Estado inicial del equipo (diagnóstico)

Comando ejecutado en PowerShell para saber qué había instalado:

```powershell
foreach ($c in 'git','py','python','mysql','code') { Get-Command $c -ErrorAction SilentlyContinue }
```

| Herramienta | Estado inicial |
|---|---|
| Git | No instalado |
| Python (`py`) | No instalado (`python` solo era el alias de la Microsoft Store, no un Python real) |
| MySQL Server / Workbench | No instalado (sin servicio MySQL) |
| Visual Studio Code | Ya instalado |

### 1.1 Git for Windows

- Instalador oficial: https://git-scm.com/downloads/win (ejecutado por Fernando).
- Opciones elegidas: editor por defecto Visual Studio Code; PATH "Git from the command line and also from 3rd-party software".
- Verificación: `git --version` → `git version 2.56.0.windows.2` (ruta `C:\Program Files\Git\cmd\git.exe`).
- Configuración global:
  ```powershell
  git config --global user.name "fcoronell"
  git config --global init.defaultBranch main
  git config --global user.email "<correo personal de Fernando>"
  ```
- Decisión: se usa el correo personal (no el institucional), porque sigue activo después de terminar la carrera. La cuenta de GitHub se creará con ese mismo correo (paso 1.4).
- Captura: `01-git-version.png`.

### 1.2 Python 3.13

- Instalador oficial: https://www.python.org/downloads/windows/ — "Windows installer (64-bit)", ejecutado por Fernando.
- Opciones elegidas: "Add python.exe to PATH" y "Use admin privileges when installing py.exe" → "Install Now"; "Disable path length limit" si el instalador lo ofrece.
- Verificación:
  ```powershell
  py --version          # Python 3.13.16
  python --version      # Python 3.13.16
  py -0p                # -V:3.13 * ...\Python\Python313\python.exe
  python -m pip --version   # pip 26.2.1 (python 3.13)
  ```
- El alias de la Microsoft Store sigue existiendo, pero el Python real aparece primero en el PATH, así que no hace falta desactivarlo.
- Captura: `02-python-version.png`.

### 1.3 MySQL Community Server 8.4 LTS y MySQL Workbench

- Servidor: https://dev.mysql.com/downloads/mysql/ → 8.4.x LTS, "Windows (x86, 64-bit), MSI Installer" (ejecutado por Fernando).
- MySQL Configurator: *Development Computer*, puerto 3306, autenticación recomendada, servicio de Windows con inicio automático, sin bases de ejemplo. La contraseña de `root` la definió Fernando y **no se documenta**.
- Workbench: https://dev.mysql.com/downloads/workbench/ → MySQL Workbench 8.0 CE (la versión más reciente publicada; es compatible con el servidor 8.4).
- Conexión en Workbench: "Festival local (root)", host `127.0.0.1`, puerto `3306`, usuario `root`, sin esquema por defecto. *Test Connection* exitoso.
- Verificación desde PowerShell:
  ```powershell
  Get-Service -Name 'MySQL*'          # MySQL84  Running  Automatic
  Test-NetConnection 127.0.0.1 -Port 3306 -InformationLevel Quiet   # True
  & "C:\Program Files\MySQL\MySQL Server 8.4\bin\mysql.exe" --version  # Ver 8.4.11 for Win64
  ```
- Capturas: `03-workbench-test-conexion.png`, `04-workbench-conectado.png`.

### 1.4 Cuenta de GitHub

- Cuenta creada en https://github.com/signup con el mismo correo configurado en Git (así los commits quedan asociados al perfil).
- Usuario: **fcoronell** → https://github.com/fcoronell (plan Free).
- Captura: `06-github-perfil.png`.

### 1.5 Visual Studio Code (opcional)

- Ya estaba instalado antes de empezar.

### Resumen del Paso 1

| Herramienta | Versión verificada |
|---|---|
| Git for Windows | 2.56.0.windows.2 |
| Python | 3.13.16 (pip 26.2.1) |
| MySQL Community Server | 8.4.11 (servicio `MySQL84`, automático) |
| MySQL Workbench | 8.0 CE |
| GitHub | cuenta `fcoronell` |

- **Fecha de cierre:** 2026-10-17
- Se preparó `docs/GUIA-INSTALACION-EQUIPO.md` para que Sebastián, Javik y Dahiana instalen lo mismo en sus equipos.
- No hay commit en este paso porque el repositorio se crea en el Paso 2. Este archivo entra en el primer commit.

### Problemas encontrados y solución

- Recién instalado Git, la terminal ya abierta no reconocía `git`: Windows solo actualiza el PATH en las terminales nuevas. **Solución:** cerrar y abrir de nuevo la terminal.
- El instalador no guardó la rama inicial `main`. **Solución:** `git config --global init.defaultBranch main`.

### Capturas del paso

| Archivo | Contenido | Sección del PDF |
|---|---|---|
| `01-git-version.png` | `git --version` y `git config --global --list` | Desarrollo técnico (entorno) |
| `02-python-version.png` | `py --version`, `python --version`, `pip --version` | Desarrollo técnico (entorno) |
| `03-workbench-test-conexion.png` | "Successfully made the MySQL connection" | Desarrollo técnico (entorno) |
| `04-workbench-conectado.png` | Workbench conectado (Navigator/Schemas) | Desarrollo técnico (entorno) |
| `05-servicio-mysql.png` | `Get-Service MySQL*` → Running | Desarrollo técnico (entorno) |
| `06-github-perfil.png` | Perfil de GitHub `fcoronell` | Desarrollo técnico (entorno) |

---

## Paso 2 — Repositorio y proyecto Django

- **Fecha de inicio:** 2026-10-17
- **Responsable:** Fernando Coronell Molina

### 2.1 Repositorio local

```powershell
git init
git config --global core.autocrlf true   # saltos de línea consistentes en Windows
git add .
git commit -m "Inicio del proyecto: README, .gitignore, CLAUDE.md y bitácora del Paso 1"
```
- Se creó `.gitignore` (excluye `.venv/`, `.env`, `__pycache__/`, `*.pyc`, `db.sqlite3`, `staticfiles/`) y un `README.md` inicial.
- Primer commit: `6d52cce`. Captura: `07-primer-commit.png`.

### 2.2 Repositorio en GitHub

- Repositorio público: https://github.com/fcoronell/festival-vallenato-votaciones (creado sin README/.gitignore/licencia para no chocar con los locales).
```powershell
git remote add origin https://github.com/fcoronell/festival-vallenato-votaciones.git
git push -u origin main
```
- El primer push abrió el navegador (Git Credential Manager) para autorizar a Git en la cuenta de GitHub.

### 2.3 Colaboradores

- Pendiente: invitar a Sebastián, Javik y Dahiana cuando envíen sus usuarios de GitHub.

### 2.4 Entorno virtual y dependencias

```powershell
py -3.13 -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install "Django>=5.2,<5.3" mysqlclient python-dotenv
pip freeze > requirements.txt
```
- Instalado: Django 5.2.18, mysqlclient 2.3.0 (rueda precompilada para Windows), python-dotenv 1.2.4, más asgiref, sqlparse y tzdata.
- Captura: `08-venv-dependencias.png`.

### 2.5 Proyecto y aplicaciones Django

```powershell
django-admin startproject config .
python manage.py startapp votaciones
python manage.py startapp cuentas
```
- `config/`: configuración general del proyecto (`settings.py`, `urls.py`, `wsgi.py`, `asgi.py`).
- `votaciones/`: ediciones, categorías, votaciones y opciones.
- `cuentas/`: registro, inicio y cierre de sesión.
- Se crearon también `templates/` y `static/css`, `static/img` (con `.gitkeep` para que Git guarde las carpetas vacías).

### 2.6 Ajustes de `config/settings.py`

- `SECRET_KEY`, `DEBUG` y `ALLOWED_HOSTS` se leen del archivo `.env` con `python-dotenv` (`load_dotenv`). La clave que Django escribió dentro de `settings.py` se eliminó y nunca se subió a Git; se generó una nueva solo en `.env`.
- `.env.example` (sí se sube) lista las variables sin valores reales, incluidas las de MySQL para el Paso 3.
- `INSTALLED_APPS`: se agregaron `votaciones` y `cuentas`.
- `TEMPLATES['DIRS'] = [BASE_DIR / 'templates']`; `STATICFILES_DIRS = [BASE_DIR / 'static']`; `STATIC_ROOT = BASE_DIR / 'staticfiles'`.
- `LANGUAGE_CODE = 'es-co'`, `TIME_ZONE = 'America/Bogota'`, `USE_TZ = True`.
- Verificación: `python manage.py check` → "System check identified no issues"; `git check-ignore -v .env` confirma que `.env` está ignorado.

### 2.7 Servidor de desarrollo

```powershell
python manage.py runserver
```
- http://127.0.0.1:8000 muestra la página de bienvenida de Django en español ("¡La instalación funcionó con éxito!").
- El aviso "18 unapplied migrations" es normal: las migraciones se aplican en el Paso 3, ya conectados a MySQL.
- Capturas: `09-runserver-consola.png`, `10-django-bienvenida.png`.

### 2.8 Cierre del Paso 2

- **Fecha de cierre:** 2026-10-18
- Antes del commit se revisó con `git status` que no se incluyeran `.env`, `.venv/` ni `db.sqlite3`.
```powershell
git add .
git commit -m "Paso 2: proyecto Django (config, apps votaciones y cuentas), settings con .env y bitácora"
git push
```

### Problemas encontrados y solución (Paso 2)

- **El primer `git push` desde una terminal no interactiva falló** ("terminal prompts disabled"). **Solución:** ejecutarlo en una terminal normal, donde Git Credential Manager pudo abrir el navegador para iniciar sesión.
- **`.venv\Scripts\Activate.ps1` fue bloqueado**: "la ejecución de scripts está deshabilitada en este sistema" (PSSecurityException). Windows bloquea los scripts de PowerShell por defecto. **Solución:** `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`, ejecutado por Fernando (solo afecta a su usuario y permite scripts locales). Captura: `08a-error-executionpolicy.png`.
- **En la terminal de VS Code, `git` "no se reconoce como nombre de un cmdlet"** (CommandNotFoundException). VS Code estaba abierto desde antes de instalar Git, y Windows solo carga el PATH cuando un programa se abre. **Solución:** cerrar todas las ventanas de VS Code y volver a abrirlo; luego `git --version` responde normalmente. Captura: `10a-error-git-no-reconocido.png`.
