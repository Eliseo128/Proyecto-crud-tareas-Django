# 📘 Material para Estudiantes: Aplicación CRUD de Tareas en Django con `base.html`

Este documento te guiará paso a paso para crear una **aplicación CRUD de tareas** usando **Django** en **Windows**, ahora con un **archivo base (`base.html`)** para evitar duplicar código HTML. Ideal para estudiantes.

---

## 🔧 Procedimientos Iniciales

### 1. Acceder a la Terminal de Windows
- Presiona las teclas: **`Windows + R`**
- Escribe: **`cmd`**
- Presiona **Enter**

> Esto abrirá la **ventana de Command Prompt (cmd)**.

---

### 2. Crear carpeta de trabajo “Crud”
```cmd
mkdir Crud
cd Crud
```

> Crea una carpeta llamada `Crud` y entra en ella.

---

### 3. Desde terminal, abrir Visual Studio Code
```cmd
code .
```

> Asegúrate de tener **VS Code instalado** y agregado al PATH.

---

### 4. Verificar instalación de Python
```cmd
python --version
```

> Debe mostrar: `Python 3.x.x`

---

### 5. Crear entorno virtual
```cmd
python -m venv venv
```

---

### 6. Activar entorno virtual
```cmd
venv\Scripts\activate
```

> Verás `(venv)` al inicio de la línea.

---

### 7. Seleccionar intérprete de Python en VS Code
- Abre VS Code.
- Presiona **`Ctrl + Shift + P`**.
- Escribe: **"Python: Select Interpreter"**.
- Elige el que diga: `venv`.

---

### 8. Instalar Django
```cmd
pip install django
```

---

### 9. Crear proyecto “backend_tareas” sin duplicar carpetas
```cmd
django-admin startproject backend_tareas .
```

> El punto (`.`) evita una carpeta extra.

---

### 10. Ejecutar el servidor Django
```cmd
python manage.py runserver
```

> Visita: [http://127.0.0.1:8000](http://127.0.0.1:8000)

---

### 11. Crear aplicación “app_tareas”
```cmd
python manage.py startapp app_tareas
```

---

## 🗂️ Estructura completa de carpetas y archivos (actualizada con `base.html`)

```
Crud/
│
├── venv/                          # Entorno virtual
├── backend_tareas/                # Proyecto Django
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── app_tareas/                    # Aplicación de tareas
│   ├── __init__.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py                    # Crear manualmente
│   └── admin.py
│
├── templates/                     # Plantillas
│   └── app_tareas/
│       ├── base.html              # Plantilla base
│       ├── listar_tareas.html
│       ├── crear_tarea.html
│       └── actualizar_tarea.html
│
├── static/                        # Archivos estáticos
│   └── css/
│       └── styles.css
│
├── manage.py
└── db.sqlite3                     # Se crea con migrate
```

---

## 🧱 Modelo: Tarea (tarea, completado)

### 📄 `app_tareas/models.py`

```python
from django.db import models

class Tarea(models.Model):
    tarea = models.CharField(max_length=200)
    completado = models.BooleanField(default=False)

    def __str__(self):
        return self.tarea
```

---

## 🔁 Migraciones: makemigrations y migrate

```cmd
python manage.py makemigrations
python manage.py migrate
```

> Crea y aplica la tabla `app_tareas_tarea` en la base de datos.

---

## ⚙️ Configuraciones en `settings.py` y `urls.py`

### 📄 `backend_tareas/settings.py`

```python
import os

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_tareas',  # Añadir aquí
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],  # Ruta a templates
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),  # Directorio de archivos estáticos
]
```

---

### 📄 `backend_tareas/urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_tareas.urls')),
]
```

---

### 📄 `app_tareas/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.listar_tareas, name='listar_tareas'),
    path('crear/', views.crear_tarea, name='crear_tarea'),
    path('actualizar/<int:id>/', views.actualizar_tarea, name='actualizar_tarea'),
    path('borrar/<int:id>/', views.borrar_tarea, name='borrar_tarea'),
]
```

---

## 🖥️ Vistas basadas en funciones (CRUD)

### 📄 `app_tareas/views.py`

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Tarea

def listar_tareas(request):
    tareas = Tarea.objects.all()
    return render(request, 'app_tareas/listar_tareas.html', {'tareas': tareas})

def crear_tarea(request):
    if request.method == 'POST':
        tarea_nombre = request.POST.get('tarea')
        Tarea.objects.create(tarea=tarea_nombre)
        return redirect('listar_tareas')
    return render(request, 'app_tareas/crear_tarea.html')

def actualizar_tarea(request, id):
    tarea = get_object_or_404(Tarea, id=id)
    if request.method == 'POST':
        tarea.tarea = request.POST.get('tarea')
        tarea.completado = 'completado' in request.POST
        tarea.save()
        return redirect('listar_tareas')
    return render(request, 'app_tareas/actualizar_tarea.html', {'tarea': tarea})

def borrar_tarea(request, id):
    tarea = get_object_or_404(Tarea, id=id)
    tarea.delete()
    return redirect('listar_tareas')
```

