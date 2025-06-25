# Subdominios

Recordemos que un Un **dominio** es una denominación de un grupo de dispositivos o equipos conectados a una red que comparten una administración centralizada. Por lo tanto, un **subdominio** es un subconjunto de un dominio.

Ejemplos:

```sh
# Esto es un dominio
google.com
# Esto es un subdominio
images.google.com
```

Nótese que la parte agregada al dominio es un _prefijo_, y es la práctica recomendada para crear administraciones de redes a partir de dominios.

En esta lección vamos a encontrar los _subdominios_ de un dominio.

## Herramientas para encontrar subdominios

Vamos a utilizar otra herramienta de comando para encontrar los subdominios a partir de un dominio, **sublist3r**.

### sublist3r

Para instalar en tu Kali Linux, instala el paquete:

```sh
$> sudo apt install sublist3r
```

> Nota: Puedes iniciar sesión como sudo para evitar el uso de _sudo_ en los comandos.


Vamos a usar como ejemplo al dominio del creador de Kali Linux, _kali.org_

```sh
$> sublist3r -d kali.org
```

Donde:

- **sublist3r**: Comando o herramienta.
- **-d**: Bandera que indica _dominio a buscar los subdominios_
- **kali.org**: Valor de la opción o argumento de -d.

En la salida del comando cuando se ejecuta:

1. Lista una serie de motores de búsqueda que tienen indexadas subdominios que contengan _*.kali.org_.
2. Si existen bloqueos a sublist3r y las solicitudes para encontrar los subdominios en un motor, te lo dira en un log que dice _[!] Error: <Motor> now is blocking our requests_. Esto puede afectar al número de subdominios que puede encontrar la herramienta.
3. La operación de búsqueda puede tardar un poco, más cuando finalize:
  
- Indica el total de sudominios únicos encontrados:
  - ```[-] Total Unique Subdomains Found: 104```
- Lista los subdominios encontrados.

Prueba a visitar con tu navegador uno de los subdominios arrojados por _sublist3r_, por ejemplo _archive.kali.org_, que es uno de los subdominios en donde se encuentran las ímagenes de instalación de Kali Linux.

4. Ahora encuentra las direcciones IP tanto del dominio _kali.org_ como _archive.kali.org_:

```sh
$> nslookup kali.org && echo "\n\n" && nslookup archive.kali.org
```

Este comando nos ayuda a identificar si las redes de las IP **son diferentes**, es decir, si las IP son similares en octetos o no, ejemplo si enn el resultado del anterior te sale:

- kali.org = 104.18.4.159
- archive.kali.org = 192.99.45.190

Por lo tanto una deduccion es que _las IP no estén en la misma red_, debido a que los primeros bits _104.x.x.x_ y _192.x.x.x_ no son los mismos.

> Nota: El comando **echo** imprime el mensaje que especifiquemos, en este caso imprime _\n\n_ = 2 Sequencias de saltos de líneas. Ejemplo, _echo "Hola\nKali"_ imprime:

```sh
Hola
Kali
```

### Netcraft

_Netcraft_ es un sitio web que hace la misma función de _sublist3r_ y además es uno de los motores que consulta, enumerando los subdominios encontrados a partir de un subdominio, su URL es:

http://searchdns.netcraft.com

1. Hay una lista de selección y una barra de búsqueda. Selecciona de la lista _Site ends with_ (el sitio finaliza con ...) y teclea en la barra de búsqueda _kali.org_.
2. Haz clic en _Search_, y aparecerán los resultados provistos en su base de datos:

- Conteo de subdominios encontrados. Como verás, son pocos comparado a sublist3r, ya que los distintos motores tiene diferentes "formas y filtros" de búsqueda.
- Tabla del listado de los subdominios, con las columnas:
- Rank: Número entero que Netcraft establece al subdominio determinando el tráfico, entre menor sea el número, más visitado es.
  - _Site_: Subdominio encontrado.
  - _First Seen_: Fecha en que fue encontrado por primera vez.
  - _Netblock_: Entidad que provee las IPs del subdominio.
  - _OS_: Sistema Operativo de los servidores.
  - _Site Report_: Datos del subdominio, como la IP y protocols que usa.

## Herramienta para comprobar subdominios

### crt.sh

Es una herramienta que permite buscar certificados SSL/TLS emitidos para un dominio, lo que ayuda a identificar subdominios asociados, su URL es:

https://crt.sh

1. Escribe en la barra de búsqueda el dominio _kali.org_ para comparar los resultados con las anteriores herramientas.
2. Los resultados se muestran en una tabla:

- ID: Un identificador único para el certificado en la base de datos del sitio.
- Logged At: La fecha en que el certificado fue registrado en el sitio.
- Not Before: La fecha de inicio de validez del certificado.
- Not After: La fecha de expiración del certificado.
- Common Name (CN): El nombre principal del dominio para el cual se emitió el certificado.
- Matching Identities: Lista de subdominios o nombres alternativos (SANs) incluidos en el certificado.

