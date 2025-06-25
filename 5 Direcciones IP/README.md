# Direcciones IP

## ¿Qué es?

Es un conjunto de números, separados por puntos o 2 puntos, que identifican de forma única a un dispositivo conectado a una red.

_Ejemplo: **127.0.0.1**_

Los dispositivos está utilizando el procolt **_IP_ = Internet Protocol**, el protocolo de conexión.

## Direcciones MAC (Físicas)

Es un conjunto de números hexadecimales, separados por guiones o 2 puntos, que identifican a un tarjeta de red de forma única conectado a una red, no importando el protocolo de conexión.

_Ejemplo: **80-11-98-17-DD-F7**_

Por lo tanto, no se debe de confundir: **_Direcciones IP no es lo mismo que Direcciones MAC_**

## Asignación de Dirección IP a un host

Para simplificar la explicación, un host es cualquier dispositivo conectado a una red. Para asignarle una direccion IP a un host, hay 2 formas:

1. _Manual_: Mediante la configuracion de red del software del dispositivo.
2. _Dinámica_: Por **DHCP**, protocolo de red que permite la asignación automática de direcciones IP y otros parámetros de configuración a los dispositivos en una red. El servidor DHCP se encargará de asignarles una automáticamentea c/host.

Esta asignación se hace de forma privada, es decir, los hosts de la red se podrán comunicar entre ellos. Para que otras redes se comuniquen con otras redes, se tienen que ver otras configuraciones, protocoles y mecanismos.

## Comunicación entre hosts

Toda comunicación entre 2 hosts de una red tiene que hacerse por su dirección IP, ya sea de asignada por las formas ya explicadas, más como ya se explicó, esas direcciones son _privadas_. También hay direcciones IP publicas, las que permiten que redes se comuniquen entre ellas, y más aún, los recursos de internet guardados en servidores utilizan dichas IP públicas.

Cuando navegamos por un navegador no usamos direcciones IP, ya que:

1. _No tenemos la "memoria activada"_ en sólo recordar IPs de nuestros intereses, como por ejemplo el servidor de cientos de sitios web.
2. _Por seguridad_, ya que si se supieran todas las IPs de los servicios más importantes, sería un caos.
3. _Posible sobresaturación_, ya que el protocolo en versión 4 de internet tiene un límite de direcciones IP a asignar, así que variedad de protocolos previeven ese evento, como contramedida, está el protocolo IP versión 6, que tiene una infinidad más de IPs a asignar.

    - Formado por números enteros de 0 a 255 con un total de 32 bits, con 4 octectos (grupos de 8 bits, 4x8 = 32 bits). Es decir, cada número de 8 bits separados por un punto por el sistema binario tiene ese rango, 0-255.
    - El número máximo teórico para direcciones IPv4 es 2^32 = 4,294,967,296 que son las combinaciones únicas de los 32 bits.
    - A través de los años, el número de hosts se ha ido acercando más y más a 2^32, por los que fue necesario la creación de IPv6 para ello, que un profesor de la universidad nos decía que "tiene tantas direcciones IP como granos de arena hay",  2^128.

Para hacer de las direcciones IP "más claras de recordar" a los humanos, se tiene el procolo DNS **(Domain Name System)**, que permite a los usuarios usar nombres de dominio como "www.google.com" en lugar de direcciones IP numéricas como "74.125.19.147" para acceder a sitios web. Funciona como una especie de guía telefónica de Internet, traduciendo nombres de dominio a direcciones IP.

Además, en todas las redes, hay un dispositivo que "orquesta" la salidas y entradas de las comunicaciones de los hosts que pertencen a la red privada, comúnmente conocidos con _módems_ o formalmente como _routers_. Cada host tiene una conexíon con el router, y cada router tienes una conexión con tu ISP (Proveedor de Servicio de Internet), como Telmex Y Megacable en México, que son los que administran las IPs entre redes, entre ellas las IP públicas.

## localhost

Hay una dirección IP que hace referencia al propio host y que sirve para saber si los protocoles está configurados correctamente, se llama _localhost_ y tiene la IP 127.0.0.1. Cuando hosteamos un servidor web como NGINX o Apache, comúnmente navegando a "localhost" o "127.0.0.1" en el navegador se presenta el index de servidor web (su página HTML predeterminada).

## Ejemplo Práctico

Para la actividad, abre tu terminal de Kali Linux y ejecuta los siguientes comandos:

### ifconfig

Comando Muestra los datos de tu conexión a la red, como interfaces de red (ethernet, wifi), direcciones IP (versiones 4 y 6), y direcciones físicas (MAC). Las secciones de la salidas del comando nuestro interés son:

- **[interfaz][#]:**: Interfaz de red, las más comunes son por ethernet (eth) y conexión inalámbrica (WIFI).
- **inet**: Dirección IPv4.
- **inet6**: Dirección IPv6.

```sh
$> ifconfig
eth0: flags=4324<UP,BROADCAST,RUNNING,MULTICAST>    mtu 1500
    inet 10.0.2.15  netmask 255.255.255.0   broadcast 10.0.2.255
    inet6 ff80::f49:d443:a622:ee3d prefixlen 64 scopeid 0x20<link>
    ether 0a:08:4f:05:08:aa txqueuelen 1000 (Ethernet)
    ...
```

> Nota: _ifconfig_ es un comando de los shell de SOs basados en Linux. Su equivalente en Windows y su terminal "cmd" o "PowerShell" es **ipconfig**.

### nslookup

Es una herramienta (comando) de línea de comandos que se utiliza para realizar consultas DNS. Su función principal es averiguar la dirección IP de un dominio o encontrar el nombre de dominio asociado a una dirección IP. También se puede utilizar para verificar la configuración DNS de un dominio y diagnosticar problemas de red relacionados con DNS:

```sh
$> nslookup www.google.com
Server:     192.168.1.254
Address:    192.168.1.254#53

Non-authoritative answer:
Name:       www.google.com
Address:    192.178.57.36
Name:       www.google.com
Address:    2607:f8b0:4012:80a::2004
```

> Nota: _nslookup_ muestra IPs públicas.
> Nota: Para saber tu IP pública puedes buscar en tu motor de búsqueda "myip" y cualquier de los resultados te lo dirá.

### ping

La herramienta básica para saber sin hay conexión mediante el envió de paquetes muy pequeños (ping) y si hay conexión exitosa con el host indicado le retorna una respuesta similar (pong):

```sh
$> ping www.google.com
PING www.google.com (192.178.57.36) 56(84) bytes of data.
64 bytes from pnqroa-aj-in-f4-.1e100.net (192.178.57.36): icmp_seq=1 ttl=116 time=21.3
64 bytes from pnqroa-aj-in-f4-.1e100.net (192.178.57.36): icmp_seq=2 ttl=117 time=21.8
64 bytes from pnqroa-aj-in-f4-.1e100.net (192.178.57.36): icmp_seq=3 ttl=115 time=20.3
```

> Nota: Si pones atención y ejecutas uno tras otros _nslookup_ y _ping_ con el mismo host, hay una alta probabilidad de que la IP reportada en nslookup sea la que reporte el ping (en este caso de ejemplo 192.178.57.36).
