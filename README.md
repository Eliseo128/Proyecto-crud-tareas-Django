# Proyecto-crud-tareas-Django
practica

```text
# 📘 Material para Estudiantes: Aplicación CRUD de Tareas en Python Django

Este documento guía paso a paso la creación de una **aplicación CRUD de tareas** usando **Django** en **Windows**, ideal para estudiantes.  
Se detallan todos los procedimientos, estructura de carpetas, código completo y configuraciones necesarias.

---

## 🔧 PROCEDIMIENTOS INICIALES

### ✅ 1. Acceder a la Terminal de Windows
```text
- Presiona las teclas: [Windows + R]
- Escribe: "cmd"
- Presiona [Enter]
```

> Esto abrirá la **ventana de Command Prompt (cmd)**.

---

### ✅ 2. Crear carpeta de trabajo “Crud”
```cmd
mkdir Crud
cd Crud
```

> Crea una carpeta llamada `Crud` y entra en ella.

---

### ✅ 3. Desde terminal, abrir Visual Studio Code
```cmd
code .
```

> Asegúrate de tener **VS Code instalado** y agregado al PATH. Este comando abre la carpeta actual en VS Code.

---

### ✅ 4. Verificar instalación de Python
```cmd
python --version
```

> Salida esperada: `Python 3.x.x`  
> Si no está instalado, descarga Python desde [python.org](https://www.python.org/downloads/).

---

### ✅ 5. Crear entorno virtual
```cmd
python -m venv venv
```

> Crea una carpeta `venv` con el entorno virtual.

---

### ✅ 6. Activar entorno virtual
```cmd
venv\Scripts\activate
```

> Verás `(venv)` al inicio de la línea en la terminal.

---

### ✅ 7. Seleccionar intérprete de Python en VS Code
```text
- Abre VS Code
- Presiona [Ctrl + Shift + P]
- Escribe: "Python: Select Interpreter"
- Selecciona el que diga: "venv" o con ruta hacia tu carpeta Crud\venv\...
```

---

### ✅ 8. Instalar Django
```cmd
pip install django
```

> Instala Django en el entorno virtual.

---

### ✅ 9. Crear proyecto Django “backend_tareas” sin duplicar carpetas
```cmd
django-admin startproject backend_tareas .
```

> El punto (`.`) evita crear una carpeta extra. El proyecto se crea en la carpeta actual.

---

### ✅ 10. Ejecutar el servidor Django
```cmd
python manage.py runserver
```

> Abre tu navegador y ve a: `http://127.0.0.1:8000`  
> Debe mostrarse la página de bienvenida de Django.

---

### ✅ 11. Crear aplicación “app_tareas”
```cmd
python manage.py startapp app_tareas
```

> Crea una carpeta `app_tareas` con los archivos base de la app.

---

## 🗂️ Estructura completa de carpetas y archivos (crea esta estructura)

```text
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
├── templates/                     # Crear manualmente
│   └── app_tareas/
│       ├── listar_tareas.html
│       ├── crear_tarea.html
│       └── actualizar_tarea.html
│
├── static/                        # Crear dentro de templates
│   └── css/
│       └── styles.css
│
├── manage.py
└── db.sqlite3                     # Se crea al hacer migrate
```

---

## 🧱 Modelo: Tarea (tarea, completado)

### 📄 app_tareas/models.py
```python
from django.db import models

# <span style="color: red;">class</span> <span style="color: white;">Tarea</span>(<span style="color: white;">models.Model</span>):
    # <span style="color: white;">tarea</span> = <span style="color: red;">models.CharField</span>(<span style="color: yellow;">max_length=200</span>)
    # <span style="color: white;">completado</span> = <span style="color: red;">models.BooleanField</span>(<span style="color: yellow;">default=False</span>)

    # <span style="color: red;">def</span> <span style="color: white;">__str__</span>(<span style="color: white;">self</span>):
        # <span style="color: red;">return</span> <span style="color: white;">self.tarea</span>
```

---

## 🔁 Migraciones: makemigrations y migrate

```cmd
python manage.py makemigrations
python manage.py migrate
```

> Genera y aplica la migración para crear la tabla en la base de datos.

