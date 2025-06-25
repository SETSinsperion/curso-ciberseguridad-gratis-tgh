# Creando Lab para hacer test de seguridad

En ciberseguridad, un _Lab_ o _laboratorio_ es un ambiente que permite hacer ataque y defensa informáticas _sin dañar_ a un ambiente productivo.

En esta lección veremos la instalación de otra MV llamada _Metasploitable 2_, un host con un SO basado en Linux que _simula_ problemas de seguridad, siendo un ejemplo perfecto para un _objetivo_ de ataques con un host con el SO Kali Linux.

## Instalación de Metasploitable 2

Para ello, tenemos que seguir los siguientes pasos:

1. Descargar el archivo de _Metasploitable 2_:
  https://sourceforge.net/projects/metasploitable/files/Metasploitable2/

2. Extraer los archivos de la descarga, hay 4:

- Para VMWare: Tenemos los archivos para la definión y un disco duro virtual para iniciar una máquina virtual con el sistema.
- Para VirtualBox: Usaremos el archivo _.vmdk_ para usar en una MV nueva ese almacenamiento.

3. Para su instalación en los sistemas de virtualización:

- VMWare: Haz clic en el archivo con la extensión _.vmx_ y VMWare automática se abrirá con una MV lista para configurar e iniciar.
- VirtualBox: Crear una nueva máquina virtual con las siguientes especificaciones:
  1. Memoria RAM 1GB (es suficiente para la instalación).
  2. Cuando te pregunta por un almacenamiento, seleccione "Almacenamiento que ya existe", y selecciona el archivo "*.vmdk".

> Nota: Configura tu MV de _Metasploitable_ con una red NAT _igual a la configurada en tu VM de Kali Linux_, ya sea con NAT configyurada con tu máquina física (opción NAT) o personalizada (Custom > VMnet# en _VMWare_ o Red NAT en _VirtualBox_).

4. Inicia tu VM de _Metasploitable 2_ para verificar su funcionamiento.

**Esta VM la usaremos en posteriores lecciones.**
