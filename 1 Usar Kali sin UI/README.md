# Usar Kali sin UI

Para realizar esta actividad, necesitarás tener una máquina virtual de VirtualBox. Esta forma de manejar Kali está basada en utilizar tu máquina virtual como "servidor SSH" en tu SO anfitrión.

1. Abre VirtualBox y entra en la configuración de tu Máquina Virtual (VM).
2. Ve a la sección de Red (Network) y selecciona en el parámetro "Attached to" (conectado a) el valor _Bridged Adapter_ (adaptador de puente), no olvides de mantener seleccionado _Enable Network Adapter_ (habiltar adaptar de red).
3. Iniciar tu máquina virtual e iniciar tu Terminal de Kali Linux, tecleando y ejecutando los siguientes comandos:
  a. sudo su (se te pedirá la contraseña de tu superusuario, por default, _kali_).
  b. service ssh start (inicia ssh en tu máquina virtual).
  c. systemctl enable ssh (habilita ssh una vez iniciasdo el servicio).
  d. hostname -I (obtener tu dirección IP para usar tu máquina virtual).
4. Para hacer un test de la confuiguración de nuestra VM, en tu SO anfitrión abre tu terminal y conecta por el comando de SSH con el siguiente servidor:

```sh
$> ssh [usuarioVM]@[direccionIpVM]
```

por lo tanto:

```sh
$> ssh kali@192.168.1.70
The authenticity of host '192.168.1.70 (192.168.1.70)' can't be established.
ED25519 key fingerprint is SHA256:tXysfDF6MZ54KGBlzAQJLm7NIqSmOPdrQ1laR7Kqa1U.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.1.70' (ED25519) to the list of known hosts.
kali@192.168.1.70's password:
Linux kali 6.12.13-amd64 #1 SMP PREEMPT_DYNAMIC Kali 6.12.13-1kali1 (2025-02-11) x86_64

The programs included with the Kali GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Kali GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
┌──(kali㉿kali)-[~]
└─$ 
```

5. Una vez establecida la conexión, da la misma contraseña que en tu VM, tendrás una salida similar a la anterior.
6. Apaga tu VM, y de nuevo iniciada PERO con el modo **Headless Start** (inicio sin encabezados). Puedes encontrar esta opción en la flecha de "Iniciar" en la lista que despliega las demas opciones de encendido. Puedes ver el estado del encendido de la VM en la ventanita de _Preview_.
7. Cuando en el Preview se visualice la solicitud de las credenciales, puedes hacer nuevamente la conexión SSH como en los pasos 4 y 5. ¡Ya has configurado tu Kali sin usar UI!
8. Para detener / apagar la VM, en el listado de las máquinas clic contextual en tu VM Kali y selecciona Stop > Power Off.

> Nota: Puedes volver al modo NAT de Red de tu VM una vez finalizado este ejercicio.
