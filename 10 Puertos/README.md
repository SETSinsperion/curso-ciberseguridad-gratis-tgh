# Puertos

## Conceptos

**TCP / IP**: (_Protocolo de Control de Transmisión / Protocolo de Internet_) Conjunto de protocolos de comunicación que permiten a los dispositivos en una red, incluyendo Internet, comunicarse entre sí.

**Puertos**: Son tipos de conexiones identificados por un número. Para un host en particular, son puntos de comunicación dentro de un sistema operativo que permiten diferenciar distintos tipos de tráfico en una red.

**Socket**: La interfaz de comunicación entre procesos, ya sea en la misma máquina o en diferentes máquinas conectadas a una red. Para que dos procesos se comuniquen, necesitan identificar un punto de conexión, que es precisamente lo que define un socket. Este punto de conexión se determina por **dirección IP del dispositivo:puerto** que está utilizando el proceso que desea comunicarse, ejemplo _127.0.0.1:8000_.

**Firewall**: Hardware o software que administra los estados de los puertos conforme a reglas de entrada y salida del tráfico de una red, a esta acción se le llama _filtrado de tráfico por puertos_. Ejemplo: comando _iptables_ en terminal Linux.

## Estados de Puertos

- **Abierto**: A la _escucha_ (espera) de una conexión, procesado por una aplicación o software.
- **Cerrado**: _Bloqueo / Rechazo_ de cualquier intento de conexión, puede o no haber aplicaciones que estén a la espera de conexiones.
- **Filtrado**: Indica que un firewall _está bloqueando activamente_ el tráfico hacia ese puerto, sin responder a las solicitudes de conexión.

## Clasificación de Puertos

### Por rango

El rango de los puertos van de 0 a 65535, y están divididos en 2 clases:

**Puertos bien conocidos (0-1023)**
Usados por servicios estándar como HTTP (80) y SSH (22).

**Puertos registrados (1024-49151)**
Asignados a aplicaciones específicas.

**Puertos dinámicos o privados (49152-65535)**
Usados temporalmente por aplicaciones.

### Por protocolo de transporte

Un protocolo de transporte en red es un conjunto de reglas que definen cómo se deben enviar, recibir y procesar los datos entre diferentes dispositivos o aplicaciones en una red. Los puertos pueden ser:

- _**TCP (Transmission Control Protocol)**_: Son puertos que _necesitan que los datos sean recibidos enteramente_ (con comprobación de errores) antes de reportar que la conexión fue establecido correctamente. Ejemplo de uso: Navegador web.
- _**UDP (User Datagrams Protocol)**_. Son puertos que _reciben la información en partes_ (sin comprobación de errores) para reportar que la conexión fue establecido correctamente. Ejemplo de uso: juegos en línea, transmisión de video y llamadas VoIP.

## Por qué necesitamos saber de puertos

1. Para saber los tipos de conexiones y las aplicaciones que están esperando información a través de dicho punto de conexión, ejemplo:

    | # Puerto | Tipo/Aplicación |
    |----------|-----------------|
    |    80    |       HTTP      |
    |   443    |      HTTPS      |
    |  20-21   |       FTP       |
    |    22    |     SSH / SCP   |

2. _**Es el punto de acceso para los ciber-ataques**_. Cuando un host recibe un ataque es porque están _abiertos_ sin gestión adecuada, permitiendo el acceso al host o víctima. Algunos ejemplos:

- Ataques _port scanning_.
- _Exploits_ de servicios vulnerables.
- _Brute force_ en SSH.
- Ataques de _Denegación de Servicio_ (DoS).
- Ataques _Man-in-the-Middle_ (MITM).

> Nota: _Exploit_ es un software/hardware malicioso que abre la puerta a una vulnerabilidad, permitiendo que un atacante explote un fallo en el sistema.
