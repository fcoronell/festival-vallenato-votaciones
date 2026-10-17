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
