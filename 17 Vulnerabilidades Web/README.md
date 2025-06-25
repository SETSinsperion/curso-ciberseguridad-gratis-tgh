# Vulnerabilidades Web

Las vulnerabilidades web abarcan tanto clientes (nosotros como usuario de internet o de una red) como servidores (computadores superpotentes que almacenan los recursos de la red). Vamos a realizar un ejercicio en donde nuestro host víctima, _la MV de Metasploitable2_, tienes servicio web que no están bien configurados para la seguridad.

## Servidor Web

1. En la carpeta que creamos y que tenemos guardamos el archivo de _users.txt_, abrimos una terminal y hacemos un escaneo de versiones de servicios de _nmap_ del dominio local _ms2_, con un pequeño detalle con una nueva opción:

```sh
$> nmap -sV -oN scan_ms2_v.txt ms2
```

Donde la opción nueva, _-oN_, le indica a _nmap_ que guardaremos la salida del comando en un archivo _Normal_ (de texto) llamado **scan_ms2_v.txt**.

2. Verificamos con _cat_ o _less_ el contenido del archivo:

```sh
$> cat scan_ms2_v.txt 
# Nmap 7.95 scan initiated Sun Jun 15 19:28:25 2025 as: /usr/lib/nmap/nmap -sV -oN scan_ms2_v.txt ms2
Nmap scan report for ms2 (10.0.2.5)
Host is up (0.0039s latency).
Not shown: 976 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         vsftpd 2.3.4
22/tcp    open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp    open  telnet      Linux telnetd
25/tcp    open  smtp        Postfix smtpd
53/tcp    open  domain      ISC BIND 9.4.2
80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp   open  rpcbind     2 (RPC #100000)
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp   open  exec        netkit-rsh rexecd
513/tcp   open  login
514/tcp   open  tcpwrapped
1099/tcp  open  java-rmi    GNU Classpath grmiregistry
1524/tcp  open  bindshell   Metasploitable root shell
2049/tcp  open  nfs         2-4 (RPC #100003)
2121/tcp  open  ftp         ProFTPD 1.3.1
3306/tcp  open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp  open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp  open  vnc         VNC (protocol 3.3)
6000/tcp  open  X11         (access denied)
6667/tcp  open  irc         UnrealIRCd
8009/tcp  open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
55055/tcp open  mountd      1-3 (RPC #100005)
MAC Address: 08:00:27:DE:69:12 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jun 15 19:28:37 2025 -- 1 IP address (1 host up) scanned in 12.05 seconds
```

> Nota: Hay más formatos de archivo que se pueden usar con la opción _-o_, por ejemplo _-oX_ para archivos _XML_.

3. Ahora como vamos a verificar los puertos web, tenemos que buscar el servicio _http_ o _https_, entonces el siguiente comando filtran las líneas que tiene la palabra "http":

```sh
$> cat scan_ms2_v.txt | grep http
80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
```

- El caracter de la tubería o el _pipe_ va a dar como argumento del siguiente comando, _grep_, el texto entero del archivo.
- El comando _grep_ filtra el texto con las líneas que tengan la palabra "http".

El resultado nos da 2 puertos:

- _80_. Sabemos que cuando el puerto 80 está abierto con un software de servidor web, en este caso _Apache httpd 2.2.8 ((Ubuntu) DAV/2)_, podemos visitar en un navegador su IP y ver su index.html configurado, lo cuál hicimos en una lección pasada.
- _8180_. Con el software _Apache Tomcat/Coyote JSP engine 1.1_, que permite _ejecutar aplicaciones web del lenguaje Java_, en específico _JSP (Java Server Pages)_, visitando http://ms2:8180.

> Notas: _grep -i http_ haría la búsqueda sin importar mayúsculas/minúsculas, mientras que para una búsqueda más estructurada, _grep -E 'http|https'_ incluiría ambos protocolos.

4. Ahora vamos a instalar _Nikto_, un escáner de vulnerabilidades web de código abierto. Se utiliza para identificar posibles problemas de seguridad en servidores web, como archivos y configuraciones peligrosas, versiones obsoletas del software del servidor, además escanea más allá de versiones y detecta archivos peligrosos, configuraciones inseguras y cabeceras HTTP que pueden revelar información entre otros:

```sh
$> apt install nikto
```

> Nota: En las versiones nuevas de Kali Linux, _Nikto_ viene preinstalado.

5. Ahora vamos a usar el nuevo paquete de Nikto, poniendo la bandera _-h (host)_ y el valor _http://ms2_ como dominio local que apunta a la IP de _Metasploitable2_, para el análisis de las vulnerabilidades del puerto 80:

```sh
nikto -h http://ms2            
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          10.0.2.5
+ Target Hostname:    ms2
+ Target Port:        80
+ Start Time:         2025-06-15 19:55:37 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache/2.2.8 (Ubuntu) DAV/2
+ /: Retrieved x-powered-by header: PHP/5.2.4-2ubuntu5.10.
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ /index: Uncommon header 'tcn' found, with contents: list.
+ /index: Apache mod_negotiation is enabled with MultiViews, which allows attackers to easily brute force file names. The following alternatives for 'index' were found: index.php. See: http://www.wisec.it/sectou.php?id=4698ebdc59d15,https://exchange.xforce.ibmcloud.com/vulnerabilities/8275
+ Apache/2.2.8 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
+ /: Web Server returns a valid response with junk HTTP methods which may cause false positives.
+ /: HTTP TRACE method is active which suggests the host is vulnerable to XST. See: https://owasp.org/www-community/attacks/Cross_Site_Tracing
+ /phpinfo.php: Output from the phpinfo() function was found.
+ /doc/: Directory indexing found.
+ /doc/: The /doc/ directory is browsable. This may be /usr/doc. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-1999-0678
+ /?=PHPB8B5F2A0-3C92-11d3-A3A9-4C7B08C10000: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings. See: OSVDB-12184
+ /?=PHPE9568F36-D428-11d2-A769-00AA001ACF42: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings. See: OSVDB-12184
+ /?=PHPE9568F34-D428-11d2-A769-00AA001ACF42: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings. See: OSVDB-12184
+ /?=PHPE9568F35-D428-11d2-A769-00AA001ACF42: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings. See: OSVDB-12184
+ /phpMyAdmin/changelog.php: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts.
+ /phpMyAdmin/ChangeLog: Server may leak inodes via ETags, header found with file /phpMyAdmin/ChangeLog, inode: 92462, size: 40540, mtime: Tue Dec  9 12:24:00 2008. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
+ /phpMyAdmin/ChangeLog: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts.
+ /test/: Directory indexing found.
+ /test/: This might be interesting.
+ /phpinfo.php: PHP is installed, and a test script which runs phpinfo() was found. This gives a lot of system information. See: CWE-552
+ /icons/: Directory indexing found.
+ /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
+ /phpMyAdmin/: phpMyAdmin directory found.
+ /phpMyAdmin/Documentation.html: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts.
+ /phpMyAdmin/README: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts. See: https://typo3.org/
+ /#wp-config.php#: #wp-config.php# file found. This file contains the credentials.
+ 8658 requests: 0 error(s) and 27 item(s) reported on remote host
+ End Time:           2025-06-15 19:56:18 (GMT-4) (41 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

> Nota: El puerto 80 se toma por defecto cuando se busca una URL en un navegador, por ese motivo también funciona _http://ms2:80_.

6. Hacemos lo mismo pero con el puerto _8180_:

```sh
$> nikto -h http://ms2:8180
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          10.0.2.5
+ Target Hostname:    ms2
+ Target Port:        8180
+ Start Time:         2025-06-15 19:56:25 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache-Coyote/1.1
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ /favicon.ico: identifies this app/server as: Apache Tomcat (possibly 5.5.26 through 8.0.15), Alfresco Community. See: https://en.wikipedia.org/wiki/Favicon
+ OPTIONS: Allowed HTTP Methods: GET, HEAD, POST, PUT, DELETE, TRACE, OPTIONS .
+ HTTP method ('Allow' Header): 'PUT' method could allow clients to save files on the web server.
+ HTTP method ('Allow' Header): 'DELETE' may allow clients to remove files on the web server.
+ /: Web Server returns a valid response with junk HTTP methods which may cause false positives.
+ /: Appears to be a default Apache Tomcat install.
+ /admin/: Cookie JSESSIONID created without the httponly flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
+ /admin/contextAdmin/contextAdmin.html: Tomcat may be configured to let attackers read arbitrary files. Restrict access to /admin. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2000-0672
+ /admin/: This might be interesting.
+ /tomcat-docs/index.html: Default Apache Tomcat documentation found. See: CWE-552
+ /manager/html-manager-howto.html: Tomcat documentation found. See: CWE-552
+ /manager/manager-howto.html: Tomcat documentation found. See: CWE-552
+ /webdav/index.html: WebDAV support is enabled.
+ /jsp-examples/: Apache Java Server Pages documentation. See: CWE-552
+ /admin/account.html: Admin login page/section found.
+ /admin/controlpanel.html: Admin login page/section found.
+ /admin/cp.html: Admin login page/section found.
+ /admin/index.html: Admin login page/section found.
+ /admin/login.html: Admin login page/section found.
+ /servlets-examples/: Tomcat servlets examples are visible.
+ /manager/html: Default account found for 'Tomcat Manager Application' at (ID 'tomcat', PW 'tomcat'). Apache Tomcat. See: CWE-16
+ /manager/html: Tomcat Manager / Host Manager interface found (pass protected).
+ /host-manager/html: Tomcat Manager / Host Manager interface found (pass protected).
+ /manager/status: Tomcat Server Status interface found (pass protected).
+ /admin/login.jsp: Tomcat Server Administration interface found.
+ 7974 requests: 0 error(s) and 27 item(s) reported on remote host
+ End Time:           2025-06-15 19:57:38 (GMT-4) (73 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

8. Ahora vamos a analizar algunos resultados de Nikto con el host víctima:

27 elementos se encontraron analizando el puerto 80, el mismo número que con el puerto 8180.

Ejemplos de puerto 80:

- El servidor _Metasploitable2_ tiene instalado _PHP_, un lenguaje de programación web, el cual si se introduce un script con la etensión _.php_ lo puede ejecutar.
- Se confirma que _phpMyAdmin_ se encuentra instalado en el servidor, el cual sirve para administrar bases de datos del motor _MySQL_.
- El usuario normal cuando navega a http://ms2/phpinfo.php da muchos datos del servidor y de la versión de PHP instalada. Investigala tu mismo. Esta ruta sólo debería de aparecer para algunos usuarios como los desarrolladores web.
- PHP puede mostrar datos sensitivos con los _query parameters_, aquellos pares _nombre=valor_ que aparecen en las URLs que hacen solicitudes al servidor mostrado las páginas web como respuesta. _Nikto_ detectó _http://ms2/doc/?=PHPB8B5F2A0-3C92-11d3-A3A9-4C7B08C10000_, el cuál abre un directorio con muchas archivos y carpetas de la instalación de _Apache_ que no debería de ser de acceso a todos los usuarios.

Ejemplos del puerto 8081:

- Las acciones (en software _verbos_) _PUT y DELETE_ están habilitados. Esto quiere decir que podemos subir/crear archivos y recursos (PUT) al igual que borrárlos (DELETE) con peticiones a la URL _http://ms2:8180_.
- Hay muchas rutas que dan información del login del usuario administrador del servidor de _Tomcat_, como http://ms2:8180/admin/login.html que muestra el formulario de inicio de sesión de Tomcat.
- ¡Se está usando la cuenta predeterminada de administrador de Tomcat! En la línea _Default account found for 'Tomcat Manager Application' at (ID 'tomcat', PW 'tomcat')_. Si entramos al formulario anterior o a http://ms2:8180/admin y escribimos estas credenciales, tendremos el acceso total al administrador de Tomcat.
- Se tiene el acceso a /manager/status y con las credenciales por defecto se puede ver el estado del servidor _Tomcat_.

**Con Nikto, ejecutamos 2 comandos y hemos obtenido toda esta información.**

## Mitigar vulnerabilidades

Algunas acciones que se pueden hacer para mitigar estos peligros detectados por Nikto son:

- Deshabilitar métodos HTTP peligrosos (PUT, DELETE, TRACE).
- Aplicar restricciones a _phpMyAdmin_.
- **No usar credenciales predeterminadas**.
- Actualizar Apache y Tomcat a versiones seguras.

## Reflexión final

Ya vamos almacenando varias herramientas para la detección, verificación, búsqueda y escaneo de vulnerabilidades de una red, agregando _Nikto_, que detecta lo relativo a HTTP / HTTPS, su manejo es simple y tenemos un ejemplo de _nmap + nikto_ para detectar vulnerabilidades de acceso a un servidor web, o generalmente de un host cualquiera.

Más adelante en el curso veremos formas más avanzadas de ataque, que teniendo acceso al host víctima, podemos hacer acciones más severas en cuanto al compromiso de datos y la administración de software.

Lo que hemos estado viendo a través de esas prácticas son softwares y servicios que ocupan puertos para su funcionamiento, y a pesar de que es vital el uso de puertos específicos para su operación, se _pueden usar de otras formas_ no teniendo en cuenta los desarrolladores de software la utilización de su software con otro propósito más que el de su construcción. A esas vulnerabilidades se les llama _Day Zero_, herramientas como ExploitDB pueden ayudar a identificar vulnerabilidades recientes, por eso debemos de **mantener nuestros paquetes, librería y software actualizados**.
