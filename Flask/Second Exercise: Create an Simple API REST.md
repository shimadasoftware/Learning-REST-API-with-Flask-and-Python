# Segundo Ejercicio: Crear una simple API Rest 

Los temas a ver son:

- Tu primera REST API
- Introducción a Docker
- Flask-Smorest para un desarrollo más eficiente
- Almacenar datos en una base de datos SQL con SQLAlchemy
- Relaciones de muchos a muchos con SQLAlchemy
- Autenticación de usuarios con Flask-JWT-Extended
- Migraciones de bases de datos con Alembic y Flask-Migrate
- Curso intensivo de Git
- Implementaciones con Render.com
- Colas de tareas con solicitud y envío de correos electrónicos


## 1. Tu primera REST API 🙋🏻‍♀️

- Introducción a Flask
- Configuración inicial para una aplicación Flask
- Su primer punto final de API REST
- ¿Qué es un JSON?
- Cómo interactuar con su API REST y probarla
- Cómo crear tiendas en nuestra API REST
- Cómo crear artículos en cada tienda
- Cómo conseguir una tienda específica y sus artículos

### Descripción general del proyecto que construiremos

#### Explicación general

Este fragmento describe cómo crear una API REST simple que permite trabajar con tiendas y productos. Se puede:

- Crear tiendas con nombre y lista de productos.

- Agregar productos a una tienda.

- Obtener todas las tiendas y sus productos.

- Obtener una tienda específica y sus productos.

- Obtener solo los productos de una tienda específica.

![image](./img/99.png)

#### Crear tiendas - Create stores

Request:

```
POST /store {"name": "My Store"}
```

Response:

```
{"name": "My Store", "items":[]}
```

Se crea una tienda llamada My Store.

Al principio, la tienda no tiene productos (items: []).

![image](./img/100.png)

#### Crear productos - Create items

Request:

```
POST /store/item {"name": "Chair", "price": 175.50}
```

Response:

```
{"name": "Chair", "price": 175.50}
```

Se crea un producto llamado Chair (silla) con un precio de 175.50.

![image](./img/101.png)

#### Obtener todas las tiendas y sus productos - Retrieve all stories and their items

Request:

```
GET /store
```

Response:

```
{
    "stores": [
        "name": "My Store",
        "items": [
            {
               "name": "Chair",
               "price": 175.50 
            }
        ]
    ]
}
```

Devuelve una lista de tiendas (stores).

En este caso, hay una tienda llamada My Store que contiene un producto llamado Chair con precio 175.50.

![image](./img/102.png)


#### Obtener una tienda específica - Get a particular store

Request:

```
GET /store/My Store/item
```

Response:

```
[
    {
        "name": "Chair",
        "price": 175.50 
    }
]
```

![image](./img/103.png)

#### Obtener solo los productos de una tienda - Get only items in a store

Request:

```
POST /store {"name": "My Store"}
```

Response:

```
{"name": "My Store", "items":[]}
```

Devuelve solo los productos de la tienda My Store, sin información adicional sobre la tienda.

![image](./img/104.png)


### Configuración inicial para una aplicación Flask

[Ver instalación de Flask](./README.md)

### Su primer punto final de API REST

```
flask --app app --debug run
```

![image](./img/105.png)

![image](./img/108.png)

![image](./img/107.png)

### Cómo interactuar con su API REST y probarla

Insomnia Client o Postman.

![image](./img/106.png)

Crear el get all store data:

```
{{base_url}}/store
```

```
from flask import Flask, request

app = Flask(__name__)

stores = [{"name": "My Store", "items": [{"name": "Chair", "price": 175.50}]}]


@app.get("/store")
def get_stores():
    return {"stores": stores}
```

![image](./img/109.png)

![image](./img/110.png)

### Cómo crear tiendas en nuestra API REST

```
{{base_url}}/store
```

```
@app.post("/store")
def create_store():
    request_data = request.get_json()
    new_store = {"name": request_data["name"], "items": []}
    stores.append(new_store)
    return new_store, 201
```

![image](./img/111.png)

![image](./img/112.png)

![image](./img/113.png)

### Cómo crear artículos en cada tienda - Create an item

```
{{base_url}}/store/My Store/item
```

