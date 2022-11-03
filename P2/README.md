# P2 - Configuración de DNS en un dominio (II)

En esta práctica tenemos que añadir una serie de registros nuevos. Para ello, tendremos que buscar la información sobre qué datos van en los registros de fuentes acreditadas (idealmente, de la documentación oficial)

- **Registros MX de Google**: https://support.google.com/a/answer/174125?hl=es
- **Registro TXT SPF de Google:**: https://support.google.com/a/answer/10684623?hl=es
- **Registros CAA para Let's Encrypt**: https://letsencrypt.org/es/docs/caa/ y https://sslmate.com/caa/

## Primeros pasos

Modificamos el fichero `/etc/bind/db.stucomx.net` partiendo de cómo estaba en la práctica anterior, modificando el valor de serial cada vez que cambiemos el contenido del fichero.

## Registros MX de Google

Estos registros los utilizaremos para poder mandar y recibir emails utilizando Google Workspace.

Añadimos los nuevos registros MX que consultamos en la documentación oficial.

Los registros MX tienen un campo nuevo, la prioridad. La TTL se configura global al comienzo del fichero o en cada RR (Google recomienda una corta al principio, mientras se configura, pero volver a un día de TTL cuando ya está configurado, y así es como se deja el fichero)

## Registros TXT SPF de Google

Estos registros sirven para proteger el envío de correos electrónicos. Como explica Google en su documentacion:

> _SPF sirve para identificar los servidores de correo que tienen permiso para enviar mensajes en nombre de tu dominio. Los servidores que reciben correo utilizan SPF para comprobar que los mensajes entrantes que parecen proceder de tu dominio se hayan enviado desde servidores que hayas autorizado._

Modificamos, una vez más, el fichero `/etc/bind/db.stucomx.net` y añadimos el registro TXT siguiendo las instrucciones de la documentación oficial.

## Registros CAA de Let's Encrypt

Estos registros sirve para protegernos de que alguien creee certificados para suplantar nuestros dominios sin nuestro conocimiento.

En la documentación de Let's Encrypt que se incluye más arriba se explica más en detalle, además de incluirse una página en la que podemos lograr que nos especifiquen cómo añadir el registro que necesitamos.

Con todo ello, modificamos el fichero `/etc/bind/db.stucomx.net`.

## Comprobando hasta ahora

Reiniciamos el servicio para aplicar los cambios y probamos con los siguientes comandos:

> `nslookup -type=MX stucomx.net locahost`

> `nslookup -type=TXT stucomx.net locahost`

> `nslookup -type=CAA stucomx.net locahost`

Deberíamos obtener cada uno de los registros que hemos añadido antes.

## Registros inversos

Para los registros inversos podemos seguir las insutrcciones que están en la sección [Reverse Zone File](https://ubuntu.com/server/docs/service-domain-name-service-dns) de la documentación oficial de Ubuntu.

### Añadiendo el fichero de registros inversos

Para la sección de registros inversos tenemos que volver a modificar el fichero `/etc/bind/named.conf.local` añadiendo una nueva zona y un nuevo fichero con la información de los registros.

### Copiar el fichero de los registros

Como hicimos en la P1 para crear el nuevo fichero de registros, podemos coger un modelos y copiarlo:

> `sudo cp /etc/bind/db.127 /etc/bind/db.192`

### Rellenar de contenido los registros

Modificamos el fichero `/etc/bind/db.192`

La sintaxis es la misma que para `/etc/bind/db.stucomx.net`, solo que los registros ahora son de tipo `IN PTR`y debemos indicar la IP como el campo de nombre y el nombre en el campo de dato (el primero y el último, respectivamente).

Debemos tener las mismas precauciones que en el fichero de registros que ya hicimos (el serial, el example.com...)

### Aplicamos los cambios

Una vez más, deberemos reiniciar el servicio.

### Comprobando

> `nslookup -type=PTR 192.168.1.1 locahost`

> `nslookup -type=PTR 192.168.1.200 locahost`

> `nslookup -type=PTR 192.168.1.42 locahost`