¡Entendido! Aquí tienes el proyecto completo, incluyendo los pasos iniciales que me proporcionaste y lo que faltaba para completarlo, todo en un solo bloque.

---

¡Excelente! Vamos a crear este proyecto de Abarrotes con Django paso a paso.

Aquí tienes la guía completa, incluyendo la estructura de carpetas y archivos, y el código necesario.

### Estructura de Carpetas y Archivos

```
UIII_Abarrotes_0633/
├── .venv/
├── backend_Abarrotes/
│   ├── backend_Abarrotes/
│   │   ├── __init__.py
│   │   ├── asgi.py
│   │   ├── settings.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   ├── app_Abarrotes/
│   │   ├── migrations/
│   │   │   └── __init__.py
│   │   ├── templates/
│   │   │   ├── base.html
│   │   │   ├── header.html (No se utiliza directamente, pero se puede mantener si se desea separar el header de la base.html)
│   │   │   ├── navbar.html
│   │   │   ├── footer.html
│   │   │   ├── inicio.html
│   │   │   └── empleado/
│   │   │       ├── agregar_empleado.html
│   │   │       ├── ver_empleados.html
│   │   │       ├── actualizar_empleado.html
│   │   │       └── borrar_empleado.html
│   │   ├── __init__.py
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── models.py
│   │   ├── tests.py
│   │   ├── views.py
│   │   └── urls.py
│   └── manage.py
```

### Procedimientos Detallados:

**1. Procedimiento para crear carpeta del Proyecto: `UIII_Abarrotes_0633`**

Abre tu terminal (o cmd en Windows) y ejecuta:

```bash
mkdir UIII_Abarrotes_0633
cd UIII_Abarrotes_0633
```

**2. Procedimiento para abrir VS Code sobre la carpeta `UIII_Abarrotes_0633`**

Desde la terminal, estando dentro de `UIII_Abarrotes_0633`:

```bash
code .
```

Esto abrirá VS Code en la raíz de tu proyecto.

**3. Procedimiento para abrir terminal en VS Code**

Dentro de VS Code, ve a `Terminal -> New Terminal` (o usa el atajo `Ctrl + Shift + Ñ` o `Cmd + Shift + P` y busca "New Terminal").

**4. Procedimiento para crear carpeta entorno virtual ".venv" desde terminal de VS Code**

En la terminal de VS Code:

```bash
python -m venv .venv
```

**5. Procedimiento para activar el entorno virtual.**

En la terminal de VS Code:

**Windows:**
```bash
.venv\Scripts\activate
```
**macOS/Linux:**
```bash
source .venv/bin/activate
```
Verás `(.venv)` al inicio de tu prompt si se activó correctamente.

**6. Procedimiento para activar intérprete de python.**

VS Code generalmente detecta y sugiere el intérprete del entorno virtual activo. Si no lo hace, puedes seleccionarlo manualmente:

*   Presiona `Ctrl + Shift + P` (o `Cmd + Shift + P` en Mac).
*   Escribe `Python: Select Interpreter`.
*   Selecciona la opción que apunte a `.\.venv\Scripts\python.exe` (Windows) o `./.venv/bin/python` (macOS/Linux).

**7. Procedimiento para instalar Django**

Con el entorno virtual activado:

```bash
pip install Django
```

**8. Procedimiento para crear proyecto `backend_Abarrotes` sin duplicar carpeta.**

```bash
django-admin startproject backend_Abarrotes .
```
El `.` es crucial para que no cree una carpeta anidada.

**9. Procedimiento para ejecutar servidor en el puerto 8036**

```bash
python manage.py runserver 8036
```

**10. Procedimiento para copiar y pegar el link en el navegador.**

Una vez que ejecutes el servidor, en la terminal aparecerá una línea como esta: `Starting development server at http://127.0.0.1:8036/` Copia `http://127.0.0.1:8036/` y pégalo en tu navegador.

**11. Procedimiento para crear aplicación `app_Abarrotes`**

