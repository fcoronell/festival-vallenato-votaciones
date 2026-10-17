# Sistema Web de Votaciones — Festival de la Leyenda Vallenata 2027

Contexto para Claude Code. Léelo completo antes de cada sesión de trabajo en este proyecto.

## 1. Quiénes somos y qué estamos haciendo

- Curso: Desarrollo Web (grupo 61), Fundación Universitaria del Área Andina. Docente: Deivys Morales Uribe.
- Equipo (4 integrantes): Sebastian David Baracaldo Marín, Fernando Coronell Molina, Javik Trujillo Mateus, Dahiana Herrera Ramírez.
- Quien trabaja contigo en esta computadora es **Fernando** (Windows). Empieza desde cero: no tiene Python, MySQL, Git ni cuenta de GitHub instalados todavía.
- Entrega actual: **Entrega 2 — Desarrollo inicial**. Fecha máxima: **26 de octubre de 2026**.
- La Entrega 1 (análisis y diseño) ya se entregó; este documento resume lo que de ella necesitamos.

## 2. Cómo trabajar con Fernando (muy importante)

1. Habla siempre en **español**. Fernando está aprendiendo: explica *qué* vas a hacer y *por qué* antes de hacerlo, en lenguaje sencillo.
2. Avanza **un paso a la vez** (sección 7). Al terminar cada paso, resume lo hecho y espera su confirmación antes de seguir.
3. Él quiere **documentar todo desde la instalación**. Cada vez que algo deba quedar como evidencia para el PDF, escríbelo en una línea propia así: `[CAPTURA] <qué capturar y en qué sección del PDF va>`. Las capturas las toma él (Windows: `Win + Shift + S`) y las guarda en `docs/capturas/` con nombre numerado (`01-python-version.png`, `02-...`).
4. Mantén `docs/BITACORA.md` al día: por cada paso, fecha, qué se hizo, comandos ejecutados, problemas encontrados y cómo se resolvieron, y las capturas asociadas. Esta bitácora alimenta el PDF.
5. Instaladores gráficos (Git, Python, MySQL): **los ejecuta Fernando**; tú le das el enlace oficial, le dices qué opciones marcar y luego verificas desde la terminal.
6. Comandos en **PowerShell** de Windows (o Git Bash cuando Git esté instalado). Muestra cada comando antes de ejecutarlo.
7. Al final de cada paso que cambie código: `git add` + `git commit` con un mensaje claro en español (y `git push` cuando exista el repositorio remoto).
8. **Seguridad**: nunca escribas contraseñas, `SECRET_KEY`, tokens ni credenciales de MySQL en archivos que vayan al repositorio. Todo eso vive en `.env` (ignorado por Git). Para el PDF y el video solo se usan datos de prueba.
9. Trabajo en equipo (vale 5 de 20 puntos): el docente evalúa la participación demostrable de los cuatro integrantes, y los commits en GitHub son evidencia. Al empezar cada paso, pregunta a Fernando quién del equipo lo va a hacer; si es otro integrante, ayúdalo a dejarle instrucciones claras (por ejemplo, un *issue* en GitHub) en vez de hacerlo tú directamente, salvo que Fernando decida otra cosa.

## 3. Requisitos obligatorios de la Entrega 2 (del enunciado)

- **Python, Django, patrón MVT y MySQL** — indispensables.
- Proyecto Django con aplicaciones organizadas, rutas, vistas y plantillas.
- MySQL: conexión configurada, modelos relacionados y migraciones ejecutadas.
- Autenticación con el mecanismo de Django: **registro, inicio de sesión y cierre de sesión**.
- Ediciones: registrar y consultar distintas ediciones anuales del Festival.
- Categorías vinculadas a una edición; votaciones asociadas a categorías; opciones participantes por votación.
- **Django Admin** para administrar ediciones, categorías, votaciones y opciones (el panel propio se construirá después).
- Interfaz pública que muestre **datos reales de MySQL** de forma dinámica.
- El sistema debe ser **reutilizable año por año**: para una nueva edición solo se configuran datos, sin tocar código.
- Entregables: un único **PDF** por equipo + **repositorio GitHub con README** de instalación y ejecución + **video** con los cuatro integrantes.