---

## 🎨 Archivos HTML: Uso de `base.html`

### 📁 Crear carpetas

```cmd
mkdir templates\app_tareas
mkdir static\css
```

---

### 📄 `static/css/styles.css`

```css
/* Estilos suaves y limpios */
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f0f8ff;
    color: #2c3e50;
    margin: 0;
    padding: 20px;
}

.container {
    max-width: 800px;
    margin: 0 auto;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

header {
    text-align: center;
    margin-bottom: 20px;
}

header h1 {
    color: #3498db;
    margin: 0;
}

nav a {
    margin: 0 10px;
    text-decoration: none;
    color: #3498db;
    font-weight: bold;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
}

th, td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}

th {
    background-color: #ecf0f1;
}

.btn {
    padding: 8px 12px;
    margin: 2px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    text-decoration: none;
    font-size: 14px;
    color: white;
}

.btn-crear {
    background-color: #2ecc71;
}

.btn-editar {
    background-color: #3498db;
}

.btn-borrar {
    background-color: #e74c3c;
}

form {
    margin: 15px 0;
}

input[type="text"] {
    width: 70%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.checkbox {
    margin-left: 10px;
}

footer {
    text-align: center;
    margin-top: 30px;
    color: #7f8c8d;
    font-size: 14px;
}
```

---

### 📄 `templates/app_tareas/base.html` (Plantilla base)

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Tareas Django{% endblock %}</title>
    <link rel="stylesheet" href="/static/css/styles.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>📋 Mis Tareas</h1>
            <nav>
                <a href="{% url 'listar_tareas' %}">Inicio</a>
                <a href="{% url 'crear_tarea' %}">Agregar Tarea</a>
            </nav>
        </header>

        <main>
            {% block content %}
            <!-- Contenido específico de cada página -->
            {% endblock %}
        </main>

        <footer>
            <p>Aplicación CRUD de Tareas - Django | Material para estudiantes</p>
        </footer>
    </div>
</body>
</html>
```

---

### 📄 `templates/app_tareas/listar_tareas.html`

```html
{% extends 'app_tareas/base.html' %}

{% block title %}Lista de Tareas{% endblock %}

{% block content %}
    <a href="{% url 'crear_tarea' %}" class="btn btn-crear">Agregar Tarea</a>

    <table>
        <thead>
            <tr>
                <th>Tarea</th>
                <th>Estado</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            {% for tarea in tareas %}
            <tr>
                <td>{{ tarea.tarea }}</td>
                <td>{% if tarea.completado %}✔ Completado{% else %}⏳ Pendiente{% endif %}</td>
                <td>
                    <a href="{% url 'actualizar_tarea' tarea.id %}" class="btn btn-editar">Editar</a>
                    <a href="{% url 'borrar_tarea' tarea.id %}" class="btn btn-borrar">Borrar</a>
                </td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
{% endblock %}
```

---

### 📄 `templates/app_tareas/crear_tarea.html`

```html
{% extends 'app_tareas/base.html' %}

{% block title %}Crear Tarea{% endblock %}

{% block content %}
    <h2>Crear Nueva Tarea</h2>
    <form method="POST">
        {% csrf_token %}
        <input type="text" name="tarea" placeholder="Escribe la tarea..." required>
        <button type="submit" class="btn btn-crear">Guardar Tarea</button>
    </form>
{% endblock %}
```

---

### 📄 `templates/app_tareas/actualizar_tarea.html`

```html
{% extends 'app_tareas/base.html' %}

{% block title %}Editar Tarea{% endblock %}

{% block content %}
    <h2>Editar Tarea</h2>
    <form method="POST">
        {% csrf_token %}
        <input type="text" name="tarea" value="{{ tarea.tarea }}" required>
        <label>
            <input type="checkbox" name="completado" class="checkbox" {% if tarea.completado %}checked{% endif %}>
            Marcar como completado
        </label>
        <button type="submit" class="btn btn-editar">Actualizar</button>
    </form>
{% endblock %}
```

---

## ▶️ Final: Ejecutar el servidor

```cmd
python manage.py runserver
```

> Abre tu navegador en: [http://127.0.0.1:8000](http://127.0.0.1:8000)

---

## ✅ ¡Listo! Has creado una aplicación CRUD con plantilla base

### ✅ Ventajas del `base.html`:
- Evita duplicar código (menú, estilos, estructura).
- Fácil de mantener.
- Todas las páginas heredan el mismo diseño.

### ✅ Funcionalidades:
- Listar, crear, editar y borrar tareas.
- Diseño limpio y coherente.
- Sin Bootstrap, solo CSS personalizado.
- Navegación clara entre páginas.

---

Este material es ideal para estudiantes que inician con Django.  
Con el uso de `base.html`, aprendes buenas prácticas de desarrollo web.

¡Éxito en tu aprendizaje! 🚀