Detén el servidor (`Ctrl+C`) si está corriendo y luego ejecuta:

```bash
python manage.py startapp app_Abarrotes
```

**12. Aquí el modelo `models.py`**

Edita el archivo `app_Abarrotes/models.py` con el siguiente contenido:

```python
# app_Abarrotes/models.py
from django.db import models

==========================================
MODELO: EMPLEADO
==========================================
class Empleado(models.Model):
    nombre = models.CharField(max_length=255)
    puesto = models.CharField(max_length=100)
    salario_hora = models.DecimalField(max_digits=8, decimal_places=2)
    fecha_contratacion = models.DateField()
    horas_semanales = models.PositiveIntegerField()
    departamento = models.CharField(max_length=100)

    def __str__(self):
        return f"{self.nombre} - {self.puesto}"

==========================================
MODELO: CLIENTE
==========================================
class Cliente(models.Model):
    nombre = models.CharField(max_length=255)
    telefono = models.CharField(max_length=20)
    correo = models.CharField(max_length=255)
    fecha_ultima_compra = models.DateField()
    direccion = models.CharField(max_length=255)
    id_empleado = models.ForeignKey(Empleado, on_delete=models.CASCADE, related_name="clientes")

    def __str__(self):
        return self.nombre

==========================================
MODELO: PRODUCTO
==========================================
class Producto(models.Model):
    nombre = models.CharField(max_length=150)
    descripcion = models.TextField(blank=True, null=True)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField()
    categoria = models.CharField(max_length=50)
    fecha_ingreso = models.DateField()
    activo = models.BooleanField(default=True)

    def __str__(self):
        return self.nombre

==========================================
MODELO: VENTA
==========================================
class Venta(models.Model):
    fecha = models.DateField()
    total = models.DecimalField(max_digits=10, decimal_places=2)
    productos_vendidos = models.TextField() # Esto podría ser un ManyToManyField con Producto en una aplicación más compleja.
    id_empleado = models.ForeignKey(Empleado, on_delete=models.CASCADE, related_name="ventas")
    id_cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE, related_name="ventas")
    id_producto = models.ForeignKey(Producto, on_delete=models.CASCADE, related_name="ventas") # Esto debería ser un ManyToManyField en una app real.

    def __str__(self):
        return f"Venta {self.id} - {self.fecha}"
```

**12.5 Procedimiento para realizar las migraciones (`makemigrations` y `migrate`).**

```bash
python manage.py makemigrations app_Abarrotes
python manage.py migrate
```

**13. Primero trabajamos con el MODELO: EMPLEADO**

**14. En `views` de `app_Abarrotes` crear las funciones con sus códigos correspondientes (`inicio_abarrotes`, `agregar_empleado`, `actualizar_empleado`, `borrar_empleado`)**

Edita el archivo `app_Abarrotes/views.py`:

```python
# app_Abarrotes/views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Empleado
from django.db import IntegrityError # Importar para manejar errores de integridad

# Vista de Inicio
def inicio_abarrotes(request):
    return render(request, 'inicio.html')

# Funciones CRUD para EMPLEADO
def agregar_empleado(request):
    if request.method == 'POST':
        nombre = request.POST.get('nombre')
        puesto = request.POST.get('puesto')
        salario_hora = request.POST.get('salario_hora')
        fecha_contratacion = request.POST.get('fecha_contratacion')
        horas_semanales = request.POST.get('horas_semanales')
        departamento = request.POST.get('departamento')

        # Simple validación (sin forms.py, asumimos entrada válida)
        try:
            empleado = Empleado.objects.create(
                nombre=nombre,
                puesto=puesto,
                salario_hora=float(salario_hora),
                fecha_contratacion=fecha_contratacion,
                horas_semanales=int(horas_semanales),
                departamento=departamento
            )
            return redirect('ver_empleados') # Redirige a la lista de empleados
        except ValueError:
            # Manejo básico de errores si los datos no son del tipo esperado
            return render(request, 'empleado/agregar_empleado.html', {'error': 'Datos inválidos. Verifica los campos numéricos y de fecha.'})
        except IntegrityError:
            return render(request, 'empleado/agregar_empleado.html', {'error': 'Error al guardar el empleado. Posiblemente un dato duplicado o incorrecto.'})
    return render(request, 'empleado/agregar_empleado.html')

def ver_empleados(request):
    empleados = Empleado.objects.all()
    return render(request, 'empleado/ver_empleados.html', {'empleados': empleados})

def actualizar_empleado(request, id_empleado):
    empleado = get_object_or_404(Empleado, id=id_empleado)
    if request.method == 'POST':
        empleado.nombre = request.POST.get('nombre')
        empleado.puesto = request.POST.get('puesto')
        empleado.salario_hora = float(request.POST.get('salario_hora'))
        empleado.fecha_contratacion = request.POST.get('fecha_contratacion')
        empleado.horas_semanales = int(request.POST.get('horas_semanales'))
        empleado.departamento = request.POST.get('departamento')
        try:
            empleado.save()
            return redirect('ver_empleados')
        except ValueError:
             return render(request, 'empleado/actualizar_empleado.html', {'empleado': empleado, 'error': 'Datos inválidos. Verifica los campos numéricos y de fecha.'})
        except IntegrityError:
            return render(request, 'empleado/actualizar_empleado.html', {'empleado': empleado, 'error': 'Error al guardar el empleado. Posiblemente un dato duplicado o incorrecto.'})
    return render(request, 'empleado/actualizar_empleado.html', {'empleado': empleado})

def borrar_empleado(request, id_empleado):
    empleado = get_object_or_404(Empleado, id=id_empleado)
    if request.method == 'POST':
        empleado.delete()
        return redirect('ver_empleados')
    return render(request, 'empleado/borrar_empleado.html', {'empleado': empleado})
```

**15. Crear la carpeta "templates" dentro de "app_Abarrotes".**

Ya está en la estructura de archivos que te di, asegúrate de que exista.

**16. En la carpeta `templates` crear los archivos html (`base.html`, `header.html`, `navbar.html`, `footer.html`, `inicio.html`).**

Y la subcarpeta `empleado` dentro de `templates` con sus archivos.

**17. En el archivo `base.html` agregar bootstrap para css y js.**

Crea o edita `app_Abarrotes/templates/base.html`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Abarrotes UIII_0633</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <!-- Font Awesome para iconos -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
    <style>
        body {
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            background-color: #f8f9fa; /* Color de fondo suave */
        }
        .content {
            flex: 1;
            padding-bottom: 70px; /* Espacio para el footer fijo */
        }
        .footer {
            position: fixed;
            bottom: 0;
            width: 100%;
            height: 60px; /* Altura del footer */
            background-color: #343a40; /* Color oscuro para el footer */
            color: white;
            line-height: 60px; /* Centrar texto verticalmente */
        }
        .navbar-custom {
            background-color: #66bb6a; /* Un verde suave */
        }
        .navbar-custom .navbar-brand, .navbar-custom .nav-link {
            color: white;
        }
        .navbar-custom .nav-link:hover {
            color: #dcedc8; /* Un verde más claro al pasar el mouse */
        }
        .btn-primary-custom {
            background-color: #66bb6a;
            border-color: #66bb6a;
            color: white;
        }
        .btn-primary-custom:hover {
            background-color: #4CAF50;
            border-color: #4CAF50;
        }
        .btn-success-custom {
            background-color: #4CAF50;
            border-color: #4CAF50;
        }
        .btn-success-custom:hover {
            background-color: #388E3C;
            border-color: #388E3C;
        }
    </style>
</head>
<body>
    {% include 'navbar.html' %}

    <div class="container mt-4 content">
        {% block content %}
        {% endblock %}
    </div>

    {% include 'footer.html' %}

    <!-- Bootstrap JS y Popper.js -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