**Fuera de alcance en esta entrega** (no lo implementes aún): registro definitivo de votos, control avanzado de duplicados, reglas de elegibilidad, publicación final de resultados, auditoría avanzada, pruebas de carga, despliegue productivo y panel administrativo propio.

## 4. Decisiones técnicas

| Componente | Elección | Motivo |
|---|---|---|
| Python | 3.13 (instalador de python.org) | Compatible con Django 5.2 y con las ruedas de `mysqlclient` para Windows |
| Django | 5.2 LTS (`Django>=5.2,<5.3`) | Versión de soporte extendido (seguridad hasta abril de 2028), estable para los 4 entregables del semestre |
| Base de datos | MySQL Community Server 8.4 LTS + MySQL Workbench | LTS ampliamente documentada; Workbench sirve para las capturas de tablas |
| Conector | `mysqlclient` | Conector recomendado por Django; publica ruedas para Windows |
| Configuración | `python-dotenv` + archivo `.env` | Mantener credenciales fuera del repositorio |
| Frontend | Plantillas Django + HTML5, CSS3, Bootstrap 5 (CDN) y JavaScript | Stack definido en la Entrega 1; diseño *mobile-first* (RNF-03) |
| Usuarios | `django.contrib.auth` (modelo `User` de Django) | "Mecanismo adecuado de Django" exigido por el enunciado; contraseñas con hash (RNF-01) |
| Idioma y zona | `LANGUAGE_CODE = "es-co"`, `TIME_ZONE = "America/Bogota"`, `USE_TZ = True` | |
| Ubicación del código | `C:\dev\festival-votaciones` (fuera de OneDrive) | OneDrive sincronizando `.venv` y `.git` causa bloqueos y errores; GitHub es la copia de respaldo |

## 5. Estructura del proyecto

```
festival-votaciones/
├── config/              # proyecto Django: settings.py, urls.py, wsgi.py
├── votaciones/          # app: Edicion, Categoria, Votacion, Opcion (modelos, admin, vistas públicas)
├── cuentas/             # app: registro, inicio y cierre de sesión
├── templates/           # base.html, registration/login.html, cuentas/..., votaciones/...
├── static/              # css/estilos.css, img/
├── docs/
│   ├── BITACORA.md
│   └── capturas/
├── .env                 # NO se sube (credenciales locales)
├── .env.example         # SÍ se sube (variables sin valores reales)
├── .gitignore           # .venv/, .env, __pycache__/, *.pyc, db.sqlite3, staticfiles/
├── requirements.txt
├── manage.py
├── CLAUDE.md
└── README.md
```

Patrón MVT: **Modelos** en `votaciones/models.py` (tablas MySQL), **Vistas** en `votaciones/views.py` y `cuentas/views.py` (consultan el ORM y eligen plantilla), **Plantillas** en `templates/` (HTML que muestra los datos). Explícale a Fernando esta correspondencia cuando la construyan: va en la sección "Desarrollo técnico" del PDF.

## 6. Modelo de datos (Edición → Categoría → Votación → Opción)

Basado en el modelo conceptual de la Entrega 1. Nombres de clases y campos en español, con `verbose_name` y `verbose_name_plural` para que el Admin se vea en español.

**Edicion**
- `anio` PositiveSmallIntegerField, único (ej. 2027)
- `nombre` CharField(150) (ej. "Festival de la Leyenda Vallenata 2027")
- `descripcion` TextField, opcional
- `fecha_inicio`, `fecha_fin` DateField — validar en `clean()` que fin ≥ inicio
- `activa` BooleanField — la edición que se muestra en la página de inicio; al guardar una edición activa, desactivar las demás (solo una activa)
- `creado`, `actualizado` (auto_now_add / auto_now)
- `ordering = ["-anio"]`

**Categoria**
- `edicion` ForeignKey(Edicion, CASCADE, related_name="categorias")
- `nombre` CharField(150), `descripcion` TextField opcional, `orden` PositiveSmallIntegerField(default=0)
- Restricción única (`edicion`, `nombre`) — RN-04

