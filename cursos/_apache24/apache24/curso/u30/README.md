---
title: "Introducción al módulo mod_security2"
permalink: /cursos/apache24/curso/u30/index.html
---

[modSecurity](`https://www.modsecurity.org/`) es un firewall de aplicaciones Web que se ejecuta como módulo del servidor web Apache, provee protección contra diversos ataques hacia aplicaciones Web y permite monitorizar tráfico HTTP, así como realizar análisis en tiempo real sin necesidad de hacer cambios a la infraestructura existente.

## Instalación de modSecurity

Para instalar y activar el módulo ejecutamos:

	# apt-get install libapache2-mod-security2

Por defecto el módulo trae una configuración recomendado, para activarla le cambiamos el nombre:

	# cd /etc/modsecurity
	# mv modsecurity.conf-recommended modsecurity.conf

Cuando reiniciamos el servicio, se ha creado un nuevo fichero de log, donde `mod_security` va a guardar información detallada de las peticiones y respuestas para posibles auditorias: `/var/log/apache2/modsec_audit.log`.

Por defecto la configuración de modsecurity solo permite la detención de los ataques, pero no los evita. En el fichero de configuración `/etc/modsecurity/modsecurity.conf`, podemos encontrar:
	
	SecRuleEngine DetectionOnly

Podemos modificar otras directivas:

* `SecResponseBodyAccess`: Podemos desactivarla para evitar que se guarde el cuerpo de la respuesta.
* `SecRequestBodyLimit`: Podemos especificar el tamaño máximo de los datos POST.
* `SecRequestBodyNoFilesLimit`: De forma similar podemos indicar el tamaño de los datos POST menos el tamaño de los ficheros de subida. Este valor debe ser lo más pequeño posible (128K) (se supone que si no tenemos en cuenta los ficheros subidos los datos que se mandan por POST no deben ser muy grandes).
* `SecRequestBodyInMemoryLimit`: Indica el tamaño de RAM que se utiliza para cachear la petición. Variar este parámetro puede tener consecuencia en el rendimiento del servidor.

## Activando las reglas de detección

Por defecto tenemos un conjunto de reglas activadas, que llamamos CRS (*Core Rules Set*). Si nos fijamos en el fichero de configuración del módulo `/etc/apache2/mods-available/security2.conf`, ademas de indicar el directorio donde se va a guardar información (directiva `SecDataDir`), incluye el fichero donde están definida las CRS:

	IncludeOptional /usr/share/modsecurity-crs/owasp-crs.load

Las reglas se encuentran en el directorio `/usr/share/modsecurity-crs/rules`.