---

## ⚙️ Configuraciones en settings.py y urls.py

### 📄 backend_tareas/settings.py
```python
# ... otras configuraciones ...

# <span style="color: green;"># Agregar 'app_tareas' a INSTALLED_APPS</span>
<span style="color: red;">INSTALLED_APPS</span> = [
    <span style="color: yellow;">'django.contrib.admin'</span>,
    <span style="color: yellow;">'django.contrib.auth'</span>,
    <span style="color: yellow;">'django.contrib.contenttypes'</span>,
    <span style="color: yellow;">'django.contrib.sessions'</span>,
    <span style="color: yellow;">'django.contrib.messages'</span>,
    <span style="color: yellow;">'django.contrib.staticfiles'</span>,
    <span style="color: yellow;">'app_tareas'</span>,  <span style="color: green;"># Añadir esta línea</span>
]

# <span style="color: green;"># Configuración de templates</span>
<span style="color: red;">TEMPLATES</span> = [
    {
        <span style="color: yellow;">'BACKEND'</span>: <span style="color: yellow;">'django.template.backends.django.DjangoTemplates'</span>,
        <span style="color: yellow;">'DIRS'</span>: [<span style="color: yellow;">os.path.join(BASE_DIR, 'templates')</span>],  <span style="color: green;"># Añadir esta línea</span>
        <span style="color: yellow;">'APP_DIRS'</span>: <span style="color: red;">True</span>,
        <span style="color: yellow;">'OPTIONS'</span>: {
            <span style="color: yellow;">'context_processors'</span>: [
                <span style="color: yellow;">'django.template.context_processors.debug'</span>,
                <span style="color: yellow;">'django.template.context_processors.request'</span>,
                <span style="color: yellow;">'django.contrib.auth.context_processors.auth'</span>,
                <span style="color: yellow;">'django.contrib.messages.context_processors.messages'</span>,
            ],
        },
    },
]

# <span style="color: green;"># Configuración de archivos estáticos</span>
<span style="color: red;">STATIC_URL</span> = <span style="color: yellow;">'/static/'</span>
<span style="color: red;">STATICFILES_DIRS</span> = [
    <span style="color: yellow;">os.path.join(BASE_DIR, 'static')</span>,
]
```

> Asegúrate de tener `import os` al inicio del archivo `settings.py`.

---

### 📄 backend_tareas/urls.py
```python
from django.contrib import admin
from django.urls import path, include

# <span style="color: red;">urlpatterns</span> = [
    <span style="color: red;">path</span>(<span style="color: yellow;">'admin/'</span>, <span style="color: white;">admin.site.urls</span>),
    <span style="color: red;">path</span>(<span style="color: yellow;">''</span>, <span style="color: red;">include</span>(<span style="color: yellow;">'app_tareas.urls'</span>)),  <span style="color: green;"># Ruta principal</span>
]
```

---

### 📄 app_tareas/urls.py (crear manualmente)
```python
from django.urls import path
from . import views

# <span style="color: red;">urlpatterns</span> = [
    <span style="color: red;">path</span>(<span style="color: yellow;">''</span>, <span style="color: white;">views.listar_tareas</span>, <span style="color: yellow;">name='listar_tareas'</span>),
    <span style="color: red;">path</span>(<span style="color: yellow;">'crear/'</span>, <span style="color: white;">views.crear_tarea</span>, <span style="color: yellow;">name='crear_tarea'</span>),
    <span style="color: red;">path</span>(<span style="color: yellow;">'actualizar/<int:id>/'</span>, <span style="color: white;">views.actualizar_tarea</span>, <span style="color: yellow;">name='actualizar_tarea'</span>),
    <span style="color: red;">path</span>(<span style="color: yellow;">'borrar/<int:id>/'</span>, <span style="color: white;">views.borrar_tarea</span>, <span style="color: yellow;">name='borrar_tarea'</span>),
]
```

---

## 🖥️ Vistas basadas en funciones (CRUD)