```
@app.post("/store/<string:store_name>/item")
def create_item(store_name):
    request_data = request.get_json()
    for store in stores:
        if store["name"] == store_name:
            new_item = {"name": request_data["name"], "price": request_data["price"]}
            store["items"].append(new_item)
            return new_item, 201
    return {"message": "Store not found"}, 404

```

Función para crear un item:

![image](./img/114.png)

Crear el item table:

![image](./img/115.png)

Ver que tiendas hay:

![image](./img/116.png)

Crear el item laptop:

![image](./img/117.png)

![image](./img/118.png)

### Cómo conseguir una tienda específica y sus artículos

#### Get a particular store - Obtener los datos de una tienda en específico

```
{{base_url}}/store/My Store
```

```
@app.get("/store/<string:store_name>")
def get_store(store_name):
    for store in stores:
        if store["name"] == store_name:
            return store
    return {"message": "Store not found"}, 404
```

Función:

![image](./img/119.png)

Obtener los datos de la tienda My Store:

![image](./img/121.png)

#### Get only items in a store - Obtener solo los items de una tienda

```
{{base_url}}/store/My Store/item
```

```
@app.get("/store/<string:store_name>/item")
def get_only_item_in_store(store_name):
    for store in stores:
        if store["name"] == store_name:
            return {"items": store["items"]}
    return {"message": "Store not found"}, 404
```

Función:

![image](./img/120.png)

Obtener solo los items de la My Store:

![image](./img/122.png)


---


## 2. Introducción a Docker 🙋🏻‍♀️

- ¿Qué son los contenedores e imágenes de Docker?
- Cómo ejecutar una aplicación Flask en un contenedor Docker
- Cómo ejecutar la API REST de Flask con Docker Compose

### ¿Qué son los contenedores e imágenes de Docker?

![image](./img/123.png)

![image](./img/124.png)

![image](./img/125.png)

![image](./img/126.png)

![image](./img/127.png)

![image](./img/128.png)

### Cómo ejecutar una aplicación Flask en un contenedor Docker

![image](./img/124.png)

### Cómo ejecutar la API REST de Flask con Docker Compose

Dockerfile es el archivo que le dice a Docker cómo construir una imagen personalizada que puede ejecutar tu API REST con Flask.

Comandos clave en Dockerfile:

* FROM → Imagen base.

Usa una imagen base de Python (la última versión oficial disponible)..

* EXPOSE → El contenedor escucha en un puerto específico, pero no lo abre al exterior automáticamente.

Indica que el contenedor va a usar el puerto 5000, que es el puerto por defecto donde Flask sirve la aplicación. No abre el puerto por sí solo, solo lo "declara" para quien lo ejecute.

* WORKDIR → Definir la ruta (directorio) de trabajo dentro del contenedor para los comandos posteriores. Si el directorio no existe, Docker lo crea automáticamente.

Establece el directorio de trabajo dentro del contenedor en /app. Todas las instrucciones posteriores se ejecutarán desde ahí. Es como decir: “Entra a esta carpeta dentro del contenedor”.

* RUN → Ejecuta comandos durante la construcción.

Instala el framework Flask dentro del contenedor usando pip. Asegura que Flask esté disponible cuando se ejecute la app.

* COPY → Copia archivos al contenedor.

Copia todo el contenido del directorio actual (donde está el Dockerfile) a la carpeta /app dentro del contenedor. Así el código de tu app se incluye dentro del contenedor.

* CMD → Comando por defecto al iniciar el contenedor.

Define el comando predeterminado que se ejecuta cuando se inicia el contenedor. Aquí le dice a Flask que inicie el servidor y lo haga accesible en cualquier IP (0.0.0.0) dentro del contenedor.

```
FROM python
EXPOSE 5000
WORKDIR /app
RUN pip install flask
COPY . /app
CMD ["flask", "run", "--host", "0.0.0.0"]
```

Construye una imagen tomando como base la ruta donde se esté ubicado y las instrucciones en el Dockerfile. 

- -t rest-apis-flask-python etiqueta la imagen con un nombre específico.
- ./ usa el directorio actual (donde está el Dockerfile) como contexto de construcción.
- build construir la imagen.

```
sudo docker build -t rest-apis-flask-python ./
sudo docker image ls
```

![image](./img/129.png)

