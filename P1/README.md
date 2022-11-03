# Práctica 4 - Configurción de un DNS para un dominio

## Instalando el software

Debemos instalar algunos paquetes de software. Como mínimo `bind9`:

> `sudo apt install bind9`

También es recomenbable instalar las `dnsutils`

> `sudo apt install dnsutils`

## Definición de la zona

Debemos modificar, primero, el fichero `/etc/bind/named.conf.local`. En él, debemos especificar una zona para nuestro dominio (_stucomx.net_), además de el tipo (principal o master) y el fichero en el que estarán los registros de configuración (los RR).

## Definición de los RR

Empezamos copiando el fichero de ejemplo al fichero cuya ruta hayamos especificado en `/etc/bind/named.conf.local`:

> `sudo cp /ect/bind/db.empty /etc/bind/db.stucomx.net`

Y editamos el nuevo fichero con nuestro editor de preferencia.

Debemos hacer varias modificaciones:
- Cambiar todas las referencias a example.com o localhost por nuestro dominio.
- Cada vez que modifiquemos el fichero, deberemos actualizar el campo Serial que aparece en el primer RR.
- Deberemos añadir una línea nueva al final para cada registro de los que nos pide el enunciado. Todos son de tipo `IN A` menos `www` y `ns`, que son de tipo `IN CNAME`.

## Aplicando las configuraciones

Reiniciamos el sistema para aplicar las configuraciones realizadas anteriormente.

> `sudo systemctl restart bind9`

## Comprobaciones

Como nuestro servidor no está configurado como un servidor primaro o que replique a otros, combiene no ponerlo como el DNS principal de nuestra máquina.

En su lugar, podemos hacer peticiones al servidor que hemos configurado especificando `localhost` como el servidor cuando hagamos las peticiones con `nslookup`

> `nslookup server.stucomx.net localhost`
>
> `nslookup www.stucomx.net localhost`
>
> `nslookup pc45.stucomx.net localhost`

Y así para todos los registros.