### 📄 app_tareas/views.py
```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Tarea

# <span style="color: red;">def</span> <span style="color: white;">listar_tareas</span>(<span style="color: white;">request</span>):
    <span style="color: white;">tareas</span> = <span style="color: white;">Tarea.objects.all</span>()
    <span style="color: red;">return</span> <span style="color: white;">render</span>(<span style="color: white;">request</span>, <span style="color: yellow;">'app_tareas/listar_tareas.html'</span>, {<span style="color: yellow;">'tareas'</span>: <span style="color: white;">tareas</span>})

# <span style="color: red;">def</span> <span style="color: white;">crear_tarea</span>(<span style="color: white;">request</span>):
    <span style="color: red;">if</span> <span style="color: white;">request.method</span> == <span style="color: yellow;">'POST'</span>:
        <span style="color: white;">tarea_nombre</span> = <span style="color: white;">request.POST.get</span>(<span style="color: yellow;">'tarea'</span>)
        <span style="color: white;">Tarea.objects.create</span>(<span style="color: white;">tarea=tarea_nombre</span>)
        <span style="color: red;">return</span> <span style="color: white;">redirect</span>(<span style="color: yellow;">'listar_tareas'</span>)
    <span style="color: red;">return</span> <span style="color: white;">render</span>(<span style="color: white;">request</span>, <span style="color: yellow;">'app_tareas/crear_tarea.html'</span>)

# <span style="color: red;">def</span> <span style="color: white;">actualizar_tarea</span>(<span style="color: white;">request</span>, <span style="color: white;">id</span>):
    <span style="color: white;">tarea</span> = <span style="color: white;">get_object_or_404</span>(<span style="color: white;">Tarea</span>, <span style="color: white;">id=id</span>)
    <span style="color: red;">if</span> <span style="color: white;">request.method</span> == <span style="color: yellow;">'POST'</span>:
        <span style="color: white;">tarea.tarea</span> = <span style="color: white;">request.POST.get</span>(<span style="color: yellow;">'tarea'</span>)
        <span style="color: white;">tarea.completado</span> = <span style="color: yellow;">'completado'</span> <span style="color: red;">in</span> <span style="color: white;">request.POST</span>
        <span style="color: white;">tarea.save</span>()
        <span style="color: red;">return</span> <span style="color: white;">redirect</span>(<span style="color: yellow;">'listar_tareas'</span>)
    <span style="color: red;">return</span> <span style="color: white;">render</span>(<span style="color: white;">request</span>, <span style="color: yellow;">'app_tareas/actualizar_tarea.html'</span>, {<span style="color: yellow;">'tarea'</span>: <span style="color: white;">tarea</span>})

# <span style="color: red;">def</span> <span style="color: white;">borrar_tarea</span>(<span style="color: white;">request</span>, <span style="color: white;">id</span>):
    <span style="color: white;">tarea</span> = <span style="color: white;">get_object_or_404</span>(<span style="color: white;">Tarea</span>, <span style="color: white;">id=id</span>)
    <span style="color: white;">tarea.delete</span>()
    <span style="color: red;">return</span> <span style="color: white;">redirect</span>(<span style="color: yellow;">'listar_tareas'</span>)
```

---

## 🎨 Archivos HTML y CSS

### 📁 Crear carpetas:
```cmd
mkdir templates\app_tareas
mkdir static\css
```

---

