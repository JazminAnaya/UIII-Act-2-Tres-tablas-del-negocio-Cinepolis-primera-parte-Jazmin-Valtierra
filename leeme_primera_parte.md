¡Perfecto — vamos a dejar todo listo paso a paso para crear tu proyecto **UIII_Cinepolis_0716** en **Django** (Python) usando **VS Code** y con el CRUD de **Sucursal** tal como pediste. Te doy los procedimientos, comandos y fragmentos de código listos para copiar/pegar.

# 1 — Crear la carpeta del proyecto

En tu explorador de archivos o desde la terminal:

```bash
# desde la carpeta donde quieras crear el proyecto
mkdir UIII_Cinepolis_0716
cd UIII_Cinepolis_0716
```

# 2 — Abrir VS Code en la carpeta

Desde la terminal (estando dentro de `UIII_Cinepolis_0716`):

```bash
code .
```

(O abre VS Code y usa *File > Open Folder...* y selecciona `UIII_Cinepolis_0716`.)

# 3 — Abrir la terminal integrada en VS Code

Menú `Terminal > New Terminal` o atajo:

* Windows/Linux: ``Ctrl+` ``
* macOS: ``Cmd+` ``

# 4 — Crear la carpeta del entorno virtual `.venv`

(Recomendado crear el venv dentro de la carpeta del proyecto y nombrarlo `.venv`.)

```bash
# Windows (PowerShell)
python -m venv .venv

# macOS / Linux
python3 -m venv .venv
```

# 5 — Activar el entorno virtual

Dependiendo de tu sistema/terminal:

* CMD (Windows):

```cmd
.venv\Scripts\activate
```

* macOS / Linux (bash/zsh):

```bash
source .venv/bin/activate
```

Cuando esté activo verás `(.venv)` al inicio del prompt.

# 6 — Activar intérprete de Python en VS Code

1. `Ctrl+Shift+P` (o `Cmd+Shift+P` en macOS).
2. Escribe `Python: Select Interpreter`.
3. Selecciona el intérprete que apunte a `.../UIII_Cinepolis_0716/.venv/bin/python` (o `.venv\Scripts\python.exe` en Windows).

# 7 — Instalar Django

Con el venv activo:

```bash
pip install django
```

(opcional: `pip install django==4.2` si quieres una versión específica)

# 8 — Crear proyecto `backend_Cinepolis` sin duplicar carpeta

Estando dentro de `UIII_Cinepolis_0716` y queriendo que el proyecto esté en la raíz (sin crear una subcarpeta extra), ejecuta:

```bash
django-admin startproject backend_Cinepolis .
```

El punto `.` indica crear los archivos del proyecto en el directorio actual (evita duplicar una carpeta).

# 9 — Ejecutar servidor en el puerto 8716

```bash
python manage.py runserver 0.0.0.0:8716
# o, si quieres sólo en localhost:
python manage.py runserver 127.0.0.1:8716
```

# 10 — Copiar y pegar el link en el navegador

Abre el navegador y pega:

```
http://127.0.0.1:8716/
```

(o usa la IP que muestra la terminal si usaste `0.0.0.0` — normalmente `127.0.0.1:8716` funciona localmente).

# 11 — Crear la aplicación `app_Cinepolis`

Con el venv activo y dentro de la raíz del proyecto:

```bash
python manage.py startapp app_Cinepolis
```

Ahora tendrás la carpeta `app_Cinepolis`.

# 12 — El `models.py`

Reemplaza `app_Cinepolis/models.py` con el código que ya proporcionaste (lo copio con ligera mejora de formato). — **YA LO TIENES** en tu mensaje, pero aquí lo dejo listo para pegar:

```python
from django.db import models

# MODELO: Sucursal
class Sucursal(models.Model):
    ESTADOS = [
        ('activo', 'Activo'),
        ('cerrado', 'Cerrado'),
        ('mantenimiento', 'Mantenimiento'),
    ]
    FORMATOS = [
        ('tradicional', 'Tradicional'),
        ('3D', '3D'),
        ('4DX', '4DX'),
        ('VIP', 'VIP'),
        ('IMAX', 'IMAX'),
        ('JUNIOR', 'Junior'),
    ]
    id_sucursal = models.AutoField(primary_key=True)
    nombre_cine = models.CharField(max_length=100)
    direccion = models.CharField(max_length=200)
    ciudad = models.CharField(max_length=100)
    telefono = models.CharField(max_length=20)
    numero_salas = models.IntegerField()
    estado = models.CharField(max_length=20, choices=ESTADOS, default='activo')
    formatos = models.CharField(max_length=20, choices=FORMATOS, default='tradicional')

    def __str__(self):
        return f"{self.nombre_cine} - {self.ciudad}"

# MODELO: Sala (pendiente usar)
class Sala(models.Model):
    TIPOS_SALA = [
        ('Tradicional', 'Tradicional'),
        ('3D', '3D'),
        ('4DX', '4DX'),
        ('VIP', 'VIP'),
        ('IMAX', 'IMAX'),
        ('JUNIOR', 'Junior'),
    ]
    ESTADOS_SALA = [
        ('Ocupada', 'Ocupada'),
        ('Desocupada', 'Desocupada'),
        ('En mantenimiento', 'En mantenimiento'),
    ]
    id_sala = models.AutoField(primary_key=True)
    numero_sala = models.IntegerField()
    tipo_sala = models.CharField(max_length=20, choices=TIPOS_SALA, default='Tradicional')
    capacidad = models.IntegerField()
    estado = models.CharField(max_length=20, choices=ESTADOS_SALA, default='Desocupada')
    fecha_ultimo_mantenimiento = models.DateField()
    asientos_especiales = models.IntegerField()
    sucursal = models.ForeignKey(Sucursal, on_delete=models.CASCADE, related_name='salas')

    def __str__(self):
        return f"Sala {self.numero_sala} - {self.tipo_sala} ({self.sucursal.nombre_cine})"

# MODELO: Pelicula (pendiente usar)
class Pelicula(models.Model):
    id_pelicula = models.AutoField(primary_key=True)
    titulo = models.CharField(max_length=200)
    genero = models.CharField(max_length=100)
    clasificacion = models.CharField(max_length=100)
    duracion = models.IntegerField(help_text="Duración en minutos")
    sinopsis = models.TextField()
    director = models.CharField(max_length=150)
    salas = models.ManyToManyField(Sala, related_name='peliculas')

    def __str__(self):
        return self.titulo
```

# 12.5 — Procedimiento para realizar migraciones

```bash
# crear migraciones para app_Cinepolis
python manage.py makemigrations app_Cinepolis

# aplicar migraciones
python manage.py migrate
```

# 13 — Empezar trabajando sólo con el MODELO: SUCURSAL

(Como pediste, dejamos Sala y Pelicula pendientes. El modelo Sucursal está listo.)

# 14 — `views.py` de `app_Cinepolis` (funciones CRUD para Sucursal)

En `app_Cinepolis/views.py` pega lo siguiente (no usamos `forms.py`, manejamos `request.POST`):

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Sucursal

def inicio_cinepolis(request):
    return render(request, 'inicio.html')

def agregar_sucursal(request):
    if request.method == 'POST':
        # obtener datos del POST sin validación (según indicación)
        nombre = request.POST.get('nombre_cine')
        direccion = request.POST.get('direccion')
        ciudad = request.POST.get('ciudad')
        telefono = request.POST.get('telefono')
        numero_salas = request.POST.get('numero_salas') or 0
        estado = request.POST.get('estado') or 'activo'
        formatos = request.POST.get('formatos') or 'tradicional'

        suc = Sucursal(
            nombre_cine=nombre,
            direccion=direccion,
            ciudad=ciudad,
            telefono=telefono,
            numero_salas=int(numero_salas),
            estado=estado,
            formatos=formatos
        )
        suc.save()
        return redirect('ver_sucursales')

    return render(request, 'sucursal/agregar_sucursal.html')

