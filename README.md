# Proyecto-crud-tareas-Django
practica
# 📘 Material para Estudiantes: Aplicación CRUD de Tareas en Python Django

Este documento te guiará paso a paso para crear una **aplicación CRUD de tareas** usando **Django** en **Windows**, ideal para estudiantes.  
Incluye todos los procedimientos, estructura de carpetas, código completo y configuraciones necesarias.

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

> Asegúrate de tener **VS Code instalado** y agregado al PATH. Este comando abre la carpeta actual en VS Code.

---

### 4. Verificar instalación de Python
```cmd
python --version
```

> Debe mostrar algo como: `Python 3.x.x`  
> Si no aparece, instala Python desde [python.org](https://www.python.org/downloads/).

---

### 5. Crear entorno virtual
```cmd
python -m venv venv
```

> Crea una carpeta `venv` con el entorno virtual de Python.

---

### 6. Activar entorno virtual
```cmd
venv\Scripts\activate
```

> Verás `(venv)` al inicio de la línea en la terminal.

---

### 7. Seleccionar intérprete de Python en VS Code
- Abre VS Code.
- Presiona **`Ctrl + Shift + P`**.
- Escribe: **"Python: Select Interpreter"**.
- Elige el que diga: `venv` o con ruta como: `./Crud/venv/...`.

---

### 8. Instalar Django
```cmd
pip install django
```

> Instala Django en el entorno virtual.

---

### 9. Crear proyecto Django “backend_tareas” sin duplicar carpetas
```cmd
django-admin startproject backend_tareas .
```

> El punto (`.`) evita crear una carpeta extra. El proyecto se genera en la carpeta actual.

---

### 10. Ejecutar el servidor Django
```cmd
python manage.py runserver
```

> Abre tu navegador y visita: [http://127.0.0.1:8000](http://127.0.0.1:8000)  
> Debe mostrarse la página de bienvenida de Django.

---

### 11. Crear aplicación “app_tareas”
```cmd
python manage.py startapp app_tareas
```

> Crea una carpeta `app_tareas` con los archivos base de la aplicación.

---

## 🗂️ Estructura completa de carpetas y archivos

Crea esta estructura dentro de la carpeta `Crud/`:

```
Crud/
│
├── venv/                          # Entorno virtual (automático)
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
├── templates/                     # Crear manualmente
│   └── app_tareas/
│       ├── listar_tareas.html
│       ├── crear_tarea.html
│       └── actualizar_tarea.html
│
├── static/                        # Crear manualmente
│   └── css/
│       └── styles.css
│
├── manage.py
└── db.sqlite3                     # Se crea automáticamente con migrate
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

> Estos comandos crean y aplican los cambios en la base de datos.

---

## ⚙️ Configuraciones en settings.py y urls.py

### 📄 `backend_tareas/settings.py`

```python
import os  # Asegúrate de tener esta línea al inicio

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_tareas',  # Agrega tu app aquí
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],  # Ruta a tus plantillas
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
    path('', include('app_tareas.urls')),  # Ruta principal a tu app
]
```

---

### 📄 `app_tareas/urls.py` (crear manualmente)

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

## 🎨 Archivos HTML y CSS

### Crear carpetas

Desde la terminal (dentro de `Crud/`):

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
    background-color: #f0f8ff; /* Azul muy claro */
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

h1 {
    text-align: center;
    color: #3498db;
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
```

---

### 📄 `templates/app_tareas/listar_tareas.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Lista de Tareas</title>
    <link rel="stylesheet" href="/static/css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Mis Tareas</h1>
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
    </div>
</body>
</html>
```

---

### 📄 `templates/app_tareas/crear_tarea.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Crear Tarea</title>
    <link rel="stylesheet" href="/static/css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Crear Nueva Tarea</h1>
        <form method="POST">
            {% csrf_token %}
            <input type="text" name="tarea" placeholder="Escribe la tarea..." required>
            <button type="submit" class="btn btn-crear">Guardar Tarea</button>
        </form>
        <a href="{% url 'listar_tareas' %}">← Volver</a>
    </div>
</body>
</html>
```

---

### 📄 `templates/app_tareas/actualizar_tarea.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Editar Tarea</title>
    <link rel="stylesheet" href="/static/css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Editar Tarea</h1>
        <form method="POST">
            {% csrf_token %}
            <input type="text" name="tarea" value="{{ tarea.tarea }}" required>
            <label>
                <input type="checkbox" name="completado" class="checkbox" {% if tarea.completado %}checked{% endif %}>
                Marcar como completado
            </label>
            <button type="submit" class="btn btn-editar">Actualizar</button>
        </form>
        <a href="{% url 'listar_tareas' %}">← Volver</a>
    </div>
</body>
</html>
```

---

## ▶️ Final: Ejecutar el servidor

```cmd
python manage.py runserver
```

> Abre tu navegador en: [http://127.0.0.1:8000](http://127.0.0.1:8000)

---

## ✅ ¡Listo! Has creado una aplicación CRUD completa de tareas en Django.

### Funcionalidades:
- ✅ Listar tareas
- ✅ Crear tareas
- ✅ Editar tareas
- ✅ Marcar como completadas
- ✅ Eliminar tareas

### Características:
- Sin Bootstrap
- Estilo con CSS personalizado
- Colores suaves
- HTML completo y semántico
- No se usa `forms.py`
- Sin validaciones (como se solicitó)

---

Este material es ideal para estudiantes que inician con Django.  
Todos los pasos están claros, detallados y listos para seguir.

¡Éxito en tu aprendizaje! 🚀
