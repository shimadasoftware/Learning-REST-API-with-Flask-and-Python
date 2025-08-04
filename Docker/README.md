# Introducción a Docker

Los temas a ver son:

- Conceptos
- Arquitectura de Docker
- Instalación de Docker
- 

## 1. Conceptos 🙋🏻‍♀️

### 1. Contenedores (Containers)

Un contenedor es una unidad ligera, portable y autosuficiente que empaqueta una aplicación con todas sus dependencias (bibliotecas, configuraciones, código, etc.).

🔹 Características principales:

Aislamiento: Cada contenedor se ejecuta de forma independiente, sin afectar a otros.

Portabilidad: Funciona igual en cualquier entorno (desarrollo, pruebas, producción).

Eficiencia: Consume menos recursos que una máquina virtual (VM).

Velocidad: Se inicia en segundos.

📌 Diferencia entre contenedores y máquinas virtuales (VMs):

| Contenedores                         | Máquinas Virtuales (VMs)              |
|-------------------------------------|---------------------------------------|
| Comparten el kernel del host        | Cada VM tiene su propio sistema operativo |
| Más ligeros y rápidos               | Más pesados y lentos                  |
| Menor consumo de recursos           | Mayor consumo de CPU/RAM              |
| Ideal para microservicios           | Útil para sistemas completos          |

### 2. Imágenes (Images)

Una imagen es un paquete inmutable (solo lectura) que contiene todo lo necesario para ejecutar una aplicación (sistema de archivos, código, dependencias, configuraciones).

🔹 Características:

Se crean a partir de un Dockerfile (archivo de configuración).

Se almacenan en repositorios como Docker Hub.

Son la "plantilla" para crear contenedores.

📌 Ejemplo:

docker pull nginx → Descarga la imagen oficial de Nginx.


### 3. Dockerfile

Es un archivo de texto que define los pasos para construir una imagen Docker.

🔹 Estructura básica:

Usa una imagen base (ej: Ubuntu)
```
FROM ubuntu:latest
```

Actualiza e instala dependencias
```
RUN apt-get update && apt-get install -y python3
```

Copia archivos al contenedor

```
COPY app.py /app/
```

Define el comando al iniciar

```
CMD ["python3", "/app/app.py"]
```

Comandos clave en Dockerfile:

* FROM → Imagen base.

* RUN → Ejecuta comandos durante la construcción.

* COPY → Copia archivos al contenedor.

* CMD → Comando por defecto al iniciar el contenedor.


### 4. Docker Hub (Registro de imágenes)

Es un repositorio público donde se almacenan imágenes Docker (como GitHub para código).

🔹 Funciones:

    Descargar imágenes oficiales (nginx, mysql, python).

    Subir tus propias imágenes.

    Compartir contenedores con otros desarrolladores.

📌 Ejemplo:

```
docker pull mysql → Descarga la imagen de MySQL desde Docker Hub.
```

### 5. Volúmenes (Volumes)

Los volúmenes permiten persistir datos fuera del contenedor (útil para bases de datos, configuraciones, etc.).

🔹 Tipos de volúmenes:

5.1 Volúmenes nombrados:

```
docker volume create mi_volumen
```

```
docker run -v mi_volumen:/ruta/en/contenedor mysql
```

5.2 Bind mounts (montajes directos):

```
docker run -v /ruta/local:/ruta/en/contenedor nginx
```

📌 ¿Por qué usarlos?

Evita perder datos al eliminar un contenedor.

Permite compartir datos entre contenedores.

### 6. Redes (Networks)

Docker permite crear redes virtuales para que los contenedores se comuniquen entre sí.

🔹 Tipos de redes:

Bridge (predeterminada): Aislamiento interno.

Host: Comparte red con el sistema host.

Overlay: Para contenedores en diferentes hosts (Docker Swarm/Kubernetes).

📌 Ejemplo:

```
docker network create mi_red
docker run --network=mi_red --name=app1 nginx
docker run --network=mi_red --name=app2 mysql
```

app1 y app2 pueden comunicarse usando sus nombres (app1, app2).


### 7. Docker Compose

Herramienta para definir y gestionar aplicaciones multi-contenedor usando un archivo YAML.

🔹 Ejemplo (docker-compose.yml):

```
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: contraseña
```      

Comandos útiles:

```
docker-compose up -d   # Inicia los servicios
docker-compose down    # Detiene y elimina todo
```