def ver_sucursales(request):
    sucursales = Sucursal.objects.all()
    return render(request, 'sucursal/ver_sucursales.html', {'sucursales': sucursales})

def actualizar_sucursal(request, id_sucursal):
    suc = get_object_or_404(Sucursal, pk=id_sucursal)
    return render(request, 'sucursal/actualizar_sucursal.html', {'sucursal': suc})

def realizar_actualizacion_sucursal(request, id_sucursal):
    suc = get_object_or_404(Sucursal, pk=id_sucursal)
    if request.method == 'POST':
        suc.nombre_cine = request.POST.get('nombre_cine')
        suc.direccion = request.POST.get('direccion')
        suc.ciudad = request.POST.get('ciudad')
        suc.telefono = request.POST.get('telefono')
        suc.numero_salas = int(request.POST.get('numero_salas') or suc.numero_salas)
        suc.estado = request.POST.get('estado') or suc.estado
        suc.formatos = request.POST.get('formatos') or suc.formatos
        suc.save()
        return redirect('ver_sucursales')
    # si alguien entra por GET redirigimos al formulario de edición
    return redirect('actualizar_sucursal', id_sucursal=id_sucursal)

def borrar_sucursal(request, id_sucursal):
    suc = get_object_or_404(Sucursal, pk=id_sucursal)
    if request.method == 'POST':
        suc.delete()
        return redirect('ver_sucursales')
    return render(request, 'sucursal/borrar_sucursal.html', {'sucursal': suc})
```

# 15 — Crear carpeta `templates` en `app_Cinepolis`

Estructura sugerida:

```
app_Cinepolis/
  templates/
    base.html
    header.html
    navbar.html
    footer.html
    inicio.html
    sucursal/
      agregar_sucursal.html
      ver_sucursales.html
      actualizar_sucursal.html
      borrar_sucursal.html
```

Crea las carpetas y archivos (desde terminal):

```bash
mkdir -p app_Cinepolis/templates/sucursal
code app_Cinepolis/templates/base.html
# y demás archivos
```

# 16 — Archivos HTML principales (ejemplos listos)

## `base.html`

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Sistema de Administración Cinepolis</title>
  <!-- Bootstrap CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- Bootstrap Icons -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.5/font/bootstrap-icons.css" rel="stylesheet">
  <style>
    body { padding-top: 70px; padding-bottom: 70px; }
    .footer-fixed {
      position: fixed;
      bottom: 0;
      width: 100%;
      background: #f8f9fa;
      border-top: 1px solid #e7e7e7;
      padding: 10px 0;
    }
  </style>
</head>
<body>
  {% include 'navbar.html' %}
  <div class="container mt-4">
    {% block content %}{% endblock %}
  </div>
  {% include 'footer.html' %}

  <!-- Bootstrap JS -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

## `navbar.html`

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light fixed-top">
  <div class="container">
    <a class="navbar-brand" href="{% url 'inicio' %}">
      <i class="bi bi-film"></i> Sistema de Administración Cinepolis
    </a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navmenu">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navmenu">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="{% url 'inicio' %}"><i class="bi bi-house"></i> Inicio</a></li>

        <!-- Sucursales -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown"><i class="bi bi-geo-alt"></i> Sucursal</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'agregar_sucursal' %}">Agregar sucursal</a></li>
            <li><a class="dropdown-item" href="{% url 'ver_sucursales' %}">Ver sucursales</a></li>
            <li><a class="dropdown-item" href="#">Actualizar sucursal</a></li>
            <li><a class="dropdown-item" href="#">Borrar sucursal</a></li>
          </ul>
        </li>

        <!-- Salas -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown"><i class="bi bi-door-open"></i> Salas</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar salas</a></li>
            <li><a class="dropdown-item" href="#">Ver salas</a></li>
            <li><a class="dropdown-item" href="#">Actualizar salas</a></li>
            <li><a class="dropdown-item" href="#">Borrar salas</a></li>
          </ul>
        </li>

        <!-- Peliculas -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown"><i class="bi bi-camera-reels"></i> Películas</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar películas</a></li>
            <li><a class="dropdown-item" href="#">Ver películas</a></li>
            <li><a class="dropdown-item" href="#">Actualizar películas</a></li>
            <li><a class="dropdown-item" href="#">Borrar películas</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

## `footer.html`

```html
<footer class="footer-fixed text-center">
  <div class="container">
    <small>&copy; {{ now.year }} Derechos reservados. Creado por Jazmin Valtierra, Cbtis 128</small>
  </div>