Este comando sirve para ejecutar un contenedor Docker en segundo plano, basado en la imagen rest-apis-flask-python:

```
sudo docker run -d -p 5005:5000 rest-apis-flask-python
    sudo docker container ls
```

![image](./img/130.png)

| Parte del comando        | Explicación                                                                                    |
| ------------------------ | ---------------------------------------------------------------------------------------------- |
| `docker run`             | Inicia un nuevo contenedor a partir de una imagen.                                             |
| `-d`                     | Modo "detached": el contenedor se ejecuta en segundo plano (no bloquea la terminal).           |
| `-p 5005:5000`           | Mapea el puerto `5000` del contenedor al puerto `5005` de tu máquina.                          |
| `rest-apis-flask-python` | Nombre de la imagen Docker que quieres ejecutar.                                               |

Puertos:

-p [PUERTO_EXTERNO]:[PUERTO_INTERNO]

| Parte  | Qué es                            | Explicación                                                                                                 |
| ------ | --------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `5000` | **Puerto interno del contenedor** | Es donde **Flask** está corriendo **dentro del contenedor** (por defecto, Flask escucha en el puerto 5000). |
| `5005` | **Puerto de tu máquina (host)**   | Es el puerto por el que tú accedes desde fuera del contenedor (desde tu navegador, Postman, etc.).          |

Analogía simple

Imagina que tienes un departamento (contenedor) con una puerta interna (5000), pero para que la gente pueda entrar desde la calle (tu PC), necesitas abrir una puerta externa (5005).

Entonces:

* Adentro del contenedor, tu aplicación corre en el puerto 5000.

* Pero tú quieres que desde tu navegador se acceda usando el puerto 5005.

Docker hace ese "puente" gracias al parámetro -p.

Visualmente:

[Tu PC:5005] ───▶ [Docker contenedor:5000 (donde está Flask)]

Tú entras por el puerto 5005, y Docker lo redirige al puerto 5000 interno del contenedor donde está Flask.

```
sudo docker stop 9f34edff8145
sudo docker rm 9f34edff8145
sudo docker container ls -a
```

![image](./img/131.png)

![image](./img/132.png)


### Cómo ejecutar la API REST de Flask con Docker Compose

Qué es Docker Compose

Docker recomienda que se debe de manejar un servicio por contenedor, pero en la realidad se necesitan muchos servicios. Por eso se utiliza Docker Composer que es un Orquestador local que permite trabajar con aplicaciones múltiples contenedores, mediante un archivo de configuración.

Docker Compose es una herramienta para definir y ejecutar aplicaciones multi-contenedor usando un archivo YAML. Permite:

✅ Orquestrar múltiples servicios (bases de datos, backends, frontends) en un solo comando.

✅ Gestionar redes, volúmenes y variables de entorno centralizadamente.

✅ Simplificar el desarrollo y despliegue de aplicaciones complejas.

Para crear e iniciar los contenedores usar:

```
sudo docker compose up
```

Nota: detener la ejecución de Visual Studio porque tendría ocupado el puerto e interfiere para esta ejecución con Docker.

![image](./img/133.png)

![image](./img/134.png)

Si se interrumpe el comando en la terminal:

Se detiene el contenedor, pero no se borra. Puedes volver a levantarlo sin reconstruirlo con:

```
sudo docker compose start api
```

o

```
sudo docker compose up api
```

El nombre del servicio es api.

Si se realizó cambios al docker-compose, se puede volver a ejecutar así:

```
sudo docker compose up --build --force-recreate --no-deps
```

| Parte              | Explicación                                                                                                                  |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| `docker compose`   | Usa Docker Compose (herramienta para manejar múltiples contenedores como un solo servicio).                                  |
| `up`               | Levanta (inicia) el contenedor especificado.                                                                                 |
| `--build`          | Fuerza la reconstrucción de la imagen del contenedor antes de levantarlo. Útil si cambiaste código o el Dockerfile.          |
| `--force-recreate` | Obliga a eliminar y **recrear el contenedor**, incluso si no ha cambiado.                                                    |
| `--no-deps`        | **No inicia los contenedores dependientes** (solo levanta el contenedor `api`).                                              |
| `api`              | Es el **nombre del servicio** definido en el archivo `docker-compose.yml` (no es el nombre de la imagen, sino del servicio). |

