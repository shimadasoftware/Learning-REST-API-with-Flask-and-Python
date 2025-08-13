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



### Cómo ejecutar una aplicación Flask en un contenedor Docker


### Cómo ejecutar la API REST de Flask con Docker Compose


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



### Mejoras generales en nuestra primera API REST



### Nuevos puntos finales para nuestra primera API REST



### Cómo ejecutar la API en Docker con recarga automática y modo de depuración



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

