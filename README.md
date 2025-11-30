# Documentación del Proyecto: Gestor de Tareas

Gestor de Tareas es una aplicación web desarrollada en Django que permite Cada usuario puede registrar, listar, editar y eliminar únicamente sus propias tareas, con autenticación robusta utilizando el sistema de usuarios de Django

Utiliazando la estructura de Vistas Basadas en Funciones de Django. 
La interfaz utiliza **Bootstrap 5** para un diseño responsivo y amigable.

---

## Características
- Registro, inicio y cierre de sesión de usuarios.
- Gestión de tareas privadas por usuario:
  - Listar tareas.
  - Agregar nuevas tareas.
  - Editar tareas existentes.
  - Eliminar tareas.
- Vista de detalle de cada tarea.
- Interfaz responsiva con Bootstrap.

---

## Tecnologías
- Python 3.x
- Django 5.2.x
- Base de Datos: MySQL (Configurado con adaptador PyMySQL)
- Frontend: Bootstrap 5

---

# Parte 1: Configuración Inicial
## Creación del Proyecto y la Aplicación
El desarrollo comenzó con la inicialización de un nuevo proyecto Django denominado gestor_tareas. Para ello, se utilizó el comando django-admin startproject gestor_tareas, que genera la estructura de directorios y ficheros base necesaria para cualquier proyecto Django.

Posteriormente, dentro del directorio del proyecto, se procedió a crear la aplicación principal, tareas, mediante el comando python manage.py startapp tareas. Esta aplicación encapsula toda la lógica de negocio relacionada con la gestión de tareas, incluyendo modelos, vistas, formularios y plantillas.

## Configuración del Proyecto
Para que el proyecto Django reconozca y pueda utilizar la aplicación "tareas", se registro en la configuración settings.py en la lista INSTALLED_APPS.

## Configuración de URLs

 gestor_tareas/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('tareas.urls')),
]
Dentro de la aplicación tareas, se creó un fichero urls.py para definir las rutas específicas de la misma, asociando cada URL con su vista correspondiente.

## Parte 2: Vistas y Plantillas

Vistas para el CRUD de Tareas
Se implementaron las siguientes vistas basadas en funciones para manejar el ciclo de vida de las tareas (CRUD: Create, Read, Update, Delete):

lista_tareas(request): Protegida con el decorador @login_required, esta vista recupera y muestra únicamente las tareas pertenecientes al usuario autenticado.
tarea_detail(request, tarea_id): Permite visualizar los detalles de una tarea específica, asegurando que solo el usuario propietario pueda acceder a ella.
crear_tarea(request): Gestiona la creación de nuevas tareas a través de un formulario de Django. Asocia automáticamente la nueva tarea con el usuario que ha iniciado sesión.
eliminar_tarea(request, tarea_id): Permite a un usuario eliminar una de sus propias tareas.
completar_tarea(request, tarea_id): Marca una tarea como completada.

Formularios de Django
Para la creación de tareas, se utilizó la clase forms.ModelForm de Django, que facilita la creación de formularios a partir de un modelo existente. El formulario TareaForm se definió en el fichero tareas/forms.py, especificando los campos titulo y descripcion.

