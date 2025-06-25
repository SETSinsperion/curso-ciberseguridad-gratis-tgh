# Directorios de Servidores Web

Un paso fundamental en los servidores web es saber su sistema de directorios, que en su mayoría es la notación UNIX, cuyo caracter separador es el _slash (/)_ para diferenciar entre carpetas y archivos. Los sitios y aplicaciones web son archivos web (HTML, JS, PHP, JSP, etcétera) que están almacenados en un directorio del servidor, exponiendo su contenido al internet, y puede que haya una vulnerabilidad u oportunidad de que este servidor web que nos pueda servir de acceso a la víctima.

> Nota: Aunque otros sistemas operativos como Windows utilizan la _barra invertida (\)_ como separador de rutas, la adopción de la barra diagonal en las URLs se mantuvo por compatibilidad y estandarización, ya que la mayoría de los navegadores y servidores web estaban basados en sistemas UNIX o sistemas derivados de este.

Vamos a ver 3 herramienta para descubrir los directorios de un host que están accesibles por los puertos del servicio (software) HTTP.

## Gobuster

Gobuster es una herramienta utilizada para realizar ataques de fuerza bruta contra: URI (directorios y archivos) en sitios web, subdominios DNS (con soporte de comodines), nombres de host virtuales en servidores web de destino, buckets abiertos de Amazon S3, buckets abiertos de Google Cloud y servidores TFTP.

Gobuster es útil para pentesters, hackers éticos y expertos forenses. También puede utilizarse para pruebas de seguridad.

Kali Linux tiene preinstalado este paquete, más por si las dudas puedes instalarlo por el instalador de paquetes _APT_.

> Nota: Los instaladores de paquetes son una parte crucial de los SOs basados en Linux, ya que administran las librerías para estandarizar la instalación del software que usamos en nuestros SOs. Algunos otros instaladores famosos son _yum, dnf, pacman, y zypper.

Vamos a partir por un ejemplo para mostrar los directorios de _Metasploitable2_:

1. Ve en tu terminal a la ruta en donde creamos _~/ms2_, en donde creamos los archivos como _users.txt_:

```sh
$> cd ~/ms2 && ls
scan_ms2_v.txt  users.txt
```

2. Vamos a usar el comando _gobuster_ con el comando de ayuda:

```sh
$> gobuster help                                  
Usage:
  gobuster [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  dir         Uses directory/file enumeration mode
  dns         Uses DNS subdomain enumeration mode
  fuzz        Uses fuzzing mode. Replaces the keyword FUZZ in the URL, Headers and the request body
  gcs         Uses gcs bucket enumeration mode
  help        Help about any command
  s3          Uses aws bucket enumeration mode
  tftp        Uses TFTP enumeration mode
  version     shows the current version
  vhost       Uses VHOST enumeration mode (you most probably want to use the IP address as the URL parameter)

Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
  -h, --help                  help for gobuster
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)

Use "gobuster [command] --help" for more information about a command.
```

2. Nos interesan el subcomando _dir_ para listar los directorios del dominio local o la IP de _Metasoploitable2_, así que también vamos a usar un diccionario de subdominios local con la opción _-w_:

```sh
$> gobuster dir -u http://ms2 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o scan_ms2_dir.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://ms2
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/index                (Status: 200) [Size: 891]
/test                 (Status: 301) [Size: 298] [--> http://ms2/test/]
/twiki                (Status: 301) [Size: 299] [--> http://ms2/twiki/]
/tikiwiki             (Status: 301) [Size: 302] [--> http://ms2/tikiwiki/]
/phpinfo              (Status: 200) [Size: 47876]
/server-status        (Status: 403) [Size: 289]
/phpMyAdmin           (Status: 301) [Size: 304] [--> http://ms2/phpMyAdmin/]
Progress: 220560 / 220561 (100.00%)
===============================================================
Finished
===============================================================
```

Donde:

- _gobuster_: Aplicación o comando principal.
- _dir_: Subcomando que le indica a _gobuster_ qué va a buscar (directorios/archivos).
- _-u_: URL objetivo del comando, donde vamos a investigar el puerto 80, http://ms2, o sea que la notación del socket no es necesaria (:80).
- _-w_: Ruta del archivo del diccionario de subdominios a usar.
- _-o_: Ruta del archivo en que quedará guardada la salida del comando, sólo van a aparecer en el archivo la lista de directorios.

3. Si recordamos, cuando entramos al index del servidor web de Metasploitable2 tenía links a algunos de los directorios listados en las salidas, pero algunos directorio no estas en los links de la página web, como _/test, /phpinfo y /server-status_. Vamos a ver en el navegador qué esconden esos 2 directorios:

