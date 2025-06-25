# Diccionarios

Los diccionarios o _wordlists_ son archivos y bases de datos que contienen caracteres / series de contraseñas comunes (y aleatorias), subdominios, etcétera, que son usados en los ataque de _Fuerza Bruta_. Vamos a ver los wordlists locales que viene en Kali Linux y en dónde descargar los wordlists más comunes en la web.

## _Wordlists_ locales

Como sabemos, Kali Linux es una distro enfocada a la ciberseguridad, y como tal ya tiene diccionarios precargados para su utilización. Para verlos, ejecutamos el siguiente comando:

```sh
$> wordlists

> wordlists ~ Contains the rockyou wordlist

/usr/share/wordlists
├── amass -> /usr/share/amass/wordlists
├── dirb -> /usr/share/dirb/wordlists
├── dirbuster -> /usr/share/dirbuster/wordlists
├── dnsmap.txt -> /usr/share/dnsmap/wordlist_TLAs.txt
├── fasttrack.txt -> /usr/share/set/src/fasttrack/wordlist.txt
├── fern-wifi -> /usr/share/fern-wifi-cracker/extras/wordlists
├── john.lst -> /usr/share/john/password.lst
├── legion -> /usr/share/legion/wordlists
├── metasploit -> /usr/share/metasploit-framework/data/wordlists
├── nmap.lst -> /usr/share/nmap/nselib/data/passwords.lst
├── rockyou.txt.gz
├── sqlmap.txt -> /usr/share/sqlmap/data/txt/wordlist.txt
├── wfuzz -> /usr/share/wfuzz/wordlist
└── wifite.txt -> /usr/share/dict/wordlist-probable.txt

Do you want to extract the wordlist rockyou.txt? [Y/n]
```

Como verás, este comando muestra el contenido de _/usr/share/wordlists_, y al final de la salida pregunta si queremos extraer el contenido de _rockyou.txt.gz_, una de las wordlists más "conocidas" en el mundo de la ciberseguridad. Responde que _no (n)_ porque vamos a aprender cómo extraer manualmente este tipo de archivos comprimidos desde la terminal.

> Nota: _/usr/share/wordlists_ contiene directorios de herramientas como _Metasploit_, los cuales contienes wordlists específicos que se usan con la herramienta propia. Entra a las carpetas que te interesen y lo verás.

2. Navega hasta el path _/usr/share/wordlists_, copia el archivo a tu carpeta de usuario (recuerda que es _~_), ejecuta el comando para descomprimir el archivo y cuenta cuantas contraseñas tiene esa wordlist:

```sh
$> cd /usr/share/wordlists
$> ls
amass  dirbuster   fasttrack.txt  john.lst  metasploit  rockyou.txt.gz  wfuzz
dirb   dnsmap.txt  fern-wifi      legion    nmap.lst    sqlmap.txt      wifite.txt
$> cp rockyou.txt.gz ~    
$> cd ~                   
$> ls
Desktop  Documents  Downloads  go  ms2  Music  Pictures  Public  rockyou.txt.gz  Templates  Videos
$> gunzip -d rockyou.txt.gz                       
$> ls
Desktop  Documents  Downloads  go  ms2  Music  Pictures  Public  rockyou.txt  Templates  Videos
$>wc -l rockyou.txt                                                                                    14344392 rockyou.txt
```

- _cp_ es el comando para copiar un archivo en la terminal.
- _gunzip_ es la aplicación para descomprimir un _*.gz_, es similar a _Winrar_ con los archivos _*.rar o *.zip_.
- Hay _1,434,4392_ de contraseñas en el archivo _rockyou.txt_, entre más contraseñas mayor probabilidad será que el ataque de fuerza bruta sea exitoso.

## Descargar wordlists

Aquí está una lista de URLs para descargar diccionarios útiles para tus laboratorios de ciberseguridad:

1. https://github.com/ignis-sec/Pwdb-Public

- Este es un repositorio de GitHub que tiene cientos de miles de contraseñas en sus archivos de wordlists obtenidas de base de datos filtradas (powned). Cabe destacar que antes el creador del repo se llamaba _FlameOfIgnis_ y ahora se llama _ignis-sec_.

2. https://crackstation.net/crackstation-wordlist-password-cracking-dictionary.htm

- Sitio en donde podemos encontrar 2 diccionarios: una lista de 15 GB y otra de 648 MB. Se pueden descargar tanto en formato *.gz como por medio de _torrent_ (archivo para la descarga directa de recursos por el protocolo _P2P_).

3. https://weakpass.com/download

- Sitio que contiene links de descarga de wordlists para diferentes usos, los podemos encontrar con el formato _*.7z (otro formato comprimido,)_ o torrent.

> Nota: Para descomprimir los archivos _*.7z_ (ya que gunzip está solamente diseñado para archivos *.gz), instala el siguiente paquete _sudo apt-get install p7zip-full_ y ejecuta _7z x archivo.7z_.

4. https://github.com/danielmiessler/SecLists

- Repositorio de GitHub que contiene directorios de wordlists clasificado para su uso (passwords, fuzzing, etcétera), además contiene un paquete especial para Kali Linux: _apt -y install seclists_.

# Usar wordlists con Hydra

_Hydra_ es una herramienta de ataques de fuerza bruta a servicios con FTP y SSH. Una vez que obtengamos el archivo _rockyou.txt_, vamos a usarlo con esta herramienta:

1. Instala los siguientes paquetes, uno es hydra o otro es kali-tweaks, un paquete que añade compatibilidades a los servicios de Kali Linux.

```sh
$> apt install hydra kali-tweaks
```

> Nota: En las versión actuales de Kali Linux, ya vienen preinstaladas.

2. En el directorio en donde descomprimiste _rockyou.txt_, ejecuta el siguiente comando:

```sh
$> hydra -L ms2/users.txt -P rockyou.txt -t 4 ms2 ssh
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-06-16 19:34:48
[DATA] max 4 tasks per 1 server, overall 4 tasks, 100410793 login tries (l:7/p:14344399), ~25102699 tries per task
[DATA] attacking ssh://ms2:22/
[ERROR] could not connect to ssh://10.0.2.5:22 - kex error : no match for method server host key algo: server [ssh-rsa,ssh-dss], client [ssh-ed25519,ecdsa-sha2-nistp521,ecdsa-sha2-nistp384,ecdsa-sha2-nistp256,sk-ssh-ed25519@openssh.com,sk-ecdsa-sha2-nistp256@openssh.com,rsa-sha2-512,rsa-sha2-256]
```

Observamos que _hydra_ no pudo ejecutar el ataque, debido a que su software de SSH no cuenta con compatibilidad de seguridad _ssh-rsa_, lo cual nos pasó cuando intentamos hacer la conexión SSH manualmente y tuvimos que agregar una bandera para la retrocompatibilidad.

3. Para añadir compatibildad completa con los algoritmos de seguridad de SSH, vamos a ejecutar _kali-tweaks_, y no aparece una interfaz "grafica" pero que se maneja con el teclado (muy curioso y este fue por muchos años las interfaces de instalación de SOs basados en Linux). Nos vamos a la primera opción _Hardening_, despues con las teclas de dirección nos vamos a _SSH Client_ y lo seleccionamos con la tecla de barra espaciadora, nos aparece una pantalla de confirmación y le damos _Apply_, y en la terminal nos indica que presiones _Enter_ para volver al menú, y le damos a _Quit_.

4. Ahora vuelve a ejecutar el comando de _hydra_, y el ataque ha iniciado. Una vez que vayan apareciendo unos cuantos de los mensajes de _STATUS_, interrumpe el comando con _Ctrl + C_:

```sh
$> hydra -L ms2/users.txt -P rockyou.txt ms2 -t 4 ssh
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-06-16 20:04:35
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 4 tasks per 1 server, overall 4 tasks, 100410793 login tries (l:7/p:14344399), ~25102699 tries per task
[DATA] attacking ssh://ms2:22/
[STATUS] 100.00 tries/min, 100 tries in 00:01h, 100410693 to do in 16735:07h, 4 active
[STATUS] 98.67 tries/min, 296 tries in 00:03h, 100410497 to do in 16961:14h, 4 active
[STATUS] 101.29 tries/min, 709 tries in 00:07h, 100410084 to do in 16522:35h, 4 active
[STATUS] 102.13 tries/min, 1532 tries in 00:15h, 100409261 to do in 16385:20h, 4 active
^X@sS^CThe session file ./hydra.restore was written. Type "hydra -R" to resume session.
```

Entenderás el _por qué_ tarda mucho los ataques de fuerza bruta ...

- Teniendo en cuenta que en los archivos de users.txt hay 7 usuarios y más de 1.4 millones de contraseñas en el archivo de _rockyou.txt_ (100,410,793 intentos), las combinaciones entre los 2 archivos son muchísimas, y como a _hydra_ con 4 hilos (4 intentos de conexión a SSH a la vez), los STATUS nos dicen que va a una velocidad de 100 intentos _por minuto_.
- Los ataque de este tipo no sólo dependerán de nuestro hardware en el que correo nuestro Kali Linux, agregando:
  - Velocidad de conexión a internet.
  - Prestaciones del host víctima respecto al servicio a atacar, por eso algunos ataques que usamos _hydra para no llenar de peticiones de conexión a SSH nos pide que pongamos valor a la opción de -t_.

Y en este tipo de ataques, es un recordatorio de que _la paciencia es una virtud_.

> Nota: La bandera de hydra _-R_ permite reanudar el ataque, es útil para quienes pausaron la ejecución pero quieren retomarla más adelante.
> Nota: La opción de hydra _-w_, que permite ajustar el tiempo de espera en segundos entre intentos para evitar bloqueos de sesión.

## Reflexión final

Para los ataques de fuerza bruta con el objetivo de obtener acceso a un host víctima, necesitamos listas de contraseñas para _probar_ credenciales _usuario-password_, debido a que nosotros como seres humanos tendemos a reutilizar información, y las contraseñas no son la excepción. Por esa razón, los ciber-criminales hacen esfuerzos para utilizar bases de datos con las credenciales que se encuentran en los diccionarios o _wordlists_.

Recuerda que como mínimo hay 2 caras de la moneda, siendo la otra cara de la ciberseguridad los _white-collar hackers_ o _profesionales TI de ciberseguridad_, siendo estos los que utilizan estos conocimientos para la **defensa y prevención** de sus sistemas informáticos.

Otro recordatorio es usar los _Password Managers_ para almacenar y generar contraseñas que es muy improbable que estén en las wordlists que hemos visto aquí.