✅ Has modificado el código fuente o el Dockerfile del servicio api.

✅ Quieres probar solo el servicio api sin arrancar toda la aplicación completa (por ejemplo, sin la base de datos u otros servicios definidos en el docker-compose.yml).

✅ Quieres asegurarte de que el contenedor se reconstruya desde cero (por eso --force-recreate y --build).

✅ Estás depurando problemas y necesitas reiniciar ese contenedor específico.

![image](./img/135.png)


---


## 3. Flask-Smorest para un desarrollo más eficiente 🙋🏻‍♀️

- Mejoras en el modelo de datos para nuestra API
- Mejoras generales en nuestra primera API REST
- Nuevos puntos finales para nuestra primera API REST
- Cómo ejecutar la API en Docker con recarga automática y modo de depuración
- Cómo usar Blueprints y MethodViews en Flask
- Cómo escribir esquemas de malvavisco (Marshmallow) para nuestra API
- Cómo realizar la validación de datos con Marshmallow
- Decorando respuestas con Flask-Smorest

### Mejoras en el modelo de datos para nuestra API

Por ejemplo, para crear un item usamos el nombre de la tienda, la cuál es una práctica poco común. Para ello se suele utilizar un identificador único. Pueden ser:

- Números incrementales.

- Los UUIDs (Universally Unique Identifiers) son identificadores únicos universales. Se usan para asignar identificadores que sean únicos en todo el mundo y en cualquier momento, sin necesidad de una base de datos central o coordinación entre sistemas.

Pueden verse así:

```
123e4567-e89b-12d3-a456-426614174000

8-4-4-4-12 caracteres
```

#### Archivo requirements.txt

El archivo requirements.txt es un archivo de texto que se usa en proyectos de Python para listar todas las dependencias (paquetes o librerías) que tu aplicación necesita para funcionar correctamente.

Sirve para que cualquier persona (o servidor) que quiera correr tu proyecto pueda instalar exactamente los mismos paquetes que tú usaste, con un solo comando:

```
pip install -r requirements.txt
```

```
flask
flask-smorest
python-dotenv
```

| Paquete         | ¿Qué hace?                                                                         |
| --------------- | ---------------------------------------------------------------------------------- |
| `Flask`         | Framework web para construir APIs y apps web en Python.                            |
| `flask-smorest` | Extensión de Flask para crear APIs REST con validación y documentación automática. |
| `python-dotenv` | Permite usar un archivo `.env` para cargar variables de entorno en tu aplicación.  |

![image](./img/136.png)

1. flask-smorest: 

Es una extensión de Flask que facilita la creación de APIs RESTful usando:

- OpenAPI / Swagger (documentación automática de la API)

- Marshmallow (para validar y serializar datos)

- Blueprints (para organizar el código modularmente)

¿Por qué usarlo?

- Te ayuda a escribir menos código repetitivo.

- Agrega validación automática de entradas y salidas.

- Genera documentación interactiva de tu API (Swagger UI).

2. python-dotenv

python-dotenv es una librería para cargar variables de entorno desde un archivo .env.

Esto es útil para guardar configuraciones como:

- Claves secretas

- Credenciales

- URLs de bases de datos

- Configuración sensible

#### Archivo .flaskenv

El archivo .flaskenv es un archivo especial que se usa en proyectos con Flask para definir variables de entorno específicas que Flask utilizará automáticamente al iniciar la aplicación en modo desarrollo.

Es un archivo de texto plano que contiene pares clave=valor, como:

```
FLASK_APP=app.py
FLASK_ENV=development
FLASK_DEBUG=1
```

1. FLASK_APP = app

Le dice a Flask qué archivo o módulo debe ejecutar cuando corres:

```
flask run
```

En este caso, app se refiere a un archivo llamado app.py, o un módulo llamado app.

2. FLASK_ENV = 'development'

Esta línea activa el modo desarrollo de Flask.

Habilita cosas como:

- Recarga automática cuando cambias el código (hot reload).

- Errores más detallados en pantalla.

- Modo depuración (debug) activado.

Esto solo debe usarse en desarrollo, nunca en producción, porque expone información sensible.

![image](./img/137.png)

![image](./img/138.png)

