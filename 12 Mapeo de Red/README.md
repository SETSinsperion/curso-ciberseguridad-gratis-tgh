# Mapeo de Redes

En esta lección veremos otra herramienta de la terminal de Kali Linux (y de cuál SO basado en Linux) para realizar mapeos de redes como lo hacemos con _arp-scan_, mapeando qué direcciones IP de los hosts tenían una dirección MAC específica.

## NMAP (Network Mapping)

El comando _nmap_ es una poderosa herramienta de mapeo de redes, y viene por defecto en cualquier terminal de los SOs basados en Linux. Vamos a ver los casos básicos y su uso.

## Buscando un objetivo ...

1. Vamos a utilizar 2 máquinas virtuales:

- Kali Linux.
- Metasploitable2.

2. Asegúrate de que ambas MVs sea configuradas con el mismo tipo de Adaptador de Red en el apartado de Network, YA SEA nat O Red NAT (una red personalizada para ambas MVs):

- VirtualBox: _Settings > Network > Adaptador de Red = NAT o Red NAT_.
- VMWare: _Settings > Network > Adaptador de red NAT_ o _Custom > VMnet#_.

3. Inicia las 2 MVs.
4. En la MV de Kali Linux, una vez hayas iniciado sesión, encuenta la dirección IP, la puerta de enlace y la interfaz de tu Kali Linux con el siguiente comando,

```sh
$> ip route show
default via 10.0.2.1 dev eth0 proto dhcp src 10.0.2.15 metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0..2.15 metric 100
```

La primera línea de la salida es la que nos interesa:
Interfaz (dev): _eth0_.
Tu IP (src): _10.0.2.15_.
Default Gateway (deafult via): _10.0.2.1_.
Máscara de red: _/24 (255.255.255.0)_.

> Nota: _ifconfig_ no da la IP de puerta de enlace. La opción de _ip route show_ muestra más información de las IPs que sera de utilidad para encontrar la IP del host objetivo.

5. Con los datos recolectados, vamos a ejecutar _arp-scan_ con el objetivo de encontrar la IP del host de _Metasploitables2_:

```sh
$> arp-scan -I eth0 10.0.2.15/24
```

> Nota: _-I_ es igual a la opción que aprendiste como _--interface_.
> Recuerda el concepto de máscara de red (/24), que en este caso significa que _sólo son asignables las IPs del 1-254_. A esta notación IP/MASK se le llama CIDR (Classless Inter-Domain Routing).
> En este caso, he usado la IP de Kali Linux para ejecutar el escaneo, pero con cualquier IP que esté asignada funciona.

6. Discrimina las direcciones IP encontradas por el escaneo ARP para encontrar el host de Metasploitable2:

- Tu IP que encontraste en el paso 4 no es.
- La IP de la Puerta de Enlace o Default Gateway no es.

A lo sumo, te quedarán 2 o 3 IPs sin la IP de tu puerta de enlance, vamos a investigar más para determinar cuál de estas IPs es nuestro objetivo.

## Mapeando tu red a partir de tu IP

Para ello, usaremos _nmap_ para detectar los SOs que están en los hosts que están en tu red:

```sh
$> nmap 10.0.2.15/24 -O
```

La salida del comando es muy extensa, para iniciar tu análisis, sigue estas instrucciones:

- Cada reporte individual de un host es iniciado por la linea _Nmap scan report for [IP]_, ejemplo mi salida han salido 5 hosts:

  - 10.0.2.1 (puerta de enlance).
  - 10.0.2.2 (?).
  - 10.0.2.3 (?).
  - 10.0.2.5 (?).
  - 10.0.2.15 (mi IP).

  Por tanto, hay 3 IP que pueden ser el host de _Metasploitable2_.

> Nota: Si no quieres hacer mapeos de puertos mapeando la red y obteniendo información básica como la dirección física de los hosts, hay que agregar la opción _-sn_ de _nmap_:

```sh
$> nmap 10.0.2.5/24 -sn
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-08 22:05 EDT
Nmap scan report for 10.0.2.1
Host is up (0.00049s latency).
MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)
Nmap scan report for 10.0.2.2
Host is up (0.00039s latency).
MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)
Nmap scan report for 10.0.2.3
Host is up (0.00032s latency).
MAC Address: 08:00:27:E1:09:60 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Nmap scan report for 10.0.2.5
Host is up (0.0011s latency).
MAC Address: 08:00:27:DE:69:12 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Nmap scan report for 10.0.2.15
Host is up.
Nmap done: 256 IP addresses (5 hosts up) scanned in 2.05 seconds
```

- Ahora lee cuál son los SOs de los hosts, en la salida _OS details_:

  - 10.0.2.2 (?) = _No exact OS matches for host_ (se dan una serie de posibles SOs, como _OracleVirtualBox lwIP NAT Bridge (89%)_).
  - 10.0.2.3 (?) = No hay la línea de _OS details_, pero si hay la línea de _vendor_ de la MAC, _PCS Sytemtechnik/Oracle VirtualBox virtual NIC_.
  - **10.0.2.5 (?) = OS details: Linux 2.6.9**.

  Los host que corren sistemas basados en Linux se presentan así, y como los otros 2 hosts no tenemos un OS especificado al 100% vamos a _asumir_ que _10.0.2.5_ es el objetivo