Resumen de Comandos Básicos

| Comando                          | Descripción                                 |
|----------------------------------|---------------------------------------------|
| `docker run -d nginx`           | Ejecuta Nginx en segundo plano              |
| `docker ps`                     | Lista contenedores en ejecución             |
| `docker stop <ID>`             | Detiene un contenedor                       |
| `docker rm <ID>`               | Elimina un contenedor                       |
| `docker images`                | Lista imágenes disponibles                  |
| `docker rmi <IMAGEN>`          | Elimina una imagen                          |
| `docker exec -it <ID> bash`    | Entra a un contenedor en ejecución          |
| `docker build -t mi-app .`     | Construye una imagen desde un Dockerfile    |


---

## 2. Arquitectura de Docker 🙋🏻‍♀️

### ¿Qué es Docker Engine?

Docker Engine es el componente central de Docker que permite crear, ejecutar y gestionar contenedores. Es un software cliente-servidor que incluye:

* Docker Daemon (dockerd): Servidor en segundo plano que gestiona contenedores, imágenes, redes y volúmenes.

* Docker API: Interfaz REST para comunicarse con el daemon (usada por CLI y herramientas externas).

* Docker CLI (docker): Interfaz de línea de comandos para interactuar con el daemon.

📌 Resumen:
docker run (CLI) → API REST → dockerd (Daemon) → Crea/Ejecuta contenedores.


La arquitectura de Docker sigue un modelo cliente-servidor y se compone de los siguientes elementos:

### A. Docker Client (CLI)

Herramienta principal para que los usuarios interactúen con Docker (docker run, docker build, etc.).

Envía comandos al Docker Daemon mediante la API REST.

### B. Docker Daemon (dockerd)

Escucha peticiones a través de la API (por defecto en /var/run/docker.sock).

Gestiona:

* Contenedores (creación, ejecución, monitoreo).

* Imágenes (descarga desde registros como Docker Hub).

* Redes (configuración de redes virtuales).

* Volúmenes (almacenamiento persistente).

### C. Containerd

Componente de bajo nivel que ejecuta los contenedores (separado del Daemon desde Docker 1.11+).

Responsable de:

* Ciclo de vida del contenedor (start/stop/pause).

* Gestión de imágenes (pull/commit).

* Supervisión de procesos.

### D. runC

Herramienta ligera que implementa el estándar OCI (Open Container Initiative).

Crea y ejecuta contenedores según las especificaciones de OCI.

📌 Flujo de ejecución de un contenedor:
docker run → Docker Daemon → containerd → runC → Contenedor en ejecución.

---

## 3. Instalación de Docker 🙋🏻‍♀️

A continuación se realiza la instalación de Docker en Linux con distribución de Fedora 42. 

### 1. Configurar el repositorio de paquetes de Docker

