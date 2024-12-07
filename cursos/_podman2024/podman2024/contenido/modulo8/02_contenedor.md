---
title: "Construcción de imágenes a partir de un contenedor"
permalink: /cursos/podman2024/contenido/modulo8/02_contenedor.html
---

La primera forma para crear nuevas imágenes OCI es partiendo de un contenedor que hayamos modificado. Veamos un ejemplo:

Vamos a crear un contenedor a partir de una imagen base.

```
$ podman run -it --name contenedor debian 
```

Realizamos las modificaciones necesarias en el contenedor (instalaciones, modificación de archivos,...). Por ejemplo, instalamos un servidor web y modificamos el fichero `index.html`:

```
root@2df2bf1488c5:/# apt update && apt install apache2 -y
root@75f87f84a091:/# echo "<h1>Curso Podman</h1>" > /var/www/html/index.html
root@75f87f84a091:/# exit
```

Creamos una nueva imagen partiendo de ese contenedor usando `podman commit`. Con esta instrucción se creará una nueva imagen con las capas de la imagen base más la capa propia del contenedor. Si no indicamos etiqueta en el nombre, la imagen se creará con la etiqueta `latest`. Con la opción `--change` podemos indicar algunos comando que posteriormente estudiaremos al trabajar con los ficheros `Containerfile`, por ejemplo podremos indicar el proceso que se va ejecutar por defecto al crear un contenedor, usando la instrucción `CMD` (para indicar el servidor web tenemos que ejecutar `apachectl -D FOREGROUND`):

```
$ podman commit --change='CMD apachectl -D FOREGROUND' contenedor josedom24/myapache2:v1
$ podman images
REPOSITORY                     TAG          IMAGE ID      CREATED         SIZE
localhost/josedom24/myapache2  v1           76c556aa3fcf  49 seconds ago  261 MB
...
```

Ahora podemos crear un nuevo contenedor a partir de esta nueva imagen:

```
$ podman run -d -p 8080:80 --name servidor_web josedom24/myapache2:v1 
```