</footer>
```

> Nota: en tus `views` o en `context_processors` puedes enviar `now` (o usar la templatetag `{% now "Y" %}`) — ejemplo mínimo: en las plantillas puedes usar `{% now "Y" as year %}` y luego `{{ year }}` si no quieres tocar views.

## `inicio.html`

```html
{% extends 'base.html' %}
{% block content %}
  <div class="row">
    <div class="col-md-8">
      <h1>Bienvenido al sistema de administración - Cinépolis</h1>
      <p>Usa el menú para administrar sucursales, salas y películas.</p>
    </div>
    <div class="col-md-4">
      <img src="https://upload.wikimedia.org/wikipedia/commons/3/3b/Cinemark_theater.jpg" class="img-fluid rounded" alt="Cine">
    </div>
  </div>
{% endblock %}
```

(Puedes cambiar la URL de la imagen por una de Cinepolis de la web.)

# 21 — Crear subcarpeta `sucursal` en templates

(Ya incluida en la estructura del punto 15.)

# 22 — Templates para sucursal (código)

### `sucursal/agregar_sucursal.html`

```html
{% extends 'base.html' %}
{% block content %}
  <h2>Agregar Sucursal</h2>
  <form method="post">
    {% csrf_token %}
    <div class="mb-3">
      <label class="form-label">Nombre cine</label>
      <input name="nombre_cine" class="form-control" required>
    </div>
    <div class="mb-3">
      <label class="form-label">Dirección</label>
      <input name="direccion" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Ciudad</label>
      <input name="ciudad" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Teléfono</label>
      <input name="telefono" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Número salas</label>
      <input name="numero_salas" type="number" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Estado</label>
      <select name="estado" class="form-select">
        <option value="activo">Activo</option>
        <option value="cerrado">Cerrado</option>
        <option value="mantenimiento">Mantenimiento</option>
      </select>
    </div>
    <div class="mb-3">
      <label class="form-label">Formato</label>
      <select name="formatos" class="form-select">
        <option value="tradicional">Tradicional</option>
        <option value="3D">3D</option>
        <option value="4DX">4DX</option>
        <option value="VIP">VIP</option>
        <option value="IMAX">IMAX</option>
        <option value="JUNIOR">Junior</option>
      </select>
    </div>
    <button class="btn btn-primary" type="submit">Guardar</button>
  </form>
{% endblock %}
```

### `sucursal/ver_sucursales.html`

```html
{% extends 'base.html' %}
{% block content %}
  <h2>Lista de Sucursales</h2>
  <table class="table table-striped">
    <thead>
      <tr>
        <th>ID</th><th>Nombre</th><th>Ciudad</th><th>Teléfono</th><th># Salas</th><th>Estado</th><th>Formato</th><th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      {% for s in sucursales %}
      <tr>
        <td>{{ s.id_sucursal }}</td>
        <td>{{ s.nombre_cine }}</td>
        <td>{{ s.ciudad }}</td>
        <td>{{ s.telefono }}</td>
        <td>{{ s.numero_salas }}</td>
        <td>{{ s.estado }}</td>
        <td>{{ s.formatos }}</td>
        <td>
          <a class="btn btn-sm btn-info" href="{% url 'actualizar_sucursal' s.id_sucursal %}">Editar</a>
          <a class="btn btn-sm btn-danger" href="{% url 'borrar_sucursal' s.id_sucursal %}">Borrar</a>
        </td>
      </tr>
      {% empty %}
      <tr><td colspan="8">No hay sucursales registradas.</td></tr>
      {% endfor %}
    </tbody>
  </table>
{% endblock %}
```

### `sucursal/actualizar_sucursal.html`

```html
{% extends 'base.html' %}
{% block content %}
  <h2>Actualizar Sucursal</h2>
  <form method="post" action="{% url 'realizar_actualizacion_sucursal' sucursal.id_sucursal %}">
    {% csrf_token %}
    <div class="mb-3">
      <label class="form-label">Nombre cine</label>
      <input name="nombre_cine" value="{{ sucursal.nombre_cine }}" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Dirección</label>
      <input name="direccion" value="{{ sucursal.direccion }}" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Ciudad</label>
      <input name="ciudad" value="{{ sucursal.ciudad }}" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Teléfono</label>
      <input name="telefono" value="{{ sucursal.telefono }}" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Número salas</label>
      <input name="numero_salas" type="number" value="{{ sucursal.numero_salas }}" class="form-control">
    </div>
    <div class="mb-3">
      <label class="form-label">Estado</label>
      <select name="estado" class="form-select">
        <option value="activo" {% if sucursal.estado == 'activo' %}selected{% endif %}>Activo</option>
        <option value="cerrado" {% if sucursal.estado == 'cerrado' %}selected{% endif %}>Cerrado</option>
        <option value="mantenimiento" {% if sucursal.estado == 'mantenimiento' %}selected{% endif %}>Mantenimiento</option>
      </select>
    </div>
    <div class="mb-3">
      <label class="form-label">Formato</label>
      <select name="formatos" class="form-select">
        <option value="tradicional" {% if sucursal.formatos == 'tradicional' %}selected{% endif %}>Tradicional</option>
        <option value="3D" {% if sucursal.formatos == '3D' %}selected{% endif %}>3D</option>
        <option value="4DX" {% if sucursal.formatos == '4DX' %}selected{% endif %}>4DX</option>
        <option value="VIP" {% if sucursal.formatos == 'VIP' %}selected{% endif %}>VIP</option>
        <option value="IMAX" {% if sucursal.formatos == 'IMAX' %}selected{% endif %}>IMAX</option>
        <option value="JUNIOR" {% if sucursal.formatos == 'JUNIOR' %}selected{% endif %}>Junior</option>
      </select>
    </div>
    <button class="btn btn-success" type="submit">Actualizar</button>
  </form>
{% endblock %}
```

### `sucursal/borrar_sucursal.html`

```html
{% extends 'base.html' %}
{% block content %}
  <h2>Borrar Sucursal</h2>
  <p>¿Estás seguro que deseas borrar la sucursal <strong>{{ sucursal.nombre_cine }}</strong>?</p>
  <form method="post">
    {% csrf_token %}
    <a class="btn btn-secondary" href="{% url 'ver_sucursales' %}">Cancelar</a>
    <button class="btn btn-danger" type="submit">Borrar</button>
  </form>
{% endblock %}
```

# 23 — No utilizar `forms.py`

Tal como pediste, los formularios se procesan manualmente en `views.py` con `request.POST`; por eso no se creó `forms.py`.

# 24 — `urls.py` en `app_Cinepolis` (para el CRUD)

Crea `app_Cinepolis/urls.py` con:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_cinepolis, name='inicio'),
    path('sucursal/agregar/', views.agregar_sucursal, name='agregar_sucursal'),
    path('sucursal/ver/', views.ver_sucursales, name='ver_sucursales'),
    path('sucursal/actualizar/<int:id_sucursal>/', views.actualizar_sucursal, name='actualizar_sucursal'),
    path('sucursal/realizar_actualizacion/<int:id_sucursal>/', views.realizar_actualizacion_sucursal, name='realizar_actualizacion_sucursal'),
    path('sucursal/borrar/<int:id_sucursal>/', views.borrar_sucursal, name='borrar_sucursal'),
]
```

