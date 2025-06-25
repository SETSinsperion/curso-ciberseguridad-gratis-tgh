# Terminal: Fundamentos

## 1.  ¿Que es?

Al igual que otras aplicaciones de tu computadora, es una aplicación, que maneja al SO mediante comandos de texto. Otros nombres que recibe es _Consola_ o _Shell_. En otras palabras, un SO puede ser operador mediante una interfaz gráfica (como GNOME, Xfce, etcetera) o por interfaz de texto.

El "miedo" que se ha llegado a generar de los SOs basados en kernel de Linux es que "todo se maneja desde la terminal", esto es un mito, ya que GNOME, Xfce, Deepin y otros escritorios son UIs de distribuciones de Linux. Antes, los SOs era pura interfaz de texto, así que no había de "otra": o tenías que aprender los comandos para administrador tu equipo o no hacía nada. Los SOs basados en Linux siguieron esa _filosofía_, mientras que sistemas como Windows se orientaron mas a la GUI (Graphical User Interface).

Así como aprendiste a usar las ventanas y botones de la GUI en Windows, tú también puedes aprender a manejar mediante comandos tu SO, en este caso Kali Linux.

## 2. Comandos

Los comandos son instrucciones de texto que ejecutan paquetes, librerías y aplicaciones en la terminal. Siguen este simple flujo:

- Teclear comando.
- Presionar _Enter_ para ejecutarlo.

En la sección final de este documento viene los comandos que si o si vas a usar con tu terminal.

## 3. Partes del prompt

Cuando entras o inicias la terminal, el prompt es la línea que está _esperando_ al tecleo de un comando. En Kali Linux, el prompt se conforfma por datos importantes del usuario y la maquina en que se está usando la terminal:

[usuario]@[hostname]:[directorio_actual][$(usuario normal) | # (usuario root)]

2 ejemplos de un prompt son:

- kali@kali:~$
  Donde:
  - usuario = kali.
  - hostname = (nombre de tu equipo) kali.
  - directorio actual = ~ (La virgulilla indica la carpeta de usuario logueado).
  - $ = Usuario normal.
- root@kali:[/home/kali]#
  Donde:
  - usuario = root.
  - hostname = (nombre de tu equipo) kali.
  - directorio actual = /home/kali.
  - \# = Usuario root o superusuario.

## 4. Directorios

Los directorios son el nombre _formal_ de un folder o carpeta que tu conoces de Windows (en SOs basados en Linux se prefiere los términos formales). Más a fondo, son _nodos_ de un _árbol_ en donde se pueden guardar archivos, y para simplificar, hay 2 estructuras de directorios en SO:

- Estructura de directorios
  1. / = Basado en UNIX.
  2. [Drive]:\ = Basado en Windows.

Ejemplos:

1. "/home/testuser" es un directorio UNIX.
2. "C:\Users\testuser" ess una carpeta de Windows.

La estructura de directorios tiene lo que llama un _carácter separador_, "/" en UNIX y "\" en Windows, sirviendo para que el sistema reconozca cuando es el nombre de un directorio y cuando empieza el de otro folder. Kali Linux y todo SO basado en Linux usa la estructura UNIX.

## 5. Comandos fundamentales

## clear (Shortcut: Ctrl + l)

Cuando ejecutas varios comandos, se acumulan muchas líneas en tu terminal, este comando "limpia" dejando una sola línea.

## sudo su root | sudo su

Cambiar del usuario actual al usuario "root" o cualquier otro.

## pwd (_print working directory_)

Imprime la ruta completa del directorio en dondé está operando la terminal.

## cd [directorio] (_change directory_)

Cambiar al directorio especificado. Algunos detalles que te puedes serte útiles al usar este comando son:

- . = Significa "este directorio", ejemplo, cuando usas un comando "cd .", no cambiar de directorio, porque tu le ordenaste al comando que "se cambiara a este mismo directorio". Este notación adquiere más sentido con otros comandos, como "cp ./* /test" (copia todos los archivos de este mismo directorio al directorio /test).
- .. = Significa "al directorio padre o al directorio anterior", cuando ejecutas "cd .." en el directorio _/home/test_, significa que "quieres retroceder a la carpeta anterior", es decir _/home_.
- ~ = Este caracter, la virgulilla, indica la carpeta de usuario logueado. Si te logeaste con el usuario _kali_, al hace _cd ~_ le indicaste a la terminal que quierees ir a _/home/kali_.

## whoami

Indica el usuario que está usando la terminal.

## hostname

Indica el equipo en donde se está usando la terminal.

## ls [-l | -a]

Lista los archivos y directivos dentro de la carpeta actual. Algunas banderas (flags) útiles son:

- -l = Lista con la descripción de los usuarios, permisos y fechas.
- -a = Lista los archivos y subdirectorios tanto visibles como ocultos.
- -la = Lista todos los archivos y subdirectorios con la descripción de los usuarios, permisos y fechas.
- Prefijo de archivos/carpetas ocultas: .*, ejemplos:
  - .bashrc
  - /.git

## passwd

Actualiza la contraseña del usuario actual, un prompt te solicitará la nueva contraseña y otro la confirmación.

## !!

Para ejecutar de nuevo el comando ejecutado anteriormente. Usualmente se usa cuando un comando necesita más permisos y/o privilegios:

```sh
$> ifconfig
bash: ifconfig: command not found
$> sudo !!
]eth0: flags ...
```

En este caso la terminal interpretó _sudo **!!**_ en _sudo **ifconfig**_.