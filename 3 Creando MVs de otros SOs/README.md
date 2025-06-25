# Creando MVs de otros SOs

¿Para qué? Si en su mayoría, todos los que instalan VirtualBox o VMWare tiene a Windows como Sistema Operativo anfitrión (host), es fundamental que puedas aprender ciberseguridad con un LABORATORIO en el que se encuentre tu MV de Kali y otra con Windows o cualquier SO, una MV es la encargada de "estar activamente atacando y defendiendo" a la otra MV, en su caso la que vamos a crear con W10.

La principal ventaja de crear un laboratorio con Kali + otra MV es el aislamiento de todos lo que aprendamos a través del uso de Kali, quedando segura tu máquina local, por lo tanto:

- Creamos 2 MV, una con Kali y otro con algún otro SO.
- Configuramos una "red" que aisla a las 2 MVs.

## Crear una MV con W10

1. Visitar https://info.microsoft.com/ww-landing-windows-10-enterprise.html?lcid=es-es , llena los campos de tu información y después hacer clic en "Descargárlo ahora", escogiendo el idioma y la arquitectura (recomendado x64).

> Nota: W10 ya no es soportado por Microsoft a partir de octubre de 2025, por lo que el sitio de Microsoft Evaluation Center tiene a W11 como principal versión de evaluación (31/05/2025).

2. Descargada la imagen de W10, abrir VirtualBox o VMWare para crear una MV con el archivo descargado, y guarda tu nueva MV con W10, NO LA INICIES, tenemos que hacer una configuración adicional.
  - Recuerda asignarle una cantidad de memoria RAM que se encuentre dentro de recomendado por VirtualBox o VMWare, como vamos a tener ejecutandose a 2 MV juntas, W10 y Kali Linux, tienes que tener en cuenta las cantidades de esas 2 MVs para que no se sobreesature tu memoria física. 

> Nota: (SÓLO PARA VIRTUALBOX) En el video #6 del curso, usan VMWare para crear las 2 máquinas virtuales, y para que ambas esten dentro de una misma red, le asignan a c/u un adaptador de Red _Custom > VMnet8 (NAT)_, que su equivalente en VirtualBox es el adaptador **Red NAT (NAT network)**; para ello, se necesita antes crear una red NAT de VirtualBox:

  1. Abre _VirtualBox > File > Network Manager_.
  2. En las pestañas de los tiopos de red disponibles, selecciona _NAT Networks_.
  3. Ahora vamos a crear una red con el botón _Crear_.
  4. Ahora selecciona la nueva red y haz clic en _Properties_, esto es para cambiar el nombre de la red, o si quieres asignarle una dirección y/o máscaras diferentes a las que asignó por defecto VirtualBox. Cuando ya queden tus cambios, click en _Apply_.

  _Fuente: https://www.youtube.com/watch?v=1rqcK4N4OGw_

> Nota: Ahora ya tienes una red NAT de VirtualBox lista para su uso. Tal vez en VMWare tienes redes NAT creadas por defecto, por lo tanto si haces esta actividad por MVs VMWare, sólo tienes que seleccionar "Custom > VMnet# (NAT)" en el apartado de _Network_ de tu MV para hacer uso de esa red NAT.
> Nota: Recuerda que la instalación de Windows hay un momento en que te pedirá reiniciar, y si tu unidad óptica (CDROM, DVD) virtualizada todavía tiene la imagen de W10, te volverá a aparecer la ventana de inicio de instalación. Sólo apaga tu MV, remueve la image de tu unidad e inicia tu MV, y continuará la instalación.

## Red privada para las 2 MVs

Entra en las propiedades o Configuraciones de tus MVs de Kali Linux y W10 para cambiar el adaptador de red en la sección de _Network_ a lo siguiente:

1. _Red NAT_ (VirtualBox).
2. _Custom > VMnet# (NAT)_ (para VMWare).

Las redes NAT de los software de MVs sirven para aislar el tráfico de internet entre las MVs y tus máquinas físicas. Se pueden comunicar entre ellas, pero no se pueden comunicar entre tu máquina física y tus MVs (a menos por medios nativos). Compruébalo realizando en tus terminales un comando **ping** a las direcciones IP entre tus MVs y tu máquina anfitrión. Para encontrar las direcciones IP:

- En Kali Linux:

```sh
$> hostname -I
123.123.123.123
```

- En Windows:

```sh
$> ipconfig
Windows IP Configuration
...
Wireless LAN adapter Wi-Fi:

   Connection-specific DNS Suffix  . :
   IPv6 Address. . . . . . . . . . . : 1111:aaaa:11:vvvv:cccc:1111:2222:3333
   Temporary IPv6 Address. . . . . . : 1111:aaaa:11:vvvv:cccc:1111:2222:3333
   Link-local IPv6 Address . . . . . : fdff::c344:aaaa:4ddd:dddd%16
   IPv4 Address. . . . . . . . . . . : 123.123.123.123
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : fe80::5a76:acff:fe38:c0c0%16
                                       123.123.123.254

Ethernet adapter Ethernet:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :
```

> Nota: Asegúrate de que tu Red NAT tenga configurado el protocolo _DHCP_ para la asignación automática de IPs de tu red a las MVs que tengan conexión con tu red.

**¡Listo! Ya tienes tu MV con W10.**