### 📄 static/css/styles.css
```css
/* <span style="color: green;">Estilos suaves y limpios</span> */
<span style="color: white;">body</span> {
    <span style="color: white;">font-family</span>: <span style="color: yellow;">'Segoe UI', Tahoma, Geneva, Verdana, sans-serif</span>;
    <span style="color: white;">background-color</span>: <span style="color: yellow;">#f0f8ff</span>; <span style="color: green;">/* Azul muy claro */</span>
    <span style="color: white;">color</span>: <span style="color: yellow;">#2c3e50</span>;
    <span style="color: white;">margin</span>: <span style="color: yellow;">0</span>;
    <span style="color: white;">padding</span>: <span style="color: yellow;">20px</span>;
}

<span style="color: white;">.container</span> {
    <span style="color: white;">max-width</span>: <span style="color: yellow;">800px</span>;
    <span style="color: white;">margin</span>: <span style="color: yellow;">0 auto</span>;
    <span style="color: white;">background-color</span>: <span style="color: yellow;">white</span>;
    <span style="color: white;">padding</span>: <span style="color: yellow;">20px</span>;
    <span style="color: white;">border-radius</span>: <span style="color: yellow;">8px</span>;
    <span style="color: white;">box-shadow</span>: <span style="color: yellow;">0 2px 10px rgba(0,0,0,0.1)</span>;
}

<span style="color: white;">h1</span> {
    <span style="color: white;">text-align</span>: <span style="color: yellow;">center</span>;
    <span style="color: white;">color</span>: <span style="color: yellow;">#3498db</span>;
}

<span style="color: white;">table</span> {
    <span style="color: white;">width</span>: <span style="color: yellow;">100%</span>;
    <span style="color: white;">border-collapse</span>: <span style="color: yellow;">collapse</span>;
    <span style="color: white;">margin</span>: <span style="color: yellow;">20px 0</span>;
}

<span style="color: white;">th, td</span> {
    <span style="color: white;">padding</span>: <span style="color: yellow;">12px</span>;
    <span style="color: white;">text-align</span>: <span style="color: yellow;">left</span>;
    <span style="color: white;">border-bottom</span>: <span style="color: yellow;">1px solid #ddd</span>;
}

<span style="color: white;">th</span> {
    <span style="color: white;">background-color</span>: <span style="color: yellow;">#ecf0f1</span>;
}

<span style="color: white;">.btn</span> {
    <span style="color: white;">padding</span>: <span style="color: yellow;">8px 12px</span>;
    <span style="color: white;">margin</span>: <span style="color: yellow;">2px</span>;
    <span style="color: white;">border</span>: <span style="color: yellow;">none</span>;
    <span style="color: white;">border-radius</span>: <span style="color: yellow;">4px</span>;
    <span style="color: white;">cursor</span>: <span style="color: yellow;">pointer</span>;
    <span style="color: white;">text-decoration</span>: <span style="color: yellow;">none</span>;
    <span style="color: white;">font-size</span>: <span style="color: yellow;">14px</span>;
}

<span style="color: white;">.btn-crear</span> {
    <span style="color: white;">background-color</span>: <span style="color: yellow;">#2ecc71</span>;
    <span style="color: white;">color</span>: <span style="color: yellow;">white</span>;
}

<span style="color: white;">.btn-editar</span> {
    <span style="color: white;">background-color</span>: <span style="color: yellow;">#3498db</span>;
    <span style="color: white;">color</span>: <span style="color: yellow;">white</span>;
}

<span style="color: white;">.btn-borrar</span> {
    <span style="color: white;">background-color</span>: <span style="color: yellow;">#e74c3c</span>;
    <span style="color: white;">color</span>: <span style="color: yellow;">white</span>;
}

<span style="color: white;">form</span> {
    <span style="color: white;">margin</span>: <span style="color: yellow;">15px 0</span>;
}

<span style="color: white;">input[type="text"]</span> {
    <span style="color: white;">width</span>: <span style="color: yellow;">70%</span>;
    <span style="color: white;">padding</span>: <span style="color: yellow;">10px</span>;
    <span style="color: white;">border</span>: <span style="color: yellow;">1px solid #ccc</span>;
    <span style="color: white;">border-radius</span>: <span style="color: yellow;">4px</span>;
}

<span style="color: white;">.checkbox</span> {
    <span style="color: white;">margin-left</span>: <span style="color: yellow;">10px</span>;
}
```

---

### 📄 templates/app_tareas/listar_tareas.html
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

### 📄 templates/app_tareas/crear_tarea.html
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

### 📄 templates/app_tareas/actualizar_tarea.html
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

### 📌 Resumen de funcionalidades:
- Listar tareas
- Crear nuevas tareas
- Editar tareas existentes
- Marcar como completadas
- Borrar tareas

### 🎨 Estilo:
- Colores suaves
- Sin Bootstrap
- CSS personalizado
- HTML semántico y completo

---

> ✅ Este material es ideal para estudiantes que inician con Django.  
> ✅ Todos los pasos están detallados.  
> ✅ Código completo y funcional.

¡Éxito en tu aprendizaje! 🚀
```