> Nota: _nmap 10.0.2.15/24 -O_ es como si le estuvieras diciendo a nmap "Detecta todos los host de la red 10.0.2.X", ya que la máscara, /24, indica eso.

## Mapeando los puertos de un host específico

Vamos a detectar qué puertos (puntos de conexión) está usando la IP que detectamos que es nuestro posible objetivo:

```sh
$> nmap 10.0.2.5
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-08 21:53 EDT
Nmap scan report for 10.0.2.5
Host is up (0.0012s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2049/tcp open  nfs
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
5900/tcp open  vnc
6000/tcp open  X11
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown
MAC Address: 08:00:27:DE:69:12 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 0.33 seconds
```

El comando detecta al host que tiene la IP _10.0.2.5_, y _detecta los estados de todos los puertos bien conocidos_, 977 no abiertos y 23 abiertos

## Examinando los puertos abiertos

El host con la IP _10.0.2.5_ es un host basado en Linux, y como en la anterior salida se detectaron 23 puertos abiertos, hay uno que es de nuestro especial interés:

```sh
PORT     STATE SERVICE
80/tcp   open  http
```

Por lo tanto, suponemos a partir de ese puerto que hay montado un servidor web en ese host, y para comprobarlo, hay que navegar en Kali Linux al sitio _10.0.2.5_ o _http://10.0.2.5_ (como en una computadora que instalamos un servidor web visitando _localhost_): veremos que nuestra suposición estuvo correcta, hemos encontrado el host de _Metasploitable2_.

El host en el index de su servidor web tiene varios links que podemos ver al momento de entrar a 10.0.2.5 en nuestro navegador:

- TWiki (web-based collaboration platform).
- phpMyAdmin (GUI del motor de base de datos MySQL).
- Multillidae (otro sitio web disponible en el servidor).
- DVWA (una app web).
- WebDAV (directorio vacío del servidor).

##  Verificando rangos de puertos

Si verificamos un rango:

```sh
$> nmap 10.0.2.5 -p 1-65535
```

El comando anterior puede ser escrito de esta forma con la bandera de _todos los puertos_:

```sh
$> nmap 10.0.2.5 -p-
```

Para verificar uno o varios puertos en específico:

```sh
$> nmap 10.0.2.5 -p 80
$> nmap 10.0.2.5 -p 80,443,22
```

## Usando scripts

_nmap_ tiene scripts para casos de información específica en lugar de teclear las opciones manualmente del comando. Hay un script predeterminado, para ejecutarlo usamos la opción _-sC_:

```sh
$> nmap 10.0.2.5 -p 80 -sC
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-09 00:40 EDT
Nmap scan report for 10.0.2.5
Host is up (0.0023s latency).

PORT   STATE SERVICE
80/tcp open  http
|_http-title: Metasploitable2 - Linux
MAC Address: 08:00:27:DE:69:12 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 1.38 seconds
```

Más adelante veremos otros scripts.

## Ver las versiones de software que ocupa un puerto

```sh
$> nmap 10.0.2.5 -p 80 -sV
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-09 00:43 EDT
Nmap scan report for 10.0.2.5
Host is up (0.0018s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.2.8 ((Ubuntu) DAV/2)
MAC Address: 08:00:27:DE:69:12 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.61 seconds
```

En el host _Metasploitable2_ el puerto 80 es ocupado por un servidor _Apache httpd 2.2.8 ((Ubuntu) DAV/2)_.

## Agresividad

La "agresividad" o intensidad del escaneo se controla mediante opciones que determinan la velocidad y la cantidad de información que se recopila. _El escaneo agresivo_, activado con la opción -A, combina diversas técnicas para una exploración más profunda y rápida, pero también con mayor probabilidad de ser detectado.

```sh
$> nmap 10.0.2.5 -A
```

Lo que es igual a usar las opciones _-sC, -sV, -O y --traceroute (la ruta que toman los paquetes de datos entre el escaneador y el objetivo)_.

## Temporización (_timing_)

Controla la velocidad de los escaneos, permitiendo un equilibrio entre rapidez y sigilo. Puedes usar plantillas predefinidas (como -T0 a -T5) o ajustar manualmente los parámetros para optimizar el escaneo a tu objetivo específico:

```sh
$> nmap 10.0.2.5 -T3
```

Donde las plantillas predefinidas son:

- **T0**: Escaneo lento y sigiloso, útil para evadir detección.
- **T1**: Escaneo más rápido que -T0.
- **T2**: Escaneo moderado.
- **T3**: Escaneo relativamente rápido, adecuado para la mayoría de los casos.
- **T4**: Escaneo rápido (**DEFAULT**).
- **T5**: Escaneo muy rápido, potencialmente agresivo.

Estas plantillas tiene valores para opciones como _--scan-delay_ o _--max-retries_

_Más adelante haremos uso de este acceso que encontramos._
