# Filtración de Datos

La _Filtración de Datos_ o _Data Breach_ es la exposición de datos privados para su uso ilícito. Los emails y sus contraseñas, identidades bancarias y redes sociales son algunos de los datos más buscados, debido a que son el punto de entrada a los ciber-crímenes. En la lección aprenderemos sitios web que recolectan datos historícos de los _data breaches_, mostrando si alguno de nuestro datos está disponibles por bases de datos robadas.

## Have I been powned?

_URL_
https://haveibeenpwned.com

Este sitio web buscará si correos o contraseñas especifícas están encontrados en las bases de datos de los _data breaches_ registrados. Para ello, basta con visitar el sitio y realizar los siguientes pasos:

### Saber si un correo electrónico ha sido filtrado 

1. En la barra de búsqueda del _Home_ de sitio, teclea un correo y haz clic en _Check_.
2. Hay dos posibles resultados:

  - Si el correo no ha sido encontrado en las bases de datos, el correo sigue sin ser parte de una filtración (_aún_).
  - Si el correo ha sido encontrado por el sitio, significa que en un(os) filtración(es) fue robada la base de datos, exponiendo el correo buscado siendo una señal que alguien está haciendo uso ilegal de tu información. En los resultados aparecen:
    - Número de data breaches en que se comprometío tu correo.
    - Listas de los data breachers con un resumen acerca de la filtración, que contiene:
        - Sitios web y/o compañia de la filtración que fue objetivo de un ciber-ataque. 
        - Fecha de la filtración.
        - Resumen de los _tipos_ de datos expuestos (emails, usernames, passwords).
        - Un botón para saber más acerca de la filtración.

3. En la primera sección de la búsqueda el sitio provee un servicio de _alarma_ en la que si el correo se encuentra en una _futura filtración_ te aviso por un correo. **RECOMENDADÍSIMO** si haz hecho una búsqueda de tus correos, habrás de hacer clic en el botón _Notify me_ y te llegará un correo de confirmación con un link, haz clic en el link para confirmar tu correo y el sitio ya estará enterado de tu petición.

### Saber si una contraseña ha sido expuesta

1. En la barra de búsqueda del _Home_ de sitio, en el menú superior haz clic en _Passwords_ y teclea una contraseña en la barra de búsqueda y haz clic en _Check_.
2. Hay dos posibles resultados:

  - Si la contraseña no ha sido encontrada en las bases de datos, sigue sin ser parte de una filtración (_aún_).
  - Si ha sido encontrada por el sitio, significa que en un(os) filtración(es) fue robada la base de datos, exponiéndola siendo una señal que alguien está haciendo uso ilegal de tu información. En los resultados aparecen:
    - Conteo de incidencias de la contraseña en los _data breaches_.
    - Link a políticas NIST (_National Institutes of Standards of Technology_).
    - Posibles peligros de tu contraseña filtrada:
      - _Credential Stuffing_: Ciber-ataque en donde usan tus credenciales en otros sitios.
      - _Data Breaches_: Pueden usar tus credenciales para otra filtración en otros sitios.
      - _Password Patterns_: Puden analizar el patrón de tus contraseñas para deducir tus otras contraseñas.

## Dehashed.com

_URL_
https://www.dehashed.com

Es otro sitio donde podrás verificar si tus datos personales de tu identidad cibernética han sido expuestos. Es más detallada que _haveibeenpwned.com_ y cuenta con filtros para búsquedas de varios tipos de datos (IP, emails, usernames, Domain scan, etcétera). Puedes usar de manera similar al anterior sitio, más tendrás que crear una cuenta.

Cuando creas la cuenta, _dehashed.com_ te dirá tanto el conteo como los _data wells_ (lugares en donde se usa los datos, por ejemplo sitios web), más necesitarás ser usuario de pago si quieres desbloquear toda la información acerca de los resultados y funcionalidades.

## Administradores de Contraseñas

TU memoria puede retener las contraseñas, pero con el paso del tiempos las políticas de creación de contraseñas seguras en donde te piden un número de veces de caracteres mínimo te va a hacer más difícil el recordar todas. Por ello, sitios como el oficial de _NIST_ y _PasswordManager_ encarecidamente recomienda el uso de un **Administrador de Contraseñas** como **1Password, Dashlane** y **Keeper** que en lo general:

- Guardan tus contraseñas en un software cross-platform (multi-plataforma).
- Tiene un generador de contraseñas seguras para usar en todas tus cuentas.
- Siempre que deseas usar tu administrador te pedirá la _contraseña maestra_, la cual creas al momento de tu registro o de la instalación del software.
- Ofrecen opciones de respaldo en la nube de tus contraseñas.

## Acciones preventivas y correctivas ante filtraciones

Si en algunos de los sitios que has leído tus correos o contraseñas han sido filtrados, **la acción más importante y urgente** es **cambiar las contraseñas de tus cuentas filtradas por unas más seguras**, ya sea por una del generador de tu Administrador de Contraseñas o siguiendo las recomendaciones del _NIST_: https://www.nist.gov/cybersecurity/how-do-i-create-good-password (4/6/2025).

Muy recomendable tener una _política de cambio de contraseñas_, muy cómun en el software empresarial, que consiste en el registro de una nueva contraseña por cada periodo transcurrido, decido ya sea por ti o por el departamento de IT (Information Technologies).