```

**18. En el archivo `navbar.html` incluir las opciones...**

Crea o edita `app_Abarrotes/templates/navbar.html`:

```html
<nav class="navbar navbar-expand-lg navbar-custom shadow-sm">
    <div class="container-fluid">
        <a class="navbar-brand" href="{% url 'inicio_abarrotes' %}">
            <i class="fas fa-store-alt me-2"></i>Sistema de Administración Abarrotes
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item">
                    <a class="nav-link" aria-current="page" href="{% url 'inicio_abarrotes' %}">
                        <i class="fas fa-home me-1"></i>Inicio
                    </a>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownEmpleados" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-users me-1"></i>Empleados
                    </a>
                    <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="navbarDropdownEmpleados">
                        <li><a class="dropdown-item" href="{% url 'agregar_empleado' %}">Agregar Empleado</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_empleados' %}">Ver Empleados</a></li>
                        <!-- Los botones de actualizar y borrar se manejan desde la tabla de ver_empleados -->
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownClientes" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-user-friends me-1"></i>Clientes
                    </a>
                    <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="navbarDropdownClientes">
                        <li><a class="dropdown-item" href="#">Agregar Clientes</a></li>
                        <li><a class="dropdown-item" href="#">Ver Clientes</a></li>
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownProductos" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-box-open me-1"></i>Productos
                    </a>
                    <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="navbarDropdownProductos">
                        <li><a class="dropdown-item" href="#">Agregar Productos</a></li>
                        <li><a class="dropdown-item" href="#">Ver Productos</a></li>
                    </ul>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownVentas" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="fas fa-cash-register me-1"></i>Ventas
                    </a>
                    <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="navbarDropdownVentas">
                        <li><a class="dropdown-item" href="#">Agregar Ventas</a></li>
                        <li><a class="dropdown-item" href="#">Ver Ventas</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

**19. En el archivo `footer.html` incluir derechos de autor, fecha del sistema y "Creado por Ing. Eliseo Nava, Cbtis 128" y mantenerla fija al final de la página.**

Crea o edita `app_Abarrotes/templates/footer.html`:

```html
<footer class="footer bg-dark text-white text-center py-3">
    <div class="container">
        <span class="text-muted">&copy; 2024 Derechos Reservados. | Fecha del Sistema: <span id="currentDate"></span> | Creado por Ing. Eliseo Nava, Cbtis 128</span>
    </div>
    <script>
        document.getElementById('currentDate').textContent = new Date().toLocaleDateString();
    </script>
</footer>
```

**20. En el archivo `inicio.html` se usa para colocar información del sistema más una imagen tomada desde la red sobre abarrotes.**

Crea o edita `app_Abarrotes/templates/inicio.html`:

```html
{% extends 'base.html' %}

{% block content %}
<div class="p-5 mb-4 bg-light rounded-3 text-center">
    <div class="container-fluid py-5">
        <h1 class="display-5 fw-bold text-success">Bienvenido al Sistema de Administración de Abarrotes</h1>
        <p class="col-md-8 fs-4 mx-auto">Tu solución completa para gestionar empleados, clientes, productos y ventas de tu negocio de abarrotes de manera eficiente.</p>
        <hr class="my-4">
        <p>Utiliza el menú de navegación para acceder a las diferentes secciones del sistema.</p>
    </div>
</div>

<div class="row align-items-md-stretch">
    <div class="col-md-6">
        <div class="h-100 p-5 bg-light border rounded-3 text-center">
            <h2>Gestión Completa</h2>
            <p>Desde el control de inventario hasta el seguimiento de ventas y empleados, nuestro sistema te proporciona las herramientas para una administración efectiva.</p>
            <a class="btn btn-primary-custom btn-lg" href="{% url 'ver_empleados' %}" type="button">Ver Empleados</a>
        </div>
    </div>
    <div class="col-md-6">
        <div class="h-100 p-5 border rounded-3 bg-white text-center">
            <h2>Visualización del Negocio</h2>
            <p>Una vista rápida de las operaciones diarias de tu abarrotes para tomar decisiones informadas.</p>
            <img src="https://images.unsplash.com/photo-1542838132-92c2299863be?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D" class="img-fluid rounded shadow-sm" alt="Tienda de Abarrotes">
        </div>
    </div>
</div>
{% endblock %}
```