# 25 — Agregar `app_Cinepolis` en `settings.py`

En `backend_Cinepolis/settings.py`, dentro de `INSTALLED_APPS` añade:

```python
INSTALLED_APPS = [
    # apps Django...
    'app_Cinepolis',
    # ...
]
```

# 26 — Configurar `urls.py` de `backend_Cinepolis`

En `backend_Cinepolis/urls.py` reemplaza/añade:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Cinepolis.urls')),  # enrutar a tu app_Cinepolis
]
```

# 27 — Registrar modelos en `admin.py` y volver a migrar

En `app_Cinepolis/admin.py`:

```python
from django.contrib import admin
from .models import Sucursal, Sala, Pelicula

@admin.register(Sucursal)
class SucursalAdmin(admin.ModelAdmin):
    list_display = ('id_sucursal', 'nombre_cine', 'ciudad', 'telefono', 'numero_salas', 'estado', 'formatos')

# Opcional registrar Sala y Pelicula aunque los dejes pendientes
admin.site.register(Sala)
admin.site.register(Pelicula)
```

Luego:

```bash
python manage.py makemigrations
python manage.py migrate
```

Y crear superusuario si quieres acceder al admin:

```bash
python manage.py createsuperuser
```

# 27 (bis) — Sólo trabajar por ahora con Sucursal

Como pediste: deja Sala y Pelicula como modelos en el código pero **no** los uses en las vistas/plantillas hasta después.

# 28 — Estética: colores suaves y diseño sencillo

En `base.html` ya incluí Bootstrap y un CSS inline minimalista. Cambia el esquema con tu CSS si quieres colores exactos.

# 29 — Crear la estructura completa de carpetas y archivos al inicio

Resumen de la estructura recomendada:

```
UIII_Cinepolis_0716/
├─ .venv/
├─ backend_Cinepolis/
│   ├─ __init__.py
│   ├─ settings.py
│   ├─ urls.py
│   └─ wsgi.py
├─ app_Cinepolis/
│   ├─ migrations/
│   ├─ templates/
│   │  ├─ base.html
│   │  ├─ navbar.html
│   │  ├─ footer.html
│   │  ├─ inicio.html
│   │  └─ sucursal/
│   │     ├─ agregar_sucursal.html
│   │     ├─ ver_sucursales.html
│   │     ├─ actualizar_sucursal.html
│   │     └─ borrar_sucursal.html
│   ├─ admin.py
│   ├─ apps.py
│   ├─ models.py
│   ├─ views.py
│   └─ urls.py
├─ manage.py
```

# 30 — Proyecto totalmente funcional

Siguiendo los pasos anteriores deberías tener el CRUD básico de sucursales operativo — añadir, ver, editar y borrar.

# 31 — Ejecutar servidor en el puerto 8716 (recapitulación final)

```bash
# con venv activo
python manage.py runserver 127.0.0.1:8716
# abrir en navegador:
http://127.0.0.1:8716/
```

---

## Consejos rápidos y recordatorios

* Asegúrate de que `DEBUG = True` en `settings.py` mientras desarrollas.
* Si cambias templates o static, recuerda reiniciar el servidor en raros casos (generalmente no es necesario).
* Para que `now.year` en `footer.html` funcione: o usas `{% now "Y" %}` dentro de la plantilla o creas un context processor; para simplificar, cambia `{{ now.year }}` por `{% now "Y" as year %} {{ year }}` dentro del `footer.html`.
* Si más adelante quieres validación y seguridad, migramos los formularios a `forms.py` y agregamos validaciones.

---

¿Quieres que yo genere los archivos completos (`views.py`, `urls.py`, `admin.py`, `models.py`, y las plantillas) en un único bloque listo para pegar en tu proyecto? Puedo generarlos todos y también un `README.md` con los comandos exactos para ejecutarlo. ¿Lo preparo?
