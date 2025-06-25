# Actividades despues de Instalar Kali

Vamos a aprender lo básico del uso no sólo de Kali Linux sino de cualquier SO basado en el kernel de Linux.

## 1. Terminal

La terminal es la aplicación en donde tu puede acceder al shell, el programa de control mediante comandos del SO, en la esquina superior izquierda de Kali Linux está el menú de aplicaciones, haz clic en él y busca "Administrador de Configuraciones (Settings Manager)". 

## 2. Comandos Fundamentales

Algunos de los comandos del día a día trabajando con SOs basados en Linux son:

1. **sudo**: Comando para hacer cualquier otro comando con los más altos permisos en el SO, lo cual lo hace el usuario root, el "super-usuario" del SO. _sudo_ se puede traducir como _Superuser, Do [comando]_.
2. **sudo apt update**: Comando para actualizar los origenes de paquetes (coinexiones a servidores de las librería del SO). En el caso de SOs basados en Debia (otro SO basado en Linux, uno de los llamados _Distribuciones Madres_) el gestor de paquetes es llamado **apt**, y el subcomando para actualizar los origenes es *update*. Nótese que este comando NO actualiza ningún paquete, sino trae información de lo que _puede ser actualizado_.
3. **sudo apt dist-upgrade -y**: Comando para actualizar todos los paquetes del SO, tomando como SÍ la respuesta a cada pregunta que pueda haber en los prompts de la actualización.

> Nota: Las _Flags_ son las opciones que usa un comando para trabajar, en el comando anterior usa **-y**, que significa _YES_. La mayoría de los comandos tiene su propio set de opciones o flags.

4. **sudo adduser [new_username]**: Agregar al SO un nuevo usuario. Sustituir "[new_username]" por el nombre de usuario deseado.

> Nota: Cuando ejecutas comando que requieren información mediante prompts (preguntas), la respuestas predeterminadas suele ser marcadas como [RESPUESTA1 / respuesta2], siendo RESPUESTA1 la introducida cuando el usuario no quiera responder (_Enter_) o responda con cualquier cosa que no sea parte de las respuestas válidas (ej. RESPUESTA3).

5. **sudo usermod -aG [grupo] [username]**: Agregar a [username] al grupo del SO llamado [grupo]. Cuando creas un usuario y quieras que sea capaz de ejecutar comandos con _sudo_, tienes que agregarlo al grupo con este comando: ```sudo usermod -aG sudo nuevo_usuario```
6. **su - [username]**: Cambio al usuario actual por [username], introduciendo su contraseña.
7. **id**: Se usa para mostrar la información de identificación de usuario y grupos. Permite ver el UID (identificador de usuario), el GID (identificador de grupo) y los grupos a los que pertenece un usuario, así como la información de inicio de sesión, entre otros. 

> Nota: Cualquier cambio a permisos de usuario debe de re-logearse el usuario.

8. **sudo apt install [paquete]**: Instalar el o los paquetes especificados en el comando. 
  - Si quieres instalar un paquete:
    - sudo apt install htop (task manager en shell)
    - sudo apt install tmux (terminal con pestañas)
  - Si quieres instalar varios paquetes:
    - sudo apt install tmux htop
9. **sudo apt autoremove**: Algunos paquetes ya no son usados por el SO, lo cual los hacer "ocupar espacio de almacenamiento", ejecutando este comando libera dicho espacio.

## 3. Uso de htop

El comando _htop_ es uno de los administradores de tareas y procesos más poderosos y minimalistas, es una de las razones por las que Linux se ganó _el miedo_ de los usuario, mas cuando ya está ejecutando comandos en la terminal, ese miedo se va perdiendo, los comandos son como las apps, pero con _texto_. **htop** se usa básicamente de la siguiente forma:

```sh
# Iniciar htop
$> htop
# F3 Buscar (Search) htop
F3 Search: el proceso que deseas buscar ...
"Enter" para seleccionar el proceso encontrado
# F9 "Matar" procesos htop
F9 o hacer clic en el "botón" Kill
```

## 4. Uso de tmux

TMUX es otro comando para usar terminales con la diferencia de usar _pestañas_ en lugar de abrir otras ventanas de terminales. Su uso básico es: 

. Iniciar tmux
. Ctrl + b, c = Iniciar otra _pestaña_ de terminal
. Ctrl + b, F = Seleccionar siguiente pestaña de terminal
. Ctrl + b, % = Dividir verticalmente pantalla de una pestaña
. Ctrl + b, " = Dividir horizontalmente pantalla de una pestaña

Para más información sobre shorcuts: tmuxcheatsheet.com

> Nota: Primero tiene que presionar juntos Ctrl + b, y después la tecla que se indica en el shortcut.

## 5. Settings Manager

Es el "Panel de Control" de los Sistemas Operativos basados en Linux. Algunas de las opciones para personalizar tu "Kali Linux" son:

  - Appearence.
  - Desktop.
  