### **A continuación, la parte que faltaba para completar el CRUD de Empleados y la configuración final:**

**21. Configuración de `settings.py` para la aplicación `app_Abarrotes` y `templates`**

Edita el archivo `backend_Abarrotes/backend_Abarrotes/settings.py`.

Asegúrate de agregar `app_Abarrotes` a `INSTALLED_APPS`:

```python
# backend_Abarrotes/backend_Abarrotes/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_Abarrotes', # ¡Asegúrate de que tu aplicación esté aquí!
]
```

Y configura `TEMPLATES` para que Django sepa dónde buscar tus archivos HTML. Es importante que `DIRS` apunte a la carpeta `templates` dentro de tu app:

```python
# backend_Abarrotes/backend_Abarrotes/settings.py

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'app_Abarrotes' / 'templates'], # Agrega esta línea para que Django busque templates en la carpeta de la app
        'APP_DIRS': True, # Esto es importante si tienes templates específicos de la app
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
```

**22. En el archivo `urls.py` del proyecto (`backend_Abarrotes`)**

Edita `backend_Abarrotes/backend_Abarrotes/urls.py` para incluir las URLs de tu aplicación `app_Abarrotes`:

```python
# backend_Abarrotes/backend_Abarrotes/urls.py
from django.contrib import admin
from django.urls import path, include # Importa include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Abarrotes.urls')), # Incluye las URLs de tu app
]
```

**23. Crear `urls.py` para la aplicación `app_Abarrotes`**

Crea o edita el archivo `app_Abarrotes/urls.py`:

```python
# app_Abarrotes/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_abarrotes, name='inicio_abarrotes'),
    path('empleados/', views.ver_empleados, name='ver_empleados'),
    path('empleados/agregar/', views.agregar_empleado, name='agregar_empleado'),
    path('empleados/actualizar/<int:id_empleado>/', views.actualizar_empleado, name='actualizar_empleado'),
    path('empleados/borrar/<int:id_empleado>/', views.borrar_empleado, name='borrar_empleado'),
]
```

**24. Crear los archivos HTML para EMPLEADO (continuación)**

**`app_Abarrotes/templates/empleado/agregar_empleado.html`**

```html
{% extends 'base.html' %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-8">
        <div class="card shadow-sm">
            <div class="card-header bg-success text-white">
                <h2 class="mb-0"><i class="fas fa-user-plus me-2"></i>Agregar Nuevo Empleado</h2>
            </div>
            <div class="card-body">
                {% if error %}
                    <div class="alert alert-danger" role="alert">
                        {{ error }}
                    </div>
                {% endif %}
                <form method="post">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre Completo</label>
                        <input type="text" class="form-control" id="nombre" name="nombre" required>
                    </div>
                    <div class="mb-3">
                        <label for="puesto" class="form-label">Puesto</label>
                        <input type="text" class="form-control" id="puesto" name="puesto" required>
                    </div>
                    <div class="mb-3">
                        <label for="salario_hora" class="form-label">Salario por Hora</label>
                        <input type="number" step="0.01" class="form-control" id="salario_hora" name="salario_hora" required>
                    </div>
                    <div class="mb-3">
                        <label for="fecha_contratacion" class="form-label">Fecha de Contratación</label>
                        <input type="date" class="form-control" id="fecha_contratacion" name="fecha_contratacion" required>
                    </div>
                    <div class="mb-3">
                        <label for="horas_semanales" class="form-label">Horas Semanales</label>
                        <input type="number" class="form-control" id="horas_semanales" name="horas_semanales" required>
                    </div>
                    <div class="mb-3">
                        <label for="departamento" class="form-label">Departamento</label>
                        <input type="text" class="form-control" id="departamento" name="departamento" required>
                    </div>
                    <button type="submit" class="btn btn-success-custom"><i class="fas fa-save me-2"></i>Guardar Empleado</button>
                    <a href="{% url 'ver_empleados' %}" class="btn btn-secondary"><i class="fas fa-arrow-left me-2"></i>Volver a Empleados</a>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

**`app_Abarrotes/templates/empleado/ver_empleados.html`**

```html
{% extends 'base.html' %}

