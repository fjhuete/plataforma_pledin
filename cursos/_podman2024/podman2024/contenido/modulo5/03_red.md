---
title: "Funcionamiento de la red en un Pod"
permalink: /cursos/podman2024/contenido/modulo5/03_red.html
---

## Configuración de red en los contenedores de un Pod

Cuando creamos un Pod, este recibe una dirección IP, que será compartida con todos los contenedores. 

* En el caso de Pods rootful, si no indicamos lo contrario, el Pod se conectará con la red bridge por defecto. 
* En el caso de Pods rootless, por defecto se conectará a la red slirp4netns. 

A la hora de trabajar con Pods tenemos que tener en cuenta las siguientes características.

* Si queremos ofrecer algún servicio en un puerto, el mapeo del puerto se indicará en la creación del Pod.
* Los contenedores se pueden comunicar entre ellos usando la interfaz loopback: la dirección IP `127.0.0.1` o el nombre `localhost`.

Podemos crear un Pod conectado a cualquier otra red:

```
$ podman network create mired
$ podman pod create --name pod2 --network mired
$ podman pod ps
POD ID        NAME        STATUS      CREATED         INFRA ID      # OF CONTAINERS
1f6ed1602460  pod2        Created     6 minutes ago   be8baecabdc2  1
$ podman pod start pod2
$ podman inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' be8baecabdc2
10.89.0.2
```

## Exponer un servicio en un Pod

Al crear un Pod indicamos el puerto que vamos a mapear con la opción `-p` o `--publish`. Evidentemente sólo podremos tener un contenedor que ofrezca el servicio en el puerto que estamos mapeando:

```
$ podman pod create --name pod3 -p 8080:80
```

A continuación añadimos un contenedor que ofrece un servicio web en el puerto 80:

```
$ podman run -d --pod pod3 --name webserver quay.io/libpod/banner
```

Vemos el Pod y el contenedor que hemos creado:

```
$ podman pod ps --ctr-names
POD ID        NAME        STATUS      CREATED         INFRA ID      NAMES
688e66fd19dd  pod3        Running     2 minutes ago   2d994796c3f2  688e66fd19dd-infra,webserver

$ podman ps --pod
CONTAINER ID  IMAGE                                    COMMAND               CREATED             STATUS             PORTS                 NAMES               POD ID        PODNAME
2d994796c3f2  localhost/podman-pause:4.9.4-1711445992                        2 minutes ago       Up About a minute  0.0.0.0:8080->80/tcp  688e66fd19dd-infra  688e66fd19dd  pod3
b7f0b9aa1aa0  quay.io/libpod/banner:latest             nginx -g daemon o...  About a minute ago  Up About a minute  0.0.0.0:8080->80/tcp  webserver           688e66fd19dd  pod3
```

Podríamos crear un Pod y añadir un contenedor en la misma instrucción, para ello al indicar el nombre del Pod donde se crea el contenedor ponemos el valor `new`:

```
$ podman run -d --pod new:pod4 --name webserver2 -p 8081:80 quay.io/libpod/banner
```

Para acceder al servicio, utilizamos la IP del host y el puerto que hemos mapeado:

```
$ curl http://localhost:8080
   ___          __              
  / _ \___  ___/ /_ _  ___ ____ 
 / ___/ _ \/ _  /  ' \/ _ `/ _ \
/_/   \___/\_,_/_/_/_/\_,_/_//_/
```

## Comunicación entre contenedores dentro del Pod

Como hemos indicado para que un contenedor acceda al servicio de otro contenedor dentro del Pod, deberá usar la interfaz loopback: la dirección IP `127.0.0.1` o el nombre `localhost`:

```
$ podman run -it --pod pod3 --name cliente quay.io/libpod/banner curl http://localhost
   ___          __              
  / _ \___  ___/ /_ _  ___ ____ 
 / ___/ _ \/ _  /  ' \/ _ `/ _ \
/_/   \___/\_,_/_/_/_/\_,_/_//_/
```