```
pip install -r requirements.txt
```

Nota: ahora se usa FLASK_DEBUG

![image](./img/139.png)

![image](./img/140.png)

Ahora se reestructura el código, se reemplaza las listas por diccionarios y se crea el archivo db.py:

![image](./img/141.png)

![image](./img/142.png)

Los GET de stores:

1. Obtener la información de todas las tiendas:

```
@app.get("/store")
def get_stores():
    return {"stores": list(stores.values())}
```

2. Obtener la información de una tienda en particular.

```
@app.get("/store/<string:store_id>")
def get_store(store_id):
    try:
        return stores[store_id]
    except KeyError:
        return {"message": "Store not found"}, 404
```

![image](./img/143.png)

Los GET de items:

1. Obtener la información de todos los productos:

```
@app.get("/item")
def get_all_items():
    return {"items": list(items.values())}
```

2. Obtener la información de un producto en particular.

```
@app.get("/item/<string:item_id>")
def get_item(item_id):
    try:
        return items[item_id]
    except KeyError:
        return {"message": "Item not found"}, 404
```

![image](./img/144.png)

Crear una tienda:

```
@app.post("/store")
def create_store():
    store_data = request.get_json()
    store_id = uuid.uuid4().hex
    store = {**store_data, "id": store_id}
    stores[store_id] = store
    return store, 201
```

![image](./img/145.png)

Crear un producto:

```
@app.post("/item")
def create_item():
    item_data = request.get_json()

    if item_data["store_id"] not in stores:
        return {"message": "Store not found"}, 404

    item_id = uuid.uuid4().hex
    item = {**item_data, "id": item_id}
    items[item_id] = item
    return item, 201
```

![image](./img/146.png)


### Mejoras generales en nuestra primera API REST

¿Qué hace abort?

abort corta inmediatamente la ejecución de la ruta (endpoint) y devuelve un error HTTP con un código de estado, mensaje y opcionalmente un detalle.

Es muy útil para:

- Manejar errores personalizados

- Validar datos o condiciones

- Controlar accesos

| Método                    | ¿Lo recomiendo?                     | Cuándo usarlo                                                              |
| ------------------------- | ----------------------------------- | -------------------------------------------------------------------------- |
| `return {...}, 404`       | ✅ Sí                                | Cuando quieras controlar la estructura exacta tú mismo                     |
| `abort(404, message=...)` | ✅✅ Sí (más aún con `flask-smorest`) | Para respuestas de error estructuradas y bien documentadas automáticamente |

```
from flask_smorest import abort
```

Ejemplo:

```
abort(404, message="Store not found")
```

![image](./img/147.png)

![image](./img/148.png)

Se actualiza las urls en el Postman:

1. Crear una tienda:

![image](./img/149.png)

2. Obtener todas las tiendas:

![image](./img/150.png)

3. Obtener una tienda:

![image](./img/151.png)

4. Crear un producto:

![image](./img/152.png)

5. Obtener todos los productos:

![image](./img/153.png)

6. Obtener un producto:

![image](./img/154.png)

Ahora se implementan otras validaciones para las funciones de crear. No obstante, Flask tiene librerías para apoyar el proceso de las pruebas:

![image](./img/155.png)

![image](./img/156.png)

Se prueba en postman:

![image](./img/157.png)

![image](./img/158.png)

### Nuevos puntos finales para nuestra primera API REST

Se adicionan en postman delete y put para store y para items.

![image](./img/159.png)

Delete:

![image](./img/162.png)

Se prueba en postman:

![image](./img/160.png)

![image](./img/161.png)

Update:

![image](./img/163.png)

Se prueba en postman:

- Store:

![image](./img/164.png)

![image](./img/165.png)

- Item:

![image](./img/166.png)

![image](./img/167.png)


### Cómo ejecutar la API en Docker con recarga automática y modo de depuración

En vez de ejecutar la aplicación en Flask se ejecuta en docker, para ello se modifica el Dockerfile:

```
FROM python
EXPOSE 5000
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . /app
CMD ["flask", "run", "--host", "0.0.0.0"]
```

![image](./img/168.png)

```
sudo docker build -t simple-flask-api ./
sudo docker run -dp 5005:5000 simple-flask-api
sudo docker container ls
```