- **http://ms2/test/testoutput/ESAPI_logging_file_test**: Un archivo de 0 bytes.
- **http://ms2/server-status**: Una página _prohibida_ del acceso a usuarios normales.
- **http://ms2/phpinfo**: ¡Bingo! Una página que contiene datos del servidor relativos a PHP y sus módulos.

Ya vimos que con _gobuster_ + un wordlist de subdominios puedes listar los directorios que se encuentran en el puerto 80.

> Nota: Cuando usamos estos comandos + wordlists, esta técnica es conocida como _dirbusting_ (de ahí términos como _dir bust, dirbust o dirbuste_), es una de las formas más efectivas de descubrir contenido oculto en servidores.
> Nota: Para filtrar los resultados por extensión del archivo, usamos la bandera y con las extensiones _sin el punto_ separador por comas si estas por filtrar por más de un archivo: _-x php,html,js_.

## Dirbuster

DirBuster es una aplicación _Java_ multi hilo con interfaz gráfica que tiene como objetivo obtener, mediante la técnica de fuerza bruta, los nombres de directorios y archivos en servidores web que está disponibles a través de un puerto.

> Nota: _Java_ es un lenguaje de programación multiplataforma, y su principal contribución al mundo del software es la maquina virtual para la compilación y ejecución de los programas escrito en este lenguaje que se puede instalar en casi cualquier dispositivo, no sólo en computadoras.

Vamos a usarlo en la verificación de los directorios del otro puerto abierto de tipo HTTP, el _8180_:

1. Ejecuta el comando _dirbuster_ en la terminal, se abrirá una ventana con el programa.

```sh
$> dirbuster
```

> Nota: Cuando se cambia de SOs viendo de Windows, habrá veces que los softwares instalados _no generan un acceso directo para usar el programa_, pero sí generan un comando para iniciarlo, o si no lo genero, pueden generar archivo _*.run, *.sh_ o cualquier ejecutable (excepto el _*.exe_, ese es de Microsoft). Para ejecutarlos, arrastra el archivo a la terminal para que te dé la ruta completa del archivo, presionas _Enter_ y listo.

2. _dirbuster_ es como _gobuster_, pero con interfaz gráfica, vamos a configurar la búsqueda de directorio con los valores siguientes:

- Target URL: _http://ms2:8180_.
- File with list of dirs/files: _el mismo que usamos con gobuster_.
- _Brute Force Dirs, Brute Force Files y Use Blank Extension_ con el check listo.
- Dir to start with: / (el root del servidor).
- File Extension: Déjalo en blanco.

Haz clic en el botón de _Start_ y espera a que la búsqueda se complete.

> Nota: Podría surgir una pantalla de error durante la búsqueda, diciendo que han ocurrido más de 20 errores y que por eso se ha detenido el programa, intenta haz clic en _OK_ y hacer clic en el botón _Pause_ y continuará la búsqueda. En la terminal en donde ejecutó _dirbuster_ habrá mensajes en donde dice que hizo _conexiones rechazadas por el servidor_.

3. Cuando acabe la búsqueda, habrá 2 pestañas de Resultados:

- _Resultados por List View_: En donde verás tanto los directorios como los archivos encontrados en una tabla.
- _Resultados por Tree View_: La lista de directorios y archivos que encontró por un árbol de nodos que cada uno es una carpeta.

Y una nota muy importante ...

> Nota: DirBuster es un proyecto de **OWASP (Open Web Application Security Project)**, lo que significa que es una herramienta desarrollada y mantenida por la comunidad, es una organización sin fines de lucro dedicada a mejorar la seguridad del software. Su enfoque principal es la seguridad de aplicaciones web, ofreciendo recursos como el OWASP Top 10, una lista de las principales vulnerabilidades web, y otras herramientas y guías para ayudar a desarrolladores y profesionales de seguridad a crear aplicaciones más seguras.

## Dirb

DIRB es un escáner de contenido web. Busca objetos web existentes (u ocultos). Funciona básicamente lanzando un ataque de diccionario contra un servidor web y analizando las respuestas. Incluye un wordlist de ataque preconfigurado para facilitar su uso, pero puedes usar tus propios diccionarios. DIRB también puede usarse a veces como un escáner CGI clásico, **pero recuerda que es un escáner de contenido, no de vulnerabilidades**.

Para usar con el puerto 80 de Metasploitable2, vamos a realizar lo siguiente en una terminal:

1. Ejecutar el comando _dirb_ con la ayuda de sus opciones:

