---
title: "Ejecutando recursos de Kubernetes en Podman"
permalink: /cursos/podman2024/contenido/modulo5/09_kubernetes2.html
---

Además de poder generar definiciones de recursos de Kubernetes en fichero YAML. En Podman también podemos ejecutar Pods y contenedores a partir de manifiestos YAML de Kubernetes.

Puedes encontrar los ficheros que vamos a utilizar en el directorio `modulo5/kube` del [Repositorio con el código de los ejemplos](https://github.com/josedom24/ejemplos_curso_podman_ow).

En primer lugar vamos a ejecutar recursos en Podman a partir del fichero `wp-mariadb-pod.yaml` que hemos generado en el apartado anterior a partir del escenario que construimos en el **Ejemplo: Desplegado WordPress + MariaDB en un Pod**:

```
$ podman kube play wp-mariadb-pod.yaml
...
Volumes:
dbvol
wpvol
Pod:
cd2d6fc0916ba73e769562382b80464108b1305e28db9847f963212c608a43e8
Containers:
93360a599956da91011c1b720e2fe599e49afd898810c19bba079a4ddf828b1c
42621e47f8155ae40c1d3a7139279d8f23c1c40be21bbc65b742b511cced34c9
```

Podemos comprobar que hemos creado un Pod con dos contenedores y dos volúmenes:

```
$ podman pod ps --ctr-names
POD ID        NAME           STATUS      CREATED        INFRA ID      NAMES
cd2d6fc0916b  wordpress-pod  Running     2 minutes ago  bee585832127  cd2d6fc0916b-infra,wordpress-pod-db,wordpress-pod-wordpress

$ podman volume ls
DRIVER      VOLUME NAME
local       dbvol
local       wpvol
```

Ahora crearemos recursos en Podman a partir del fichero `wp-mariadb-multipod.yaml` que hemos generado en el apartado anterior a partir del escenario que construimos en el **Ejemplo: Despliegue de WordPress + MariaDB en un escenario multipod**:


```
$ podman kube play wp-mariadb-multipod.yaml
Volumes:
dbvol
wpvol
Pod:
0e07102382de6961e899eba77b60614dd449fbc72b6ff64a38a918bf565492d2
Container:
13d2ab8682dc6fcb9fb47db6c8b9eb12a36e57a8ccf62d1c32c79ff62e8d2beb

Pod:
b1dd4010698aeda650b9ba3c808de81ad16b1d4845a68be0a3d6013e18c217b1
Container:
7bc05f2f048843b03bba694fc4802d299c7c5454721123029f31f9b167c18379
```
Ahora comprobamos que hemos creados dos Pods, cada uno con un contenedor y los dos volúmenes:

```
$ podman pod ps --ctr-names
POD ID        NAME           STATUS      CREATED         INFRA ID      NAMES
b1dd4010698a  mariadb-pod    Running     34 seconds ago  5cf7cb140f7a  b1dd4010698a-infra,mariadb-pod-db
0e07102382de  wordpress-pod  Running     40 seconds ago  9ce1886ab6cc  0e07102382de-infra,wordpress-pod-wordpress

$ podman volume ls
DRIVER      VOLUME NAME
local       dbvol
local       wpvol
```
