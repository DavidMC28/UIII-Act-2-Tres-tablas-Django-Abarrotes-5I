1. Crear estructura de carpetas y archivos
En la terminal de VS Code ejecutar:

bash
# Crear carpeta principal
mkdir UIII_Abarrotes_0633
cd UIII_Abarrotes_0633

# Crear entorno virtual
python -m venv .venv

# Activar entorno virtual (Windows)
.venv\Scripts\activate

# Instalar Django
pip install django

# Crear proyecto Django
django-admin startproject backend_Abarrotes .

# Crear aplicación
python manage.py startapp app_Abarrotes
2. Configurar settings.py
En backend_Abarrotes/settings.py agregar:

python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_Abarrotes',  # Agregar esta línea
]
3. Crear modelos
En app_Abarrotes/models.py pegar el código completo del modelo proporcionado

4. Configurar URLs principales
En backend_Abarrotes/urls.py:

python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Abarrotes.urls')),
]
5. Crear archivo urls.py en la app
Crear app_Abarrotes/urls.py con:

python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_abarrotes, name='inicio'),
    path('empleados/', views.ver_empleados, name='ver_empleados'),
    path('empleados/agregar/', views.agregar_empleado, name='agregar_empleado'),
    path('empleados/actualizar/<int:id>/', views.actualizar_empleado, name='actualizar_empleado'),
    path('empleados/realizar_actualizacion/<int:id>/', views.realizar_actualizacion_empleado, name='realizar_actualizacion_empleado'),
    path('empleados/borrar/<int:id>/', views.borrar_empleado, name='borrar_empleado'),
]
6. Crear views.py
En app_Abarrotes/views.py:

python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Empleado

def inicio_abarrotes(request):
    return render(request, 'inicio.html')

def ver_empleados(request):
    empleados = Empleado.objects.all()
    return render(request, 'empleado/ver_empleados.html', {'empleados': empleados})

def agregar_empleado(request):
    if request.method == 'POST':
        Empleado.objects.create(
            nombre=request.POST['nombre'],
            puesto=request.POST['puesto'],
            salario_hora=request.POST['salario_hora'],
            fecha_contratacion=request.POST['fecha_contratacion'],
            horas_semanales=request.POST['horas_semanales'],
            departamento=request.POST['departamento']
        )
        return redirect('ver_empleados')
    return render(request, 'empleado/agregar_empleado.html')

def actualizar_empleado(request, id):
    empleado = get_object_or_404(Empleado, id=id)
    return render(request, 'empleado/actualizar_empleado.html', {'empleado': empleado})

def realizar_actualizacion_empleado(request, id):
    if request.method == 'POST':
        empleado = get_object_or_404(Empleado, id=id)
        empleado.nombre = request.POST['nombre']
        empleado.puesto = request.POST['puesto']
        empleado.salario_hora = request.POST['salario_hora']
        empleado.fecha_contratacion = request.POST['fecha_contratacion']
        empleado.horas_semanales = request.POST['horas_semanales']
        empleado.departamento = request.POST['departamento']
        empleado.save()
        return redirect('ver_empleados')
    return redirect('ver_empleados')

def borrar_empleado(request, id):
    empleado = get_object_or_404(Empleado, id=id)
    if request.method == 'POST':
        empleado.delete()
        return redirect('ver_empleados')
    return render(request, 'empleado/borrar_empleado.html', {'empleado': empleado})
7. Crear estructura de templates
Crear estas carpetas y archivos:

text
app_Abarrotes/
└── templates/
    ├── base.html
    ├── header.html
    ├── navbar.html
    ├── footer.html
    ├── inicio.html
    └── empleado/
        ├── agregar_empleado.html
        ├── ver_empleados.html
        ├── actualizar_empleado.html
        └── borrar_empleado.html
8. Realizar migraciones
bash
python manage.py makemigrations
python manage.py migrate
9. Ejecutar servidor
bash
python manage.py runserver 8036
10. Acceder al proyecto
Abrir navegador en: http://localhost:8036

Sigue estos pasos en orden y tendrás tu proyecto funcionando completamente.


