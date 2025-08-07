# Git

## Configuración de cuenta Git y conexión con Github

Guía paso a paso para configurar Git con otra cuenta distinta a la global. La cuenta solo se utiliza en repositorios específicos.

1. Iniciar git en el repositorio.

```

git init

```

![image](./img/1.png)

2. Verificar la configuración actual.

```

git config --local --list

```

![image](./img/2.png)

3. Configurar usuario y email solo para este repositorio:

```

git config user.name "JuanaVMendozaS"
git config user.email "jv.mendoza@uniandes.edu.co"
git config --local --list

```

![image](./img/3.png)

4. Generar una nueva clave SSH para la otra cuenta (no sobreescribir la personal):

```

ssh-keygen -t ed25519 -C "jv.mendoza@uniandes.edu.co" -f ~/.ssh/id_ed25519_uniandes

```

![image](./img/4.png)

5. Agregar la clave al agente SSH:

```

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_uniandes

```

![image](./img/5.png)

![image](./img/6.png)

6. Configurar ~/.ssh/config para usar la clave correcta:

```

nano ~/.ssh/config

```

```

Host github.com-uniandes
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_uniandes
  IdentitiesOnly yes

```

![image](./img/7.png)

7. Ver el contenido de la llave pública:

```

cat ~/.ssh/id_ed25519_uniandes.pub

```

![image](./img/8.png)

Verás algo como:

```

ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJt7... jv.mendoza@uniandes.edu.co

```

Posteriormente, en el Github se agrega la llave pública para conectar el Github. Ve a Settings → SSH and GPG keys → New SSH key.

![image](./img/9.png)

Probar la conexión SSH:

```

 ssh -T git@github.com-uniandes

```

![image](./img/10.png)


## Tutorial: Crear y clonar repositorio

### Introducción

**Objetivos**

Al finalizar el tutorial el estudiante estará en capacidad de:

* Entender las herramientas para aplicar el enfoque de documentación como código en sus proyectos.
* Generar y verificar automáticamente documentación técnica de un proyecto de código en un repositorio.

**Requisitos y Pasos previos**

* Tener instalado git en su ambiente. Puede hacer uso de un cliente git o con bash.
* En este tutorial haremos uso de Bash por lo que algunos comandos pueden no funcionar igual si hace uso de Powershell en Windows.
* Tener instalado Docker en su ambiente.

**Documentación como Código**

En este tutorial trabajaremos en las siguientes prácticas para implementar documentación cómo código [1]:

* Mantener la documentación en un sistema de control de versiones cómo **Git**.
* Mantener la documentación en un formato plano y legible para humanos usando **Markdown**.
* Generar documentación de manera automática usando herramientas como **Sphinx**, **Plantuml** y **OpenAPI**.
* Revisar los cambios en la documentación como parte del proceso de desarrollo haciendo uso de **PRs** y **MRs**.
* Integrar la documentación al proceso de integración continua haciendo uso de herramientas cómo **Vale**.
* Integrar la documentación al proceso de despliegue continuo haciendo uso de generadores de sitios estáticos cómo **github pages**.