{% block content %}
<div class="row">
    <div class="col-12">
        <div class="card shadow-sm">
            <div class="card-header bg-success text-white d-flex justify-content-between align-items-center">
                <h2 class="mb-0"><i class="fas fa-users me-2"></i>Listado de Empleados</h2>
                <a href="{% url 'agregar_empleado' %}" class="btn btn-light btn-sm"><i class="fas fa-plus me-1"></i>Agregar Empleado</a>
            </div>
            <div class="card-body">
                {% if empleados %}
                <div class="table-responsive">
                    <table class="table table-striped table-hover">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Nombre</th>
                                <th>Puesto</th>
                                <th>Salario/Hora</th>
                                <th>Fecha Contratación</th>
                                <th>Horas Semanales</th>
                                <th>Departamento</th>
                                <th class="text-center">Acciones</th>
                            </tr>
                        </thead>
                        <tbody>
                            {% for empleado in empleados %}
                            <tr>
                                <td>{{ empleado.id }}</td>
                                <td>{{ empleado.nombre }}</td>
                                <td>{{ empleado.puesto }}</td>
                                <td>${{ empleado.salario_hora }}</td>
                                <td>{{ empleado.fecha_contratacion|date:"d/m/Y" }}</td>
                                <td>{{ empleado.horas_semanales }}</td>
                                <td>{{ empleado.departamento }}</td>
                                <td class="text-center">
                                    <a href="{% url 'actualizar_empleado' empleado.id %}" class="btn btn-warning btn-sm me-2" title="Actualizar"><i class="fas fa-edit"></i></a>
                                    <a href="{% url 'borrar_empleado' empleado.id %}" class="btn btn-danger btn-sm" title="Eliminar"><i class="fas fa-trash-alt"></i></a>
                                </td>
                            </tr>
                            {% endfor %}
                        </tbody>
                    </table>
                </div>
                {% else %}
                <div class="alert alert-info" role="alert">
                    No hay empleados registrados en el sistema.
                </div>
                {% endif %}
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

**`app_Abarrotes/templates/empleado/actualizar_empleado.html`**

```html
{% extends 'base.html' %}

{% block content %}
<div class="row justify-content-center">
    <div class="col-md-8">
        <div class="card shadow-sm">
            <div class="card-header bg-warning text-white">
                <h2 class="mb-0"><i class="fas fa-user-edit me-2"></i>Actualizar Empleado: {{ empleado.nombre }}</h2>
            </div>
            <div class="card-body">
                {% if error %}
                    <div class="alert alert-danger" role="alert">
                        {{ error }}
                    </div>
                {% endif %}
                <form method="post">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre Completo</label>
                        <input type="text" class="form-control" id="nombre" name="nombre" value="{{ empleado.nombre }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="puesto" class="form-label">Puesto</label>
                        <input type="text" class="form-control" id="puesto" name="puesto" value="{{ empleado.puesto }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="salario_hora" class="form-label">Salario por Hora</label>
                        <input type="number" step="0.01" class="form-control" id="salario_hora" name="salario_hora" value="{{ empleado.salario_hora }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="fecha_contratacion" class="form-label">Fecha de Contratación</label>
                        <input type="date" class="form-control" id="fecha_contratacion" name="fecha_contratacion" value="{{ empleado.fecha_contratacion|date:'Y-m-d' }}" required>
                    </div>
                    <div class="mb-3">
                        <label for="horas_semanales" class="form-label">Horas Semanales</label>
                        <input type="number" class="form-control" id="horas_semanales" name="horas_semanales" value="{{ empleado.horas_semanales }}" required>
                    </div>
                    <div class="mb-3">
                        <label



