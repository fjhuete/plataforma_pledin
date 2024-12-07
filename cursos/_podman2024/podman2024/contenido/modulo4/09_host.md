---
title: "Uso de la red host en Podman"
permalink: /cursos/podman2024/contenido/modulo4/09_host.html
---

Si al crear un contenedor indicamos `--network=host`, la pila de red de ese contenedor no está aislada del host, es decir, el contenedor no tiene asignada su propia dirección IP. Por ejemplo, si ejecutas un contenedor que ofrece su servicio en al puerto 80/tcp y utilizas el modo de red host, la aplicación del contenedor estará disponible en el puerto 80/tcp de la dirección IP del host.

## Ventajas del uso de la red host

* Para optimizar el rendimiento.
* En situaciones en las que un contenedor necesita manejar un gran rango de puertos.
    Esto se debe a que no requiere traducción de direcciones de red (DNAT), por lo tanto no usaremos la opción `-p` en la creación del contenedor con `podman run`.

## Ejemplo de uso en contenedores rootful

Vamos a crear un contenedor rootful desde la imagen `nginx` que nos proporciona un servidor web que espera peticiones en el puerto 80/tcp conectado a la red **host**:

```
$ sudo podman run -d --network host --name my_nginx docker.io/nginx
```
Al listar el contenedor, comprobamos que efectivamente no hemos creado ninguna redirección de puertos:

```
$ sudo podman ps
CONTAINER ID  IMAGE                           COMMAND               CREATED        STATUS        PORTS       NAMES
4ebf4a24b7ab  docker.io/library/nginx:latest  nginx -g daemon o...  6 seconds ago  Up 5 seconds              my_nginx
```

Podemos comprobar que no tiene asignada ninguna dirección IP:

```
$ sudo podman inspect --format='{% raw %}{{range .NetworkSettings.Networks}}{% endraw %}{% raw %}{{.IPAddress}}{% endraw %}{% raw %}{{end}}{% endraw %}' my_nginx
```

Y en el host podemos comprobar quien está escuchando en el puerto 80/tcp:

```
$ sudo ss -tulpn | grep :80
```

Por último, desde un navegador podemos acceder al puerto 80/tcp del host para comprobar que funciona. Usando `curl` podemos hacer la prueba desde el mismo host:

```
$ curl http://localhost
```