Para este tutorial haremos uso de este [template](https://github.com/MISW-4301-Desarrollo-Apps-en-la-Nube/dann-personal-username), para crear su repositorio personal y realizar pruebas de código y pipelines sin agregar código innecesario a su repositorio del proyecto.

![image](./img/11.png)

Acceda al repositorio y haga click en el botón Use this template, y después Create a new repository.

![image](./img/12.png)

Clone el repositorio recién creado en su ambiente ejecutando:

```

git clone <url repositorio git>
git clone git@github.com:MISW-4301-Desarrollo-Apps-en-la-Nube/dann-personal--JuanaVMendozaS-.git

```

![image](./img/13.png)

![image](./img/14.png)


## Tutorial: Crear documentación

Abra el proyecto en el IDE de su preferencia. Si está trabajando con Visual Studio Code, Cursor o Windsurf le recomendamos instalar los siguientes plugins:

- Prettier
- PlantUML
- Markdown all in one

![image](./img/16.png)

![image](./img/17.png)

![image](./img/18.png)

1. Cree una nueva rama con el nombre feature/dac, puede hacerlo en su terminal ejecutando:

```

git checkout -b feature/dac

```

![image](./img/15.png)

### PlantUML

PlantUML [2] es una herramienta que permite crear diagramas UML haciendo uso de archivos planos siguiendo una estructura y lenguaje definido. Puede encontrar la documentación y ejemplos en la página oficial: https://plantuml.com/

La herramienta le permite diagramar y generar diagramas UML de clase, de secuencia, de componentes, y de despliegue, entre otros.

```

En Visual Studio Code: ctrl + shift + p

PlantUML: Review Current Diagram  

```

### Vista de componentes

En la carpeta /docs/diagrams cree un archivo con el nombre components.puml y agregue el siguiente código que permite crear un diagrama básico de componentes:

docs/diagrams/components.puml

![image](./img/19.png)

```

@startuml

skinparam componentStyle uml1

component "Application1" as application1
component "Application2" as application2

interface "http" as app1interface
app1interface - application1
application2 --> app1interface

@enduml

```

![image](./img/20.png)

Este código en PlantUML define un diagrama de componentes UML que muestra la relación entre dos aplicaciones (`Application1` y `Application2`) a través de una interfaz HTTP.

### Explicación detallada:

1. **`skinparam componentStyle uml1`**  

Establece el estilo del diagrama como "UML1", que es un estilo clásico de UML con iconos rectangulares para componentes.

2. **Definición de componentes**:

- `component "Application1" as application1`  

Crea un componente llamado "Application1" y le asigna el alias `application1`.

- `component "Application2" as application2`  

Crea un componente llamado "Application2" con el alias `application2`.

3. **Interfaz HTTP**:

- `interface "http" as app1interface`  

Define una interfaz llamada "http" (que representa un contrato o API HTTP) con el alias `app1interface`.

4. **Conexiones**:

- `app1interface - application1`  

Conecta la interfaz HTTP con `Application1` usando una línea sólida (`-`), indicando que la interfaz **pertenece** a `Application1` (es decir, `Application1` implementa/provee la interfaz HTTP).

- `application2 --> app1interface`  

Muestra una dependencia de `Application2` hacia la interfaz HTTP con una flecha discontinua (`-->`), lo que significa que `Application2` **usa** la interfaz HTTP de `Application1` (por ejemplo, consume su API REST).

### Representación visual:

- **`Application1`** (proveedor):  

Tiene una interfaz HTTP asociada (como un "punto de conexión" o puerto).

- **`Application2`** (cliente):  

Depende de la interfaz HTTP de `Application1` para comunicarse.

### Ejemplo en el mundo real:

- `Application1` podría ser un backend que expone una API REST.

- `Application2` sería un frontend o otro servicio que consume esa API.

### Nota sobre estilos:

- `uml1` muestra las interfaces como "circulos" (icono clásico de UML), mientras que otros estilos como `uml2` las representan como rectángulos con la etiqueta `<<interface>>`. 

En esta configuración puede observar que hay dos componentes los cuales se comunican por medio de la interfaz de uno de ellos. La documentación de este tipo de diagrama lo puede encontrar acá https://plantuml.com/component-diagram.

Esta configuración representa el siguiente diagrama:

Para observar el diagrama tiene dos alternativas:

1. Hacer uso del plugin en visual studio code (ctrl + shift + p), seleccionado el comando Preview:

![image](./img/21.png)

![image](./img/22.png)

2. Hacer uso del editor en línea de plantUML https://editor.plantuml.com/uml/

![image](./img/23.png)

Hands on: Agregue dos aplicaciones más como componentes, y otro componente que corresponda a un proxy. Conecte todos los componentes al proxy.

Revise la documentación del diagrama de componentes, encontrará cómo decorar, agrupar y añadir notas, entre otros features.

![image](./img/24.png)

```

@startuml

skinparam componentStyle uml1

' Aplicaciones existentes
component "Application1" as application1
component "Application2" as application2

' Nuevas aplicaciones
component "Application3" as application3
component "Application4" as application4

' Componente proxy
component "Proxy" as proxy

' Interfaces existentes
interface "http" as app1interface
app1interface - application1
application2 --> app1interface

' Conexiones al proxy
application1 --> proxy
application2 --> proxy
application3 --> proxy
application4 --> proxy

@enduml


```

#### Vista de despliegue

En la carpeta /docs/diagrams cree un archivo con el nombre deployment.puml y agregue el siguiente código:

docs/diagrams/deployment.puml

En este diagrama puede observar que se usan frames para agrupar y describir los servicios que se están usando. El diagrama resultante presenta una aplicación (componente) que se ejecuta en un nodo sobre el servicio ec2 en el proveedor aws.. La documentación de este diagrama lo puede encontrar acá https://plantuml.com/deployment-diagram.

Haciendo uso del plugin o el editor en línea podrá ver que el resultado de esta configuración es el siguiente:

```

@startuml

frame "aws" {
    frame "ec2" {
        node "node1" {
           component "application" as app
        }
    }
}

note top of app : Mi aplicación

@enduml

```

![image](./img/25.png)

Hands on: Agregue un API Gateway en el diagrama y conéctelo al componente de aplicación.

Revise la documentación del diagrama de componentes, encontrará cómo decorar, agrupar y añadir notas, entre otros features.

#### Vista de información

En la carpeta /docs/diagrams cree un archivo con el nombre data.puml y agregue el siguiente código que permite diagramar entidades:

docs/diagrams/data.puml

```

@startuml

skinparam linetype polyline
skinparam linetype ortho


entity pets {
    * id : uuid
    --
    * created : timestamptz
    name : varchar(255)
    updated : timestamptz
}

entity owners {
    * id : uuid
    --
    * name : varchar(255)
    * document : varchar(255)
    * created : timestamptz
    updated : timestamptz
}

pets }|..|| owners

@enduml

```

![image](./img/26.png)

En este diagrama se representan dos entidades por medio de un diagrama entidad relación. La documentación de este tipo de diagramas la encuentra en https://plantuml.com/ie-diagram.

También puede hacer uso de diagramas de clases para presentar sus modelos de información. https://plantuml.com/class-diagram


Hands on: Coloque las tablas dentro de una base de datos y ubíquelos en un modelo de despliegue.

Revise la documentación del diagrama de información, encontrará cómo decorar, agrupar y añadir notas, entre otros features.

```

@startuml

skinparam linetype polyline
skinparam linetype ortho

' Vista de despliegue con base de datos
node "Database Server" {
    database "PostgreSQL" as db {
        entity pets {
            * id : uuid
            --
            * created : timestamptz
            name : varchar(255)
            updated : timestamptz
        }

        entity owners {
            * id : uuid
            --
            * name : varchar(255)
            * document : varchar(255)
            * created : timestamptz
            updated : timestamptz
        }
    }
}

' Relación entre tablas
pets }|..|| owners : owned by

@enduml

```

![image](./img/27.png)


#### README

Cree un archivo tipo markdown con el nombre README.md en la carpeta /docs y agregue el siguiente contenido:

docs/README.md

```

# Documentación técnica del proyecto

Este es un ejemplo de documentación técnica del proyecto

## Vistas de arquitectura

* Vista de componentes
  
![Vista de componentes](./diagrams/components.png "Vista de componentes")

* Vista de despliegue

![Vista de despliegue](./diagrams/deployment.png "Vista de despliegue")

* Vista de información

![Vista de información](./diagrams/data.png "Vista de información")

```

![image](./img/28.png)

Podrá observar que el documento referencia los diagramas como archivos tipo PNG y NO archivos PUML, dado que markdown no soporta archivos PUML si queremos mostrar nuestros diagramas en la documentación.

Para generar imágenes PNG usaremos PlantUML, pero para ello necesitamos instalar Java y PlantUML en nuestro ambiente. Con el fin de evitar instalar más herramientas en el ambiente puede hacer uso de un contenedor en docker ejecutando el siguiente comando:

```

docker run --rm -e PLANTUML_LIMIT_SIZE=8192 -v $(pwd)/docs/diagrams:/workspace -w /workspace plantuml/plantuml **.puml

```

Ese mismo comando ya está en el archivo makefile del repositorio. Si su ambiente soporta make solo debe ejecutar:

```

make docsbuild

```

Verifique que las imágenes fueron creadas satisfactoriamente en la carpeta docs/diagrams.

Usando el plugin de markdown podrá ver un documento similar a este:

```

En Visual Studio Code: ctrl + shift + p

Markdown: Open Preview

```

![image](./img/29.png)

![image](./img/30.png)























































































































