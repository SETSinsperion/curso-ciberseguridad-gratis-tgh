# OSINT

La inteligencia, a parte de ser una cualidad humana que realizamos por el raciocionio de las cosas, en el contexto de redes es un _esfuerzo conjunto_. **OSINT** es la _Inteligencia de Fuentes Abiertas_ (Open Source Intelligence), que consiste en un conjunto de datos públicos de las empresas identificadas por _dominio_.

> Nota: No se debe de confundir el término _open source_ de OSINT con el utilizado en software, que es software cuyo aquel cuyo código fuente está disponible públicamente, lo que permite a los usuarios examinar, modificar y redistribuir el software. Esta disponibilidad del código fuente facilita la colaboración, la revisión y el desarrollo de nuevas características.

## Dominios

Un **dominio** es una denominación de un grupo de dispositivos o equipos conectados a una red que comparten una administración centralizada. Por lo tanto, un **subdominio** es un subconjunto de un dominio.

Ejemplos:

```sh
# Esto es un dominio
google.com
# Esto es un subdominio
images.google.com
```

Si una organización crece más y más, sus departamentos y áreas se tiende a especializar, como Google que en un principio su _producto estrella_ era sólo el buscador, pero cuando ya encontraron más posibilidades de su expertise, crearon un _buscador de imágenes_.

La notación de los correos electrónicos (@unaempresa.com) es un ejemplo de su uso.

## Sistemas OSINT

Los datos de OSINT pueden buscarse mediante sitios web que buscando un _dominio_ se puede obtener _todas las direcciones de correo electrónico además de datos públicos_ de los usuarios registrados en el OSINT. Por eso se hacía referencia a _esfuerzo conjunto_, poniendo las empresas la información a disposición OSINT de manera pública.

Los 2 sitios web OSINT que veremos son:

- https://hunter.io
- https://phonebook.cz

> Nota: Cuando visitas phonebook.cz indica que _sólo los usuarios de pago serán capaces de usarlo por abuso de cuentas spam_. Hay un aviso en un alert color azul que indica la alternativa, _"_IntelligenceX"_.

### Uso de hunter.io

1. Entra a hunter.io en tu navegador de Kali Linux (se puede hacer de cuando equipo, no sólamente de una Máquina Virtual).
2. Crear una cuenta de hunter.io, llenando uno de tus correos electrónicos, nombre y contraseña. Confirma la cuenta con el correo de validación que te envian.
3. hunter.io funciona en su plan gratuito con _créditos_ para sus funciones relevantes. Una vez que hayas creado la cuenta, te presentarán un video tour que te dará tus primeros creditos adicionales (10 o 15, no recuerdo) una vez que lo termines de ver todo (4-5 min. aproximadamente).
4. En el _Home_ de hunter.io, en la barra lateral de navegación, haz clic en la herramienta _Finder_ (buscador). Inserta un dominio  como _hunter.io_ (es el dominio de la compañia que creó la herramienta), clic en el botón _Find emails_, ¡y veras los resultados! los emails por defecto aparecen oclutos, así que te va a costar _1 crédito_ para desbloquear c/uno.

### Uso de phonebook.cz (_IntelligenceX)

1. Entra a phonebook.cz en tu navegador de Kali Linux (se puede hacer de cuando equipo, no sólamente de una Máquina Virtual).
2. Por razones de cuentas spam, sólo los usuarios de pago pueden usar phonebook.cz (3/6/2025). Arriba de la barra de búsqueda, hay un alert azul que indica que hay una alternativa, _IntelligenceX, la versión gratuita, y para usarla hay que crear una cuenta de _intelx.io_, llenando uno de tus correos electrónicos, nombre y contraseña. Confirma la cuenta con el correo de validación que te envian.
3. En el _Home_ de intelx.io, Inserta un dominio como _hunter.io_ (es el dominio de la compañia que creó la herramienta anterior) en la barra de búsqueda, clic en el botón _Search_, ¡y veras los resultados! Los resultados son _más especializados_ que en hunter.io, teniendo como filtros gratuitos _4 buckets, varias extensiones de dominio y ubicación geográfica_.

## Usos

Solo diremos que _la información es poder_, y una base de datos como **OSINT** es una herramienta poderosa en tu arsenal de hacker ético.