Plantillas y Diseño con Bootstrap
La interfaz de usuario se ha diseñado utilizando Bootstrap 5, lo que garantiza una apariencia moderna y responsiva. Se ha seguido el principio DRY (Don't Repeat Yourself) mediante el uso de un sistema de plantillas de Django:

- base.html: Plantilla principal que define la estructura común de la página, incluyendo la barra de navegación y el pie de página.
- navbar.html y footer.html: Componentes reutilizables incluidos en base.html.
- lista_tareas.html: Muestra la lista de tareas del usuario.
- tarea_detail.html: Presenta los detalles de una tarea individual.
- tareas.html: Contiene el formulario para agregar nuevas tareas.

## Parte 3: Autenticación y Seguridad
Sistema de Autenticación de Django
Se ha implementado un sistema completo de autenticación de usuarios utilizando el módulo django.contrib.auth. Se crearon las siguientes vistas para gestionar el ciclo de vida del usuario:

registro_usuario(request): Permite a los nuevos usuarios crear una cuenta. Utiliza el formulario UserCreationForm de Django.
iniciar_sesion(request): Gestiona el inicio de sesión de los usuarios existentes mediante el AuthenticationForm.
cerrar_sesion(request): Cierra la sesión del usuario actual.

Protección de Vistas
La seguridad de la aplicación se ha reforzado para garantizar que solo los usuarios autenticados puedan acceder a las funcionalidades de gestión de tareas. Esto se ha logrado mediante:

Decorador @login_required: Se ha aplicado a todas las vistas de tareas para redirigir a los usuarios no autenticados a la página de inicio de sesión.
Filtrado por Usuario: En las vistas lista_tareas, tarea_detail, eliminar_tarea y completar_tarea, las consultas a la base de datos se filtran por el usuario autenticado (request.user). Esto previene que un usuario pueda ver o modificar las tareas de otro.

## Parte 4: Despliegue y Pruebas
Pruebas de Funcionalidad
Se han realizado pruebas para verificar el correcto funcionamiento de todas las características implementadas:

Autenticación: Se ha comprobado que el registro, inicio de sesión y cierre de sesión funcionan como se esperaba.
Gestión de Tareas: Se ha verificado que los usuarios pueden crear, ver, completar y eliminar sus propias tareas.
Seguridad: Se ha confirmado que un usuario no puede acceder a las tareas de otro a través de la manipulación de URLs.

## Configuración para Producción
Para un eventual despliegue en un entorno de producción, se han ajustado las siguientes configuraciones en settings.py:

DEBUG: Se ha establecido en False para desactivar los mensajes de depuración detallados.
ALLOWED_HOSTS: Se ha configurado para permitir únicamente las peticiones desde los dominios autorizados.
SECRET_KEY: Se ha generado una clave secreta única y se ha mantenido fuera del control de versiones en un entorno de producción real.












# Cómo Ejecutar el Proyecto
Para ejecutar el proyecto en un entorno de desarrollo local, siga los siguientes pasos:

1. **Crear un entorno virtual**

```bash
python -m venv myenv
```

- Activar el entorno virtual:

```bash
# Windows
myenv\Scripts\activate
```

2. **Instalar dependencias**

```bash
pip install -r requirements.txt
```

```bash
pip install mysql
```

4. **Migrar la base de datos (despues de haber modificado los modelos)**

```bash
python manage.py makemigrations
python manage.py migrate
```

5. **Crear superusuario (opcional)**

```bash
python manage.py createsuperuser
```

6. **Ejecutar el servidor de desarrollo**

```bash
python manage.py runserver
```

- Acceder a la aplicación en: http://127.0.0.1:8000/

---

## Rutas URL

- Página principal: `/`
- Registro de usuario: `/registro/`
- Inicio de sesión: `/login/`
- Lista de tareas: `/lista/`

### Gestión de tareas
- Agregar tarea: `/agregar/`
- Editar tarea: `/tarea/<id>/`
- Eliminar tarea: `/eliminar/<id>/`

> Nota: Solo los usuarios autenticados pueden acceder a su lista y gestionar sus tareas.

---

## Estructura del Proyecto

```bash
gestor_tareas/
├─ gestor_tareas/         # Proyecto Django
│  ├─ settings.py
│  ├─ urls.py
│  └─ wsgi.py
├─ tareas/                # Aplicación de tareas
│  ├─ models.py
│  ├─ views.py
│  ├─ forms.py
│  ├─ urls.py
│  └─ templates/
│      ├─ base.html
│      ├─ index.html
│      ├─ lista_tareas.html
│      ├─ tarea_detail.html
│      ├─ footer.html
│      ├─ tareas.html
│      ├─ login.html
│      ├─ logout.html
│      ├─ footer.html
│      ├─ navbar.html
│      └─ registro.html
├─ manage.py
├─ requirements.txt
├─ .gitignore
└─ db.sqlite3
```

---

## Configuración de Producción (Educativa)

Para simular un entorno de producción en máquina local, en `settings.py`:

```bash
DEBUG = False
ALLOWED_HOSTS = ['localhost', '127.0.0.1']
```
