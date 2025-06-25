# Instalar KALI LINUX

La primera lección es instalar Kali Linux completo para realizar las actividades de este curso.

## Índice 

1. Por Máquina Virtual.
2. En USB (Portable).
3. Actualizar desde el propio OS.

## 1. Por Máquina Virtual

Tenemos 2 opciones:

- VirtualBox (Oracle).
- VMWare (Broadcom).

Ambos tienen versiones gratuitas, para VirtualBox por predeterminado ya que es un software open source, y la versión _Player_ para VMWare.

### VirtualBox

La opción recomendada y más rápida para ejecutar las máquinas virtuales es Oracle VirtualBox por su naturaleza open source, VMWare actualmente tiene varios productos como _Fusion Workstation_ cuando buscas "WMWare Player" que necesita una cuenta Broadcom para su instalación (30/05/2025)

1. Descargar desde https://www.virtualbox.org/wiki/Downloads.
  - VirtualBox Platform Packages (la versión depende de tu OS).
  - VirtualBox Extension Pack.
2. Instalar VirtualBox con la primera descarga listada en el punto anterior.
3. Una vez instalado, abrir VirtualBox y mientrras está abierto, instalar el extension pack.
  - Nota: La instalación es muy rápida y en las versiones actuales no se presenta ninguna ventana o mensaje de instalado exitoso.
4. Visitar https://www.kali.org/get-kali/#kali-virtual-machines y descargar la opción **VirtualBox**. Esta página tiene definiciones de máquinas virtuales pre-construidas listas para usar, y una de ellas es VirtualBox.
5. Una vez descargar, estará en formato comprimido (.7z, 30/05/2025), extrae su contenido y aparecerán 2 archivos:
  - Virtual Machine Definition (.vbox).
  - Virtual Disk Image (.vdi).
6. Haz doble clic en el archivo con la extensión *.vbox y aparecerá una barra de carga, este es el progreso para la instalación de tu Kali Linux virtualizado.
  - Si arrastas o haces clic en este archivo mientras VirtualBox está abierto en el sistema, no va a pasar nada, tal vez por un "bug" o detalle que Oracle no ha revisado.
7. VirtualBox una vez que haya completado la carga del archivo *.vbox abrirá tu máquina virtual, , teclea _Enter_ o haz clic en la primera opción del GRUB (arranque de cualquier Linux) para iniciar tu Kali, y enseguida se te pedirán las credenciales, como la descarga fue en una máquina preconstruida las credenciales son:
  - Username: kali
  - Password: kali
8. Una vez en el escritorio de Kali, apaga tu Kali haciendo clic en la esquina superior derecha.
9. (Opcional) Inicia otra vez VirtualBox y verás tu kali listado, haz clic en él, y enseguida clic en Settings/Configuración para ver si en algún parámetro VirtualBox detefcta una "configuración errónea", es decir, que tu Sistema Operativo anfitrión (host) pueda ejecutar con dificultad tu Kali virtualizado. Sobre todo:
  - Sistema / Número de núcleos de procesador.
  - Memoría / RAM.
  - Asegúrate de tener una cantidad que se ubique en la zona verde del rangop de tus recurso físicos.

## 2. En USB (Portable)

En la página de descarga de Kali Linux (https://www.kali.org/get-kali) hay una opción para **Live USB**, recomendado si tiene pocas prestaciones en tu máquina físicas:

1. Decarga la versión Live USB.
2. Descarga un flasheador, es decir, un programa que te permitirá cargar el archivo de Kali Linux descargada desde un puerto USB o una memoria extraíble, recomendado por el curso:
  - Balena Etcher: https://etcher.balena.io/#download-etcher
  - UNETBooting (ampliamente conocido para cargar Linux).
3. Instalar tu software para cargar SOs.
4. Sigue las instrucciones para cargar el archivo descargado (Live USB) en una memoria de mínimo 8GB (recomendado para Kali).
5. Una vez terminado el progreso de carga del Live USB en tu memoria, deberás de buscar cómo entrar a tu BIOS para cambiar el orden del dispositivo de arranque del Sistema Operativo, y cambia el orden para que tu sistema detecte como primera opción tu memoria.
6. Guarda los cambios y una vez iniciada tu máquina, tu Kali Linux estará iniciado.

> **Nota:** Esta opción NO guarda tu Kali Linux ni ninguno de tus cambios en el almacenamiento local, para ello se necesita la opción "Persistencia" que puedes erncontrar en tu software flasheador.

## 3. Actualizar desde el propio OS

En su mayoría, los Sistemas Operativos con base en el kernel de Linux tiene una serie de comandos que deberás de ejecutar en la terminal para actualizarlo a la siguiente versión Long Term Support (LTS, SO basados en Debian).

**¡Listo! tienes Kali Linux para comenzar con este curso.**