Cada vez que se actualiza el código, hay que reconstruir, para evitar eso se utiliza los volúmenes. Los volúmenes permiten persistir datos fuera del contenedor (útil para bases de datos, configuraciones, etc.).

El volúmen monta tu directorio actual del host (tu máquina) en el contenedor, en la ruta /app. Todo el código que tienes en tu carpeta local se refleja en tiempo real dentro del contenedor en /app.  Esto es útil para desarrollo: si editas archivos en tu máquina, no necesitas reconstruir la imagen Docker para ver los cambios.

Este es el "working directory" (directorio de trabajo) dentro del contenedor. "Cuando entres al contenedor o ejecutes comandos, empieza desde /app."

```
sudo docker run -dp 5005:5000 -w /app -v "$(pwd):/app" simple-flask-api
```

![image](./img/169.png)

| Parte              | Significado                                                                                                                                                           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `docker run`       | Crea y ejecuta un **nuevo contenedor** a partir de una imagen.                                                                                                        |
| `-d`               | **Detached mode**: ejecuta el contenedor en segundo plano (no bloquea la terminal).                                                                                   |
| `-p 5005:5000`     | Mapea el puerto **5000 del contenedor** (donde corre Flask) al puerto **5005 de tu máquina** (puedes acceder desde `localhost:5005`).                                 |
| `-w /app`          | Define el **directorio de trabajo dentro del contenedor** como `/app`. Es como hacer `cd /app` dentro del contenedor.                                                 |
| `-v "$(pwd):/app"` | Monta un **volumen**: el directorio actual (`$(pwd)`) de tu máquina se monta como `/app` dentro del contenedor. Esto permite que Flask vea tus archivos del proyecto. |
| `simple-flask-api` | Es el **nombre de la imagen Docker** desde la que se creará el contenedor.                                                                                            |

¿Qué está pasando exactamente?

Docker crea un contenedor desde la imagen simple-flask-api.

Asigna como directorio de trabajo /app.

Mapea tu carpeta actual (con el código) al directorio /app dentro del contenedor.

Ejecuta Flask en el puerto 5000 (como definido en la imagen).

Expone Flask al puerto 5005 de tu computadora.

El contenedor corre en segundo plano (-d).





### Cómo usar Blueprints y MethodViews en Flask



### Cómo escribir esquemas de malvavisco (Marshmallow) para nuestra API



### Cómo realizar la validación de datos con Marshmallow



### Decorando respuestas con Flask-Smorest


---


## 4. Almacenar datos en una base de datos SQL con SQLAlchemy 🙋🏻‍♀️

- Descripción general y por qué usar SQLAlchemy
- Cómo codificar un modelo simple de SQLAlchemy
- Cómo escribir relaciones uno a muchos con SQLAlchemy
- Cómo configurar Flask-SQLAlchemy con tu aplicación Flask
- Cómo insertar datos en una tabla con SQLAlchemy
- Cómo encontrar modelos en la base de datos por ID o devolver un
- Cómo actualizar modelos con SQLAlchemy}
- Cómo obtener la lista de todos los modelos
- Cómo eliminar modelos con SQLAlchemy
- Eliminar modelos relacionados con cascadas
- Conclusión de esta sección


### Descripción general y por qué usar SQLAlchemy



### Cómo codificar un modelo simple de SQLAlchemy



### Cómo escribir relaciones uno a muchos con SQLAlchemy



### Cómo configurar Flask-SQLAlchemy con tu aplicación Flask



### Cómo insertar datos en una tabla con SQLAlchemy



### Cómo encontrar modelos en la base de datos por ID o devolver un



### Cómo actualizar modelos con SQLAlchemy}



### Cómo obtener la lista de todos los modelos



### Cómo eliminar modelos con SQLAlchemy



### Eliminar modelos relacionados con cascadas



### Conclusión de esta sección


---


## 5. Relaciones de muchos a muchos con SQLAlchemy 🙋🏻‍♀️

- Cambios en esta sección
- Relación uno a muchos entre tiendas y etiquetas
- Relación muchos a muchos entre artículos y etiquetas

### Cambios en esta sección


### Relación uno a muchos entre tiendas y etiquetas


### Relación muchos a muchos entre artículos y etiquetas


---