**Votacion**
- `categoria` ForeignKey(Categoria, CASCADE, related_name="votaciones")
- `titulo` CharField(200), `descripcion` TextField opcional
- `estado` CharField con choices: `BORRADOR`, `ABIERTA`, `CERRADA` (default `BORRADOR`) — RN-02
- `fecha_apertura`, `fecha_cierre` DateTimeField — validar cierre > apertura
- `resultados_publicos` BooleanField(default=False) — RN-05 (pendiente de validación con el cliente; solo se modela)
- `orden` PositiveSmallIntegerField(default=0)
- Propiedad `esta_abierta`: estado `ABIERTA` y la fecha actual entre apertura y cierre

**Opcion**
- `votacion` ForeignKey(Votacion, CASCADE, related_name="opciones") — RN-03
- `nombre` CharField(150), `descripcion` TextField opcional, `orden` PositiveSmallIntegerField(default=0)
- Restricción única (`votacion`, `nombre`)

Todas con `__str__` legible (ej. `"2027 · Canción Vallenata Inédita"`).

**Voto — solo planeado para la Entrega 3, no crearlo ahora.** Diseño previsto, para explicarlo en el PDF como "preparado para incorporar votos": `usuario` FK a User (PROTECT), `votacion` FK (PROTECT), `opcion` FK (PROTECT), `fecha_hora` (auto_now_add, RF-08), y `UniqueConstraint(usuario, votacion)` para RN-01. Cuando exista, `PROTECT` impedirá borrar opciones o votaciones con votos (RN-07).

### Reutilizable año por año

1. Una edición nueva se crea desde el Admin; nada del código depende del año.
2. El campo `activa` decide qué edición ve el público; cambiarla es un clic.
3. **Acción de Admin "Duplicar edición para el año siguiente"**: copia la edición seleccionada con todas sus categorías, votaciones y opciones al año + 1, con la copia inactiva, las votaciones en `BORRADOR` y las fechas corridas un año. Así, para 2028 solo se duplica 2027 y se ajusta lo que cambie. Hazla dentro de una transacción (`transaction.atomic`).

## 7. Ruta de desarrollo paso a paso

Sigue la ruta sugerida del enunciado. No saltes pasos; al cerrar cada uno, actualiza `docs/BITACORA.md` y haz commit.

