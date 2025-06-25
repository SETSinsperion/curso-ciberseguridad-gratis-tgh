# Direccioness MAC

Las direcciones MAC o físicas son identificadores únicos conformados por 48 bits (6 octectos) para las tarjetas de red que es lo que permite que un host se conecte a una red. Un ejemplo de una dirección MAC es:

**01:3A:1D**:**54:6B:32**

las cantidades no son números enteros decimales (base 10), sino hexadecimales (base 16), es decir, c/uno de los caracteres de una dirección física puede ser:

- _Valor decimal:_
- **0 1   2   3   4   5   6   7   8   9   10  11  12  13  14  15**
- _Valor hexadecimal:_
- **0 1   2   3   4   5   6   7   8   9   A   B   C   D   E   F**
(16 caracteres)

## Obtener una dirección MAC de propio host

Simplemente en Kali Linux para obtener tu MAC usa _ifconfig_, y encuentra el valor _ether_:

```sh
$> ifconfig
eth0: flags=4324<UP,BROADCAST,RUNNING,MULTICAST>    mtu 1500
    inet 10.0.2.15  netmask 255.255.255.0   broadcast 10.0.2.255
    inet6 ff80::f49:d443:a622:ee3d prefixlen 64 scopeid 0x20<link>
    ether 0a:08:4f:05:08:aa txqueuelen 1000 (Ethernet)
    ...
```

Para otras terminales como en el _cmd_ de Windows cambia el comando.

## Obtener dirercciones MAC de hosts en la red

Para ello, necesitarás aprender uno que otro concepto de redes para comprender el comando a usar:

- **ARP:** o _Address Resolution Protocol_, es un protocolo de red fundamental en las redes locales que permite traducir direcciones IP (lógicas) a direcciones MAC (físicas) para facilitar la comunicación entre dispositivos. Permite a los dispositivos encontrar la ubicación física de otros en la red local.
- **Máscara de Red:** Cada IP es única en una red, pero para distinguir redes tenemos a la Máscara de Red, que es un conjunto de bit encendidos (una IP como tal con todos los bits en 1 de izquierda a derecha), comúnmente descritos a la derecha de una IP, ejemplo 192.168.1.24/24 (/24 es la abreviatura de la máscara), lo que indica que su máscara es **255.255.255.0 = 11111111.11111111.11111111.00000000** (24 bits en 1). Lo que indica la máscara es _que tanto bits son usados para asignar IPs en una red_ (los ceros a la derecha). Por lo tanto, la máscara de ejemplo identifica a una red que tiene como máximo 255 direcciones IP a asignar, de **192.168.1.1** a **192.168.1.255**.
- **Broadcast:** Es una dirección especial que se utiliza en las redes para enviar paquetes de datos a todos los dispositivos en una red sin necesidad de conocer la dirección IP individual de cada host. Es como una dirección de grupo, donde todos los dispositivos de la red están conectados y pueden recibir la información. _Es la última dirección IP disponible en esa red_, en el ejemplo anterior, **192.168.1.255**.
- **Puerta de Enlace:** O _Default Gateway_, es la dirección IP del router o módem que es el dispositivo del control de tráfico de información de la red. Como práctica común, es la última o primera dirección IP asignable.
- **Interfaz de Red:** Es un punto de conexión entre un dispositivo y una red, que permite la comunicación de datos entre ellos. Puede ser física, como una tarjeta de red, o lógica. Puedes reconocer las interfaces con el comando _ifconfig_ como cable Ethernet (eth) y tarjeta WIFI (wlan).

Ahora, podrás comprender el siguiente comando en Kali Linux:

### arp-scan

Escanea todos los hosts de una red reportando los pares IP - MAC  por el protocolo ARP.