```sh
$> dirb

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

dirb <url_base> [<wordlist_file(s)>] [options]

========================= NOTES =========================
 <url_base> : Base URL to scan. (Use -resume for session resuming)
 <wordlist_file(s)> : List of wordfiles. (wordfile1,wordfile2,wordfile3...)

======================== HOTKEYS ========================
 'n' -> Go to next directory.
 'q' -> Stop scan. (Saving state for resume)
 'r' -> Remaining scan stats.

======================== OPTIONS ========================
 -a <agent_string> : Specify your custom USER_AGENT.
 -b : Use path as is.
 -c <cookie_string> : Set a cookie for the HTTP request.
 -E <certificate> : path to the client certificate.
 -f : Fine tunning of NOT_FOUND (404) detection.
 -H <header_string> : Add a custom header to the HTTP request.
 -i : Use case-insensitive search.
 -l : Print "Location" header when found.
 -N <nf_code>: Ignore responses with this HTTP code.
 -o <output_file> : Save output to disk.
 -p <proxy[:port]> : Use this proxy. (Default port is 1080)
 -P <proxy_username:proxy_password> : Proxy Authentication.
 -r : Don't search recursively.
 -R : Interactive recursion. (Asks for each directory)
 -S : Silent Mode. Don't show tested words. (For dumb terminals)
 -t : Don't force an ending '/' on URLs.
 -u <username:password> : HTTP Authentication.
 -v : Show also NOT_FOUND pages.
 -w : Don't stop on WARNING messages.
 -X <extensions> / -x <exts_file> : Append each word with this extensions.
 -z <millisecs> : Add a milliseconds delay to not cause excessive Flood.

======================== EXAMPLES =======================
 dirb http://url/directory/ (Simple Test)
 dirb http://url/ -X .html (Test files with '.html' extension)
 dirb http://url/ /usr/share/dirb/wordlists/vulns/apache.txt (Test with apache.txt wordlist)
 dirb https://secure_url/ (Simple Test with SSL)
```

> Nota: Comúnmente cuando ejecutas un comando sin opciones de configuración, te manda la lista de opciones para tu ayuda.

2. Vamos a "hacerle caso" a la línea de la ayuda _dirb <url_base> [<wordlist_file(s)>] [options]_ para encontrar los recursos de directorios y archivos con una wordlist dedicada para esa aplicación, es decir, hay una carpeta llamada _/usr/share/wordlists/dirb_:

```sh
$> dirb http://ms2:8180 /usr/share/wordlists/dirb/small.txt -X php

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Wed Jun 18 20:25:50 2025
URL_BASE: http://ms2/
WORDLIST_FILES: /usr/share/wordlists/dirb/small.txt
EXTENSIONS_LIST: (php) | (php) [NUM = 1]

-----------------

GENERATED WORDS: 959                                                           

---- Scanning URL: http://ms2/ ----
+ http://ms2/cgi-bin/php (CODE:500|SIZE:613)                                                                                                                
                                                                                                                                                            
-----------------
END_TIME: Wed Jun 18 20:25:54 2025
DOWNLOADED: 959 - FOUND: 1
```

Encontró sólo un resultado, si visitamos esa URL nos dirá que esta prohibida, por lo que sí hay algunos recursos que el servidor sí protege públicamente.

## Reflexión final

Un refrán famoso reza que _la información es poder_, y si nosotros queremos realizar auditorías y pruebas exitosas para descubrir vulnerabilidades en una red, debemos de aprender detalles del software a buscar en los servidores web, por eso _Apache, Tomcat_, o el árbol de directorios UNIX que hemos visto en esta lección.

Todo en los SOs que usamos en el día a día está organizado mediante carpetas y archivos, y con las 3 herramientas agregadas a tu arsenal del hacking ético puedes revelar los directorios y archivos que están disponibles a tráves del puerto 80 o cualquiera que utilize servicios HTTP (por ejemplo, un software que ocupa el puerto 8000 que es una estructura de un API).

**Cualquier software o herramienta no es perfecto**, y eso lo vimos con la práctica de _dirbuster_ que tuvimos que hacer clic en OK varias veces para que continuara la búsqueda y su progreso, hay otros programas que una vez que encuentran un error se cierran _completamente_ o se _congelan_, y eso es lo que pasa cuando el programa procesó una instrucción no concebida al momento de su creación, a esos errores se les conoce con _excepciones_. La comunidad de OWASP y de Open Source encuentra esas excepciones y con la ayuda de desarrolladores encuentran una solución o un _workaround_ (una propuesta que no es exactamente la solución pero te da un resultado esperado), y a veces es por _"el amor al arte"_, no recibiendo una compensación económica por esa horas dedicadas a la solución de estos errores, por eso esta es una oportunidad para decir _GRACIAS_ a todos estos "héroes" que lo hacen para mejorar sus programas más allá de la compensación.