### Paso 1 — Instalar el software (con evidencias)
1. **Git for Windows** (https://git-scm.com/downloads/win). Verificar `git --version`. Configurar `git config --global user.name` y `user.email` (el mismo correo de GitHub). Tras instalarlo, Claude Code puede usar Git Bash.
2. **Python 3.13** (https://www.python.org/downloads/windows/). En el instalador, marcar **"Add python.exe to PATH"**. Verificar `py --version` y `python --version`. Si `python` abre la Microsoft Store, usar `py` o desactivar los alias en *Configuración → Aplicaciones → Alias de ejecución de aplicaciones*.
3. **MySQL Community Server 8.4 LTS y MySQL Workbench** (https://dev.mysql.com/downloads/). Fernando define la contraseña de `root` y la guarda solo para sí; nunca va al repositorio ni al PDF. Verificar que el servicio MySQL está corriendo y conectarse con Workbench.
4. **Cuenta de GitHub** (https://github.com/signup), con el correo que configuró en Git.
5. Opcional: Visual Studio Code.
`[CAPTURA]` de cada versión verificada y de Workbench conectado → PDF, sección "Desarrollo técnico" (entorno).

### Paso 2 — Repositorio y proyecto Django
1. `git init`, `.gitignore`, `README.md` inicial, primer commit.
2. Crear en GitHub el repositorio `festival-vallenato-votaciones` (**público**, para que el docente pueda abrir el enlace) y conectarlo (`git remote add origin ...`, `git push -u origin main`). El primer push abre el navegador para iniciar sesión (Git Credential Manager).
3. Fernando invita a Sebastián, Javik y Dahiana como colaboradores (*Settings → Collaborators*).
4. Entorno virtual: `py -3.13 -m venv .venv` y activarlo con `.venv\Scripts\Activate.ps1`. Si PowerShell bloquea el script: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`.
5. `pip install "Django>=5.2,<5.3" mysqlclient python-dotenv` y `pip freeze > requirements.txt`.
6. `django-admin startproject config .`, luego `python manage.py startapp votaciones` y `python manage.py startapp cuentas`.
7. Ajustar `settings.py`: apps instaladas, idioma, zona horaria, carpeta `templates`, `static`, y lectura de `.env` con `python-dotenv` (`SECRET_KEY`, `DEBUG`, `ALLOWED_HOSTS`).
8. `python manage.py runserver` y abrir http://127.0.0.1:8000. `[CAPTURA]` página inicial de Django.

### Paso 3 — MySQL, modelos y migraciones
1. En Workbench (o consola `mysql`), crear base y usuario de la aplicación:
   ```sql
   CREATE DATABASE festival_vallenato CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   CREATE USER 'festival_app'@'localhost' IDENTIFIED BY '<contraseña local de Fernando>';
   GRANT ALL PRIVILEGES ON festival_vallenato.* TO 'festival_app'@'localhost';
   GRANT ALL PRIVILEGES ON test_festival_vallenato.* TO 'festival_app'@'localhost';  -- para las pruebas
   FLUSH PRIVILEGES;
   ```
   La contraseña la escribe Fernando directamente; no la repitas en archivos versionados ni en la bitácora.
2. `.env` con `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST=127.0.0.1`, `DB_PORT=3306`; `.env.example` con las mismas claves sin valores.
3. `DATABASES` en `settings.py` con `ENGINE = "django.db.backends.mysql"` y `OPTIONS = {"charset": "utf8mb4"}`.
4. Escribir los modelos de la sección 6, `python manage.py makemigrations` y `python manage.py migrate`.
5. `[CAPTURA]` salida de `migrate` y tablas en Workbench → PDF "Desarrollo técnico" (migraciones) y "Capturas comentadas".

### Paso 4 — Django Admin y datos de prueba
1. `python manage.py createsuperuser` (credenciales locales, no se documentan).
2. `votaciones/admin.py`: registrar los cuatro modelos con `list_display`, `list_filter`, `search_fields` e *inlines* (Categorías dentro de Edición, Votaciones dentro de Categoría, Opciones dentro de Votación), más la acción "Duplicar edición para el año siguiente". Personalizar `admin.site.site_header` ("Administración — Festival Vallenato").
3. Comando de gestión `python manage.py cargar_datos_prueba` que cree, de forma idempotente, las ediciones 2026 (inactiva) y 2027 (activa) con categorías tomadas de la Entrega 1 (Canción Vallenata Inédita, Acordeón Profesional, Acordeonera Mayor y Menor, Acordeonero Aficionado, Juvenil e Infantil, Piquería) y algunas votaciones y opciones. Las opciones usan nombres genéricos ("Participante 1", "Canción A"): **no inventar participantes reales**, y dejar claro en el PDF que son datos de prueba ilustrativos, no categorías confirmadas por el cliente.
4. `[CAPTURA]` lista de ediciones, una edición con sus categorías, una votación con sus opciones, y la acción de duplicar funcionando.

### Paso 5 — Autenticación
1. App `cuentas`: vista de registro con un formulario basado en `UserCreationForm` que pida nombre, correo (único, RN-06) y contraseña; inicio de sesión con `django.contrib.auth.views.LoginView`; cierre con `LogoutView` (en Django 5 el cierre de sesión se hace por **POST**, con un formulario y `{% csrf_token %}`).
2. `LOGIN_URL`, `LOGIN_REDIRECT_URL`, `LOGOUT_REDIRECT_URL` en settings; mensajes con `django.contrib.messages`.
3. La barra de navegación muestra "Iniciar sesión / Registrarse" o "Hola, <nombre> · Cerrar sesión".
4. `[CAPTURA]` registro, error de validación, inicio de sesión exitoso, el usuario nuevo visible en el Admin (con la contraseña cifrada, RNF-01).

### Paso 6 — Vistas, rutas y plantillas públicas
Rutas (en `votaciones/urls.py`, incluidas desde `config/urls.py`):
- `/` — edición activa con sus categorías (o un mensaje si no hay edición activa)
- `/ediciones/` — todas las ediciones (consultar ediciones anteriores)
- `/ediciones/<anio>/` — detalle de una edición y sus categorías
- `/categorias/<id>/` — votaciones de una categoría (no mostrar las que estén en `BORRADOR`)
- `/votaciones/<id>/` — detalle de la votación con sus opciones; botón "Votar" deshabilitado con el texto "Disponible en la próxima entrega"

Usar `get_object_or_404`, `select_related` / `prefetch_related`. Plantillas con `base.html` (Bootstrap 5 por CDN, barra de navegación, mensajes, pie de página). Diseño *mobile-first* inspirado en el sitio oficial del Festival según el análisis de la Entrega 1 (negro, rojo, amarillo/dorado y blanco; tipografías Poppins y Raleway de Google Fonts), siguiendo el flujo del prototipo de la Entrega 1: Inicio → Categorías → Votación → Opciones.
`[CAPTURA]` cada página en computador y en vista móvil (DevTools del navegador, `F12` → modo dispositivo) y un cambio hecho en el Admin que aparece en la página pública (demuestra datos dinámicos de MySQL).

### Paso 7 — Pruebas
1. Pruebas automáticas en `votaciones/tests.py` y `cuentas/tests.py` con `django.test.TestCase`: creación y relaciones de modelos, validaciones de fechas, unicidad, una sola edición activa, duplicar edición, respuestas 200/404 de las vistas, registro, inicio y cierre de sesión. Ejecutar `python manage.py test`.
2. Tabla de pruebas manuales en la bitácora: funcionalidad · procedimiento · resultado esperado · resultado obtenido.
3. `[CAPTURA]` salida de las pruebas → PDF sección "Pruebas y resultados".

### Paso 8 — README, PDF y video
1. `README.md`: descripción, tecnologías, requisitos, instalación paso a paso (clonar, entorno virtual, `pip install -r requirements.txt`, `.env` desde `.env.example`, crear base MySQL, `migrate`, `cargar_datos_prueba`, `createsuperuser`, `runserver`), estructura del proyecto e integrantes.
2. Verificar que un compañero pueda clonar el repositorio y ejecutarlo siguiendo solo el README.
3. Apoyar a Fernando con el borrador del PDF a partir de `docs/BITACORA.md`, con estas secciones: portada (institución, programa, asignatura, proyecto, fecha, los cuatro nombres completos); introducción y objetivo; desarrollo técnico; capturas comentadas; código relevante (fragmentos, no todo el proyecto); pruebas y resultados; enlace al repositorio; enlace al video; conclusiones y distribución del trabajo.
4. Guion del video con un segmento para cada integrante.

## 8. Lista de verificación final (del enunciado)

- [ ] PDF con portada y los cuatro integrantes
- [ ] Aplicación Django ejecutable y conectada a MySQL
- [ ] Evidencias de modelos, migraciones y autenticación
- [ ] Ediciones, categorías, votaciones y opciones administrables y visibles dinámicamente
- [ ] Capturas legibles y explicadas
- [ ] Repositorio con código y README
- [ ] Video con demostración y participación del equipo
- [ ] Enlaces clicables, públicos y verificados desde otra sesión
- [ ] Sin contraseñas, tokens ni credenciales en el PDF, el repo o el video
- [ ] PDF enviado a más tardar el 26 de octubre de 2026

## 9. Referencias de la Entrega 1 que conviene respetar

- Requerimientos que esta entrega cubre: RF-01 (registro), RF-02 (inicio de sesión), RF-03 (consultar categorías y votaciones), RF-06 (administrar categorías, votaciones y opciones, aquí vía Django Admin); RNF-01 (contraseñas cifradas), RNF-03 (responsive), RNF-08 (agregar categorías y votaciones sin cambios estructurales).
- Reglas de negocio modeladas: RN-02, RN-03, RN-04, RN-05 (como campo), RN-06. RN-01, RN-07 y RN-08 quedan para cuando exista el modelo Voto.
- Preguntas aún abiertas con el cliente (frecuencia de voto, jurados, publicación de resultados, categorías definitivas): no inventar respuestas; dejar los datos como "de prueba" y anotar en el PDF que siguen pendientes de validación.