```sh
$> sudo arp-scan --interface=eth0 192.168.1.1/24
Interface: eth0, datalink type: EN10MB (Ethernet)
Starting arp-scan 1.9 with 256 hosts (http://www.nta-monitor.com/tools/arp-scan/)
192.168.1.1 00:50:56:c0:00:08 VMware, Inc.
192.168.1.2 00:50:56:f1:18:a8 VMware, Inc.
192.168.1.254 00:50:56:e5:7b:87 VMware, Inc.

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.9: 256 hosts scanned in 2.327 seconds (110.01 hosts/sec). 3 responded
```

> Nota: Como se ve en el ejemplo de salida del comando, por c/MAC se identificó un fabricante, esto en información el comando "consultó con c/host" un archivo con los datos de la interfaz.
> Nota: Con los comandos ARP **no todas las MAC son reportadas** "realmente", ya que en los smartphones se encuentra las _Direcciones MAC aleatorias_ o privadas. En _arp-scan_ esta MAC son reconocidas por _Unknown: Locally administered_ en lugar del fabricante.

## Usos

El _router_ debe de saber las direcciones físicas de los dispositivos para controlar el flujo o _tráfico_ de información de la red, _qué host estás enviando paquetes de infomación o recibiendo_.

Los primeros 3 octectos indican el OUI (Identificador Únicos del Fabricante) y los últimos 3 el UAA (Identificador del Producto). Es muy importante que en las audítorías de seguridad que el auditor o el hacker en los ataques conozca la dirección física, ya que así _conoce a qué marca de equipo se enfrenta_ y así hacer discernimiento entre qué estrategias sí funciona o no contra la víctima (el host atacado).

Para buscar el OUI o fabricante de una tarjeta de red, puedes ir por ejemplo a https://macvendors.com y pegar una MAC para saberlo. Ejemplo, si buscar la MAC **08:00:27:b4:a1:05** te saldrá el fabricante _PCS Systemtechnik GmbH_, empresa alemana de soluciones de hardware y software para control de acceso, videovigilancia, registro de presencia de recopilación de producción. Toda esa información la obtuve por sólo saber la MAC y una búsqueda rápida del fabricante.

## Cambiar una dirección física

El comando **macchanger** permite cambiar tu dirección física a tu tarjeta de red. Vamos a realizar los siguientes pasos para aprender su uso:

1. Ejecuta _sudo su_ para evitar usar _sudo_ a los comandos subsecuentes.
2. Actualizar tanto tus repositorios y paquetes de Kali Linux: _apt update && apt dist-upgrade -y && apt autoremove_

> Nota: Concatenando _&&_ puedes ejecutar más de un comando en un solo prompt.

3. Usa _ifconfig_ para ver tu MAC actual y la interfaz de ésta, ejemplo _eth0_.
4. Usa _macchanger -s [interfaz]_: Muestra tanto tu MAC como la cambiada por el comando.
5. Usa _macchanger -e [interfaz]_: Cambiar tu MAC sin alterar el fabricante.
6. Usa _macchanger -a [interfaz]_: Cambiar tu MAC en la parte del fabricante con un proveedor del mismo tipo.
7. Usa _macchanger -r [interfaz]_ Cambiar tu MAC por una aleatoria, posiblemente no tenga registrada un fabricante registrado.
8. Usa _macchanger -m XX:XX:XX:XX:XX:XX [interfaz]_: Establece  la MAC a XX:XX:XX:XX:XX:XX.
9. Ahora usa _macchanger -p [interfaz]_: Resetea a la MAC de fábrica (permanente).

Al cambio de una dirección MAC se llama **_MAC spoofing_**, y un uso puede ser el _Saltar las listas de dispositivos permitidos o prohibidos almacenadas en los router_. Las _white-list_ y _black-list_ son mecanismos de protección de una red que se encuentran en los router, permitiendo o no el acceso al tráfico de la red proveniente de un host identificado por su MAC. Si sabemos que nuestro host no está permitido para acceder a internet, cambiando la MAC se puede "burlar" esta protección.