## 6. Autenticación de usuarios con Flask-JWT-Extended 🙋🏻‍♀️

- ¿Quién usa JWT?
- Cómo configurar Flask-JWT-Extended con nuestra app
- Codificación del modelo y esquema de usuario
- Cómo añadir un endpoint de registro a la API REST
- Cómo añadir un endpoint de inicio de sesión a la API REST
- Proteger los endpoints solicitando un JWT
- Reclamos y autorización de JWT
- Cómo añadir el cierre de sesión a la API REST
- Encadenamiento de solicitudes con Insomnia
- Actualización de tokens con Flask-JWT-Extended

### ¿Quién usa JWT?


### Cómo configurar Flask-JWT-Extended con nuestra app


### Codificación del modelo y esquema de usuario


### Cómo añadir un endpoint de registro a la API REST


### Cómo añadir un endpoint de inicio de sesión a la API REST


### Proteger los endpoints solicitando un JWT


### Reclamos y autorización de JWT


### Cómo añadir el cierre de sesión a la API REST


### Encadenamiento de solicitudes con Insomnia


### Actualización de tokens con Flask-JWT-Extended


---


## 7. Migraciones de bases de datos con Alembic y Flask-Migrate 🙋🏻‍♀️

- Cómo añadir Flask-Migrate a nuestra aplicación Flask
- Inicializa tu base de datos con Flask-Migrate
- Modifica los modelos de SQLAlchemy y genera una migración
- Revisa y modifica manualmente las migraciones de bases de datos

### Cómo añadir Flask-Migrate a nuestra aplicación Flask


###  Inicializa tu base de datos con Flask-Migrate


###  Modifica los modelos de SQLAlchemy y genera una migración


### Revisa y modifica manualmente las migraciones de bases de datos


---


## 8. Curso intensivo de Git 🙋🏻‍♀️

- ¿Qué son los repositorios y las confirmaciones de Git?
- Inicializamos un repositorio Git para nuestro proyecto
- Escribiendo documentos y confirmaciones en Markdown
- Repositorios remotos y cómo usarlos
- Ramas de Git y merging
- Conflictos de merging y cómo resolverlos
- Resumen de los capítulos finales del ebook

### ¿Qué son los repositorios y las confirmaciones de Git?



### Inicializamos un repositorio Git para nuestro proyecto



### Escribiendo documentos y confirmaciones en Markdown



### Repositorios remotos y cómo usarlos



### Ramas de Git y merging



### Conflictos de merging y cómo resolverlos



### Resumen de los capítulos finales del ebook


---

## 9. Implementaciones con Render.com 🙋🏻‍♀️

- Creación de un servicio web de Render.com
- Cómo ejecutar Flask con gunicorn en Docker
- Obtener una base de datos PostgreSQL implementada
- Ejecutar la aplicación y la base de datos localmente con Docker
- Usar PostgreSQL localmente y en producción
- Probar la aplicación de producción terminada

### Creación de un servicio web de Render.com



### Cómo ejecutar Flask con gunicorn en Docker



### Obtener una base de datos PostgreSQL implementada



### Ejecutar la aplicación y la base de datos localmente con Docker



### Usar PostgreSQL localmente y en producción



### Probar la aplicación de producción terminada


---


## 10. Colas de tareas con solicitud y envío de correos electrónicos 🙋🏻‍♀️

- Cómo enviar correos electrónicos con Python y Mailgun
- Cómo enviar correos electrónicos cuando los usuarios se registran
- ¿Qué es una cola de tareas y cómo configurar una base de datos
- Cómo rellenar y consumir la cola de tareas con rq
- Cómo procesar tareas en segundo plano con el trabajador rq
- Cómo enviar correos electrónicos HTML con Mailgun y Python
- Cómo implementar un trabajador en segundo plano en render.com


### Cómo enviar correos electrónicos con Python y Mailgun


### Cómo enviar correos electrónicos cuando los usuarios se registran


### ¿Qué es una cola de tareas y cómo configurar una base de datos


### Cómo rellenar y consumir la cola de tareas con rq


### Cómo procesar tareas en segundo plano con el trabajador rq


### Cómo enviar correos electrónicos HTML con Mailgun y Python


### Cómo implementar un trabajador en segundo plano en render.com