[Link](https://docs.docker.com/engine/install/fedora/#uninstall-old-versions)

**1.1 Configurar el repositorio de paquetes de Docker:**

```

sudo dnf install -y dnf-plugins-core

```

![image](./img/1%20instalar%20plugins.png)

**1.2: Agregar el repositorio oficial de Docker:**

```

sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo

```

![image](./img/2%20error.png)

<blockquote>
<strong>⚠️ WARNING: </strong> Si se presenta error se descarga manualmente el archivo .repo de docker:
</blockquote>

- Ejecutar este comando para crear el archivo de configuración del repositorio:

```

sudo tee /etc/yum.repos.d/docker-ce.repo << 'EOF'
[docker-ce-stable]
name=Docker CE Stable - \$basearch
baseurl=https://download.docker.com/linux/fedora/\$releasever/\$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/fedora/gpg
EOF

```

¿Qué hace este comando?

Crea el archivo /etc/yum.repos.d/docker-ce.repo con la configuración oficial de Docker.

Habilita el repositorio (enabled=1).

Verifica firmas GPG (gpgcheck=1).

- Actualizar la caché de paquetes

```

sudo dnf makecache

```

- Verificar que el archivo existe

Ejecuta este comando para comprobar si el archivo del repositorio (docker-ce.repo) está en la ubicación correcta:

```

ls -l /etc/yum.repos.d/docker-ce.repo

```

Si existe, verás algo como: -rw-r--r--. 1 root root 320 [fecha] /etc/yum.repos.d/docker-ce.repo

- Revisar el contenido del archivo

Para asegurarte de que el contenido del archivo es correcto:

```

cat /etc/yum.repos.d/docker-ce.repo

```

Debe mostrar algo similar a esto:

```

[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/fedora/$releasever/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/fedora/gpg

```

- Verificar que el repositorio está activo en DNF

Comprueba si Docker aparece en la lista de repositorios disponibles:

```

sudo dnf repolist | grep -i docker

```

Si está correctamente agregado, verás:

```

docker-ce-stable      Docker CE Stable - x86_64

```

![image](./img/2%20Agregar%20el%20repositorio%20oficial%20de%20docker.png)


**1.3 Instalar Docker Engine:**

Instala Docker Engine, containerd y Docker Compose:

```

sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin


```

![image](./img/3%20instalar%20docker%20engine.png)

Si se le solicita que acepte la clave GPG, verifique que la huella digital coincida con 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35 y, de ser así, acéptela.

![image](./img/3%20salida%20llave.png)

Este comando instala Docker, pero no lo inicia. También crea un grupo de Docker; sin embargo, no agrega ningún usuario al grupo por defecto.


**1.4 Habilitar e iniciar el servicio Docker:**


Habilita Docker para que se inicie automáticamente al arrancar el sistema y luego inicia el servicio:

```

sudo systemctl enable --now docker

```

Salida:

```

Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /usr/lib/systemd/system/docker.service.

```

![image](./img/4%20iniciar%20docker.png)

<blockquote>
<strong>⚠️ WARNING: </strong> Verificación adicional
</blockquote>

```

systemctl status docker

```

● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
     Active: active (running) since [fecha y hora]
       Docs: https://docs.docker.com
   Main PID: 1234 (dockerd)
      Tasks: 8
     Memory: 10.5M
        CPU: 500ms
     CGroup: /system.slice/docker.service
             └─1234 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
             
             
Claves para verificar:

Loaded: enabled → Servicio habilitado en el arranque.

Active: active (running) → Docker está funcionando.           
             
             
**1.5 Verificar la instalación:**


Comprueba que Docker se ha instalado correctamente ejecutando el contenedor de prueba "hello-world":

```

sudo docker run hello-world

```

![image](./img/5%20probar%20docker.png)


### 2. Instalar Docker Desktop

[Link de la guía de Docker](https://docs.docker.com/desktop/setup/install/linux/fedora/)

**2.1 Descargar el Paquete RPM de Docker Desktop:**

[Link de la última versión](https://desktop.docker.com/linux/main/amd64/docker-desktop-x86_64.rpm?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64)

```
cd Descargas/

sudo dnf install ./docker-desktop-x86_64.rpm

```

![image](./img/6%20instalar%20docker%20deskstop.png)

![image](./img/6%20salida.png)

DNF instalará automáticamente las dependencias requeridas (ej. containerd, docker-ce-cli).

**2.2 Iniciar Docker Desktop:**

Buscar "Docker Desktop" en el menú de aplicaciones (GUI) o Iniciar desde terminal:

```

systemctl --user start docker-desktop

```

![image](./img/7%20lanzar%20docker%20desktop.png)

Aceptar los permisos:

La primera vez, pedirá permisos para:

* Configurar redes virtuales.

* Integración con WSL2 (si la usas).

![image](./img/8%20aceptar%20términos.png)


**2.3 Verificar versiones:**

```
juana@fedora:~$ docker --version
Docker version 28.3.3, build 980b856

juana@fedora:~$ docker compose version
Docker Compose version v2.38.2-desktop.1

juana@fedora:~$ docker version
Client: Docker Engine - Community
 Version:           28.3.3
 API version:       1.51
 Go version:        go1.24.5
 Git commit:        980b856
 Built:             Fri Jul 25 11:36:40 2025
 OS/Arch:           linux/amd64
 Context:           desktop-linux

Server: Docker Desktop 4.43.2 (199162)
 Engine:
  Version:          28.3.2
  API version:      1.51 (minimum version 1.24)
  Go version:       go1.24.5
  Git commit:       e77ff99
  Built:            Wed Jul  9 16:13:55 2025
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.27
  GitCommit:        05044ec0a9a75232cad458027ca83437aae3f4da
 runc:
  Version:          1.2.5
  GitCommit:        v1.2.5-0-g59923ef
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

```

![image](./img/9%20versions.png)