> Nota: Algunos resultados pueden ser duplicados, encontrados con uno y otro método del sitio.

3. Además, este sitio te permite exportar los resultados con 2 botones en la parte superior derecha: CSV o JSON. Guarda los resultados en CSV y abre el archivo con un software de Hojas de Cálculo (en Kali por default es _Vim_ o _Mousepad_, pero puedes instalar _LibreOffice_, para usar _Calc_).
4. Elimina todas las columnas excepto **Common Name**, quedando sólamente los subdominios (quita además el encabezado de la columna), y quita los duplicados. Guarda el resultado en un archivo de texto, como **kali_org_subdomains.txt**.

_Este archivo lo usaremos con la siguiente herramienta_.

### httprobe

_httprobe_ nos ayudará a comprobar si un subdominio o lista de subdominios está disponible en la web sin tener que comprobar uno por uno. Para ello vamos a seguir los siguientes pasos:

1. Encontrar el repo de _GitHub_ de httprobe:
https://github.com/tomnomnom/httprobe

2. En el repositorio indica el comando de descarga de httprobe en la sección _Install_. Para ello, tenemos que instalar un paquete para la ejecución de ese comando de descarga:

```sh
$> apt install golang
```

> GO es un lenguaje de programación, evitar preocuparte, sólo lo vamos a usar para ejecutar el comando, no para programar.

3. Ahora copia del repositorio de GitHub el comando, pégalo en la terminal y ejecútalo.
4. Cambia al directorio _home_ del usuario de la terminal, ahí debe que estar la descarga en la carpeta /home/<usuario>/go/bin (si eres root /root/go/bin), ahí estará el ejecutable de _httprobe_.
5. Ejecuta _httprobe_ con el siguiente comando:

```sh
cat /ruta/a/tu/archivo/kali_org_subdomains.txt | ./httprobe
```

Donde:

- **cat** es un comando para ver contenidos de archivos de texto en la terminal.
- **/ruta/a/tu/archivo/kali_org_subdomains.txt** es la ruta a tu archivo de texto que contienes filas con los subdominios a comprobar.
- **|** es el caracter _pipe (tubería)_, que se utiliza para encadenar comandos en la línea de comandos, donde la salida de un comando se envía como entrada al siguiente. Así, la lista de subdominios es pasada al siguiente comando del _pipe_.
- **./httprobe** es el ejecutable que comprueba los subdominios.

6. La salida del comando son las URL que comprueba que sí existen y está disponibles en la web. Ahora vuelve a ejecutar el comando pero con una adición:

```sh
cat /ruta/a/tu/archivo/kali_org_subdomains.txt | ./httprobe > /en/donde/quieres/tu/archivo/de/resultados/kali_org_subdomains_ok.txt
```

Donde en la parte agregada:

- **>** se utiliza como parte de la redirección de salida en la línea de comandos, para redirigir la salida de un comando a un archivo.
- **/en/donde/quieres/tu/archivo/de/resultados/kali_org_subdomains_ok.txt** es la ruta del archivo **kali_org_subdomains_ok.txt** que contendrá la salida de httprobe. Nota que no sale nada en la terminal mientra se ejecuta el comando. Cuando se complete verás el prompt de tu terminal listo para recibir otro comando.

7. Cuenta cuantas lineas hay en cada archivos:

```sh
$> wc -l /ruta/a/tu/archivo/kali_org_subdomains.txt && echo "\t" && wc -l /tu/archivo/de/resultados/kali_org_subdomains_ok.txt
101 kali_org_subdomains.txt

135 kali_org_subdomains_ok.txt
```

Donde:

- **wc** _(word count)_ permite realizar diferentes conteos desde la entrada estándar, ya sea de palabras, caracteres o saltos de líneas.
- **-l** es la bandera para contar _saltos de línea_ o renglones del archivo.
- **\t** es el tabulador para la terminal.

Como verás, los números no coinciden, ya que el archivo de salida de _httprobe_ contiene algunos resultados tanto para HTTP como para HTTPS, además de que algunos dominios podrían redirigir automáticamente entre versiones, lo que puede influir en los resultados. También puedes visitar cualquier subdominio comprobar por _httprobe_ en el archivos de resultados.

> Nota: Puedes usar el comando _less_ para ver contenidos de archivos de texto, y buscar con el signo de pregunta cerrado lo que quieres buscar: **?término_a_buscar**.

## Uso de los resultados

Si encuentra un subdominio que exponga el servidor web utilizado y su versión, archivos o carpetas, podrás usar dicha información para planear formas en la que se podrían entrar al servidor.

Si se encuentra el subdominio en Netcraft, puedes acceder al reporte del sitio que lista información fundamental acerca de.

Para discriminar si los subdominios están disponibles en la web y no verificar manualmente a todos puedes usar la combinación _crt.sh_ + _httprobe_. Hay veces en que los administradores de red olvidan borrar un subdominio y posiblemente ahí es una vulnerabilidad de dominio.
