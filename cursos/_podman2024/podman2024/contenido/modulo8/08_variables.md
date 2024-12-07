---
title: "Uso de ficheros Containerfile parametrizados"
permalink: /cursos/podman2024/contenido/modulo8/08_variables.html
---

El uso de parámetros en la creación de ficheros `Containerfile` es una buena forma de añadir flexibilidad a tus construcciones de imágenes. El valor de estos parámetros se pueden pasar a la hora de construir la imagen con el comando `podman build` o indicando valores predeterminados si no se especifican en el momento de la construcción.

Puedes encontrar los ficheros que vamos a utilizar en el directorio `modulo8/variables` del [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_podman_ow).

Veamos un ejemplo:

```
ARG PHP_VERSION=8.2-apache
FROM docker.io/php:${PHP_VERSION}
ARG APP_VERSION=desarrollo
ENV VERSION=${APP_VERSION}
COPY app /var/www/html/
EXPOSE 80
```

Para crear los parámetros usamos la instrucción `ARG` en el fichero `Containerfile`. Hemos creado dos parámetros en la definición del fichero `Containerfile`:

* `PHP_VERSION`: Donde vamos a indicar la etiqueta de la imagen PHP que vamos a usar en la construcción. Su valor por defecto es `8.2-apache` y como vemos, para hacer referencia a ella usamos `${PHP_VERSION}`.
* `APP_VERSION`: Es un parámetro donde vamos a guardar la versión de la aplicación (`desarrollo`) y que posteriormente utilizaremos para darle valor a la variable de entorno `VERSION`.

Si construimos la imagen sin sobrescribir los valores de los parámetros se tomarán los valores por defecto:

```
$ podman build  -t josedom24/app_php:v1 .
$ podman run -d -p 8081:80 --name app1 josedom24/app_php:v1
$ podman exec -it app1 env
...
VERSION=desarrollo
...
```

Como vemos se ha creado una imagen a partir de la imagen `php:8.2-apache` y se ha creado una variable de entorno `VERSION` con el valor por defecto. Si accedemos a la página con un navegador web, veremos la versión de PHP y el valor de la variable de entorno que hemos creado (también podríamos acceder al fichero `info.php` para ver esta información):

![php](img/variables1.png)

Podemos crear otra versión de la imagen sobrescribiendo los parámetros de construcción, para ello usamos el parámetro `--build-arg` en el comando `podman build`:

```
$ podman build --build-arg PHP_VERSION=7.4-apache --build-arg APP_VERSION=produccion -t josedom24/app_php:v2 .
$ podman run -d -p 8082:80 --name app2 josedom24/app_php:v2
$ podman exec -it app2 env
...
VERSION=produccion
...
```

Accedemos a esta nueva aplicación y vemos los parámetros que hemos configurado:

![php](img/variables2.png)

