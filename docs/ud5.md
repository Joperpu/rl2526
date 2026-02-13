# Unidad 5 - Instalación y configuración de equipos en una red

## Direccionamiento IP

Repaso del direccionamiento IP visto en la [unidad 2](ud2.md#direccionamiento-ip).

### Subnetting

Hasta ahora hemos trabajado con direcciones IP “completas”, donde una red se identifica por su ID de red y todos los hosts pertenecen a esa misma red. Sin embargo, en redes reales esto no siempre es eficiente.

El subnetting (o subneteo) consiste en dividir una red IP en varias subredes más pequeñas, reutilizando parte de los bits del identificador de host para crear nuevos identificadores de red.

Mediante el subnetting se consigue:

- Mejor organización de la red.
- Reducción del tráfico de broadcast.
- Uso más eficiente de las direcciones IP.
- Separación lógica de departamentos, aulas o servicios.

Supongamos una red de clase C con dirección 192.168.1.0/24. Esta red permite 254 hosts, pero si solo se necesitan 20 equipos por aula y hay varias aulas, la mayoría de direcciones quedarían sin usar.

En lugar de crear varias redes distintas, se puede dividir una única red en varias subredes, cada una con su propia dirección de red y broadcast.

En subnetting se produce lo siguiente:

- Se toman bits del identificador de host.
- Esos bits pasan a formar parte del identificador de red.
- Al aumentar los bits de red:
- Aumenta el número de subredes.
- Disminuye el número de hosts por subred.

Este proceso se denomina préstamo de bits.

#### Máscara de red en subnetting

Recordemos que la máscara de red indica qué bits pertenecen a la red y cuáles al host.

Al subnetear:

- La máscara de red aumenta el número de bits a 1.
- Esto se refleja tanto en formato decimal como en notación CIDR.

Ejemplo:

- Máscara original: /24 → 255.255.255.0
- Máscara tras subnetting: /26 → 255.255.255.192

#### Subnetting en redes de clase C

Las redes de clase C son las más utilizadas para aprender subnetting, ya que parten de:

- 24 bits de red
- 8 bits de host

Ejemplo de red base:

- Red: 192.168.1.0/24
- Hosts posibles: 254

**Subdivisión en 2 subredes**

Para dividir la red en 2 subredes:

- Se necesita 1 bit adicional para red.
- Nueva máscara: /25

Resultados:

- Subredes: 2
- Hosts por subred: 126

Subredes resultantes:

- 192.168.1.0/25 → hosts 192.168.1.1 – 192.168.1.126
- 192.168.1.128/25 → hosts 192.168.1.129 – 192.168.1.254

**Subdivisión en 4 subredes**

Para obtener 4 subredes:

- Se toman 2 bits del host.
- Nueva máscara: /26

Resultados:

- Subredes: 4
- Hosts por subred: 62

Subredes:

- 192.168.1.0/26
- 192.168.1.64/26
- 192.168.1.128/26
- 192.168.1.192/26

**Cálculo rápido del tamaño de bloque**

El salto entre subredes se calcula restando el valor del último octeto de la máscara a 256.

Ejemplo:

- Máscara: 255.255.255.192
- Último octeto: 192
- Tamaño de bloque: 256 − 192 = 64

Esto indica que cada subred empieza cada 64 direcciones.

**Dirección de red y dirección de broadcast en subnetting**

En cada subred:

- La primera dirección es la dirección de red.
- La última dirección es la dirección de broadcast.
- Las direcciones intermedias se asignan a hosts.

Ejemplo:

- Subred: 192.168.1.64/26
- Red: 192.168.1.64
- Broadcast: 192.168.1.127
- Hosts: 192.168.1.65 – 192.168.1.126


<!-- 
## Configuración y administración de conmutadores

En el modelo OSI hay dos capas tan estrechamente relacionadas que, en el modelo TCP/IP, se consideran esencialmente una sola capa. Estas son la capa de enlace de datos y la capa física en el modelo OSI; en el modelo de Internet, se conocen conjuntamente como la capa de acceso a la red.

La capa de enlace de datos del modelo OSI (Capa 2) tiene las siguientes responsabilidades:

- **Detección y corrección de errores**: Su función principal es identificar y corregir todos los errores que puedan ocurrir en la línea de comunicación dentro de la misma red.
- **Control de flujo**: Se asegura de que un emisor rápido no sature a un receptor más lento, evitando así la pérdida innecesaria de datos.
- **Gestión del medio compartido**: En redes donde existe un único medio compartido para la transmisión de información, esta capa se encarga de distribuir su uso entre los diferentes nodos.

En la notación de la capa 2, los dispositivos de red conectados al medio de transmisión se denominan nodos. Estos nodos son responsables de crear y reenviar tramas. La capa de enlace de datos OSI es la encargada de gestionar el intercambio de tramas Ethernet entre los nodos de origen y destino a través de un medio de red físico.

### Ethernet

Ethernet es la tecnología LAN más utilizada a nivel mundial. Opera en las capas de enlace de datos y física del modelo OSI, y corresponde a la capa de acceso al medio en el modelo TCP/IP. Es una familia de tecnologías de red definidas en los estándares IEEE 802.2 y 802.3.

Los estándares de Ethernet abarcan tanto los protocolos de la capa 2 como las tecnologías de la capa 1. Ethernet divide sus funciones en dos subcapas:

- **Subcapa LLC (Logical Link Control)**: Esta subcapa toma los datos del protocolo de red, generalmente un paquete IPv4, y añade información de control para facilitar su entrega al nodo de destino. En un ordenador, el LLC puede considerarse como el software del controlador de la tarjeta de red (NIC, Network Interface Card).
- **Subcapa MAC (Media Access Control)**: Es la subcapa inferior de la capa de enlace de datos y se implementa en hardware, usualmente en la tarjeta de red del ordenador.

![Estándares Ethernet](assets/images/ud5/img01.png){ width="700" }

La subcapa MAC de Ethernet tiene dos funciones principales:

- **Encapsulación de datos**: Este proceso incluye la creación de tramas antes de la transmisión y su desensamblaje al recibirlas. Para formar la trama, la capa MAC añade un encabezado y una cola al paquete de la capa de red, que ya contiene la información de control añadida por la subcapa LLC.
- **Control de acceso al medio**: Es responsable de colocar las tramas en el medio de transmisión (en el origen) y de extraerlas (en el destino). Esta subcapa se comunica directamente con la capa física. A medida que los paquetes se transfieren desde el host de origen al de destino, suelen atravesar diferentes redes físicas que pueden incluir diversos tipos de medios de transmisión, como cables de cobre, fibra óptica y tecnologías inalámbricas. El control de acceso al medio es el conjunto de técnicas que gestionan cómo se colocan y extraen las tramas del medio físico de transmisión.

#### La trama Ethernet

Anteriormente, mencionamos que la unidad de datos del protocolo en la capa de enlace se conoce como trama. La capa de enlace de datos prepara los paquetes para su transmisión a través de los medios físicos encapsulándolos con un encabezado y un tráiler, formando así una trama completa. Esta trama se compone de tres partes fundamentales:

- Encabezado: Contiene información de direccionamiento y control necesaria para el envío y la recepción.
- Datos: Corresponde a la Unidad de Datos de Protocolo (PDU) de nivel 3, generalmente un paquete IP.
- Tráiler: Incluye información para la detección y posible corrección de errores.

![Trama Ethernet](assets/images/ud3/img02.png){ width="700" }

#### Dirección MAC

Una dirección MAC en Ethernet es un número binario de 48 bits, habitualmente representado mediante 12 dígitos hexadecimales (cada dígito hexadecimal equivale a 4 bits). Esta dirección se divide en dos partes:

- **Identificador Único de Organización (OUI)**: Los primeros 3 bytes (6 dígitos hexadecimales) que identifican al fabricante de la tarjeta de red. Cada fabricante posee un OUI único asignado.
- **Identificador de Dispositivo**: Los últimos 3 bytes, asignados por el fabricante, que aseguran que cada dirección MAC sea única para un dispositivo en particular.

No existen dos direcciones MAC idénticas; cada una es exclusiva para un dispositivo específico. Los fabricantes se encargan de que los tres últimos bytes sean diferentes para cada dispositivo, y cada uno tiene un OUI distinto.

Las direcciones MAC se expresan como seis pares de valores hexadecimales, donde cada par representa un byte. Por ejemplo, una dirección MAC puede ser 00-18-DE-DD-A7-B2. Generalmente, se utilizan guiones o dos puntos para separar cada byte. Existen tres tipos principales de direcciones MAC:

- **Unidifusión**: Es una dirección MAC única utilizada cuando se envía una trama desde un solo dispositivo emisor hacia un solo dispositivo receptor.
- **Difusión**: Una trama dirigida a una dirección de difusión será recibida y procesada por todos los hosts de esa red local (dominio de difusión). La dirección MAC de difusión es FF-FF-FF-FF-FF-FF en hexadecimal, lo que equivale a 48 unos en binario.
- **Multidifusión**: Permiten que un dispositivo de origen envíe una trama a un grupo específico de dispositivos. Una dirección MAC de multidifusión asociada a una dirección de multidifusión IPv4 comienza con 01-00-5E en hexadecimal. El resto de la dirección se forma convirtiendo los 23 bits inferiores de la dirección IP del grupo de multidifusión en seis dígitos hexadecimales.

Las direcciones MAC no se asignan mediante software; a menudo se les llama direcciones físicas porque están grabadas físicamente en la memoria ROM de la tarjeta de red. Es decir, la dirección está codificada de forma permanente en el chip ROM. Cualquier dispositivo que pueda ser el origen o destino de una trama Ethernet debe tener una dirección MAC asignada. Esto incluye computadoras, servidores, impresoras, dispositivos móviles y routers.

Estas direcciones físicas no indican a qué red está conectado el dispositivo. Si el dispositivo se mueve a otra red o subred, continúa operando con la misma dirección física sin necesidad de cambios.

### Switches

Un switch, también conocido como conmutador, es un dispositivo de interconexión utilizado para conectar equipos en red, formando una red de área local (LAN). Sus especificaciones técnicas cumplen con el estándar Ethernet. En la actualidad, las redes locales cableadas se basan en Ethernet, empleando una topología en estrella donde el switch actúa como el elemento central de esta configuración.

El switch opera en la capa 2 del modelo OSI y en la capa de acceso a red del modelo de Internet. Al ser un dispositivo diseñado para redes Ethernet, trabaja exclusivamente con tramas y no tiene conocimiento del protocolo que se transmite en la sección de datos de la trama, como podría ser un paquete IPv4. Las decisiones de reenvío que toma el switch se basan únicamente en las direcciones MAC Ethernet de la capa de enlace.

La cantidad de dispositivos que se pueden conectar a un switch depende del número de puertos que este posea. Existen switches con 4, 8, 16, 32 y hasta 48 puertos. Cada puerto puede conectar un dispositivo como un ordenador, impresora, servidor, entre otros. Si se requiere expandir la red para incluir más nodos, es posible conectar un switch a un puerto de otro switch, aumentando así el número de puertos disponibles. Todos los dispositivos conectados a estos switches formarían parte de la misma red local.

#### Tabla de direcciones MAC

Un switch crea y mantiene una tabla de direcciones MAC que utiliza para decidir cómo reenviar una trama. Cada vez que recibe una trama, consulta esta tabla para determinar por cuál puerto debe enviarla. Esta tabla puede tener tantas entradas como puertos tiene el switch y asocia un número de puerto con la dirección MAC del dispositivo conectado a ese puerto. Básicamente, un switch realiza dos operaciones principales:

- **Aprendizaje (Examinar la dirección MAC de origen)**: Cada vez que llega una trama al switch, este examina la dirección MAC de origen de la trama y el número de puerto por el que llegó. Si la dirección MAC de origen no está en la tabla, se agrega junto con el número de puerto de entrada. Si ya existe, el switch actualiza el temporizador de esa entrada. Por defecto, la mayoría de los switches Ethernet mantienen una entrada en la tabla durante cinco minutos. Si la dirección MAC de origen está en la tabla pero asociada a un puerto diferente, el switch la trata como una nueva entrada y reemplaza la anterior con el número de puerto más reciente.
- **Reenvío (Examinar la dirección MAC de destino)**: A continuación, si la dirección MAC de destino es una dirección de unidifusión, el switch busca en la tabla una coincidencia con la dirección MAC de destino de la trama. Si la encuentra, reenvía la trama por el puerto especificado. Si no está en la tabla, el switch reenvía la trama por todos los puertos excepto el de entrada, lo que se conoce como unidifusión desconocida. Si la dirección MAC de destino es de difusión o multidifusión, la trama también se envía por todos los puertos excepto el de entrada.

Cuando un dispositivo tiene una dirección IP en una red remota, la trama Ethernet no puede enviarse directamente al dispositivo de destino. En su lugar, la trama Ethernet se envía a la dirección MAC del gateway predeterminado, es decir, el router.

#### Protocolo ARP

En una red LAN Ethernet, cada dispositivo cuenta con dos direcciones asignadas:

- **Dirección física (dirección MAC)**: Utilizada para las comunicaciones entre tarjetas de red Ethernet (NIC) dentro de la misma red.
- **Dirección lógica (dirección IP)**: Empleada para enviar paquetes desde el origen inicial hasta el destino final.

Las direcciones IP se utilizan para identificar el origen y el destino de los paquetes. La dirección IP de destino puede pertenecer a la misma red IP que el origen o encontrarse en una red remota.

Por otro lado, las direcciones MAC de Ethernet tienen un propósito distinto: se usan para entregar la trama, que contiene el paquete IP encapsulado, de un dispositivo a otro dentro de la misma red. Si la dirección IP de destino está en la misma red, la dirección MAC de destino en la trama será la del dispositivo final.

![ARP 1](assets/images/ud3/img03.png){ width="700" }

En la figura anterior, observamos que la trama Ethernet de capa 2 incluye lo siguiente:

- **Dirección MAC de destino**: Es la dirección MAC de la NIC Ethernet del servidor de archivos.
- **Dirección MAC de origen**: Corresponde a la dirección MAC de la NIC Ethernet del PC-A.

El paquete IP de capa 3 contiene:

- **Dirección IP de origen**: Es la dirección IP del PC-A, el origen inicial.
- **Dirección IP de destino**: Es la dirección IP del servidor de archivos, el destino final.

Sin embargo, cuando la dirección IP de destino está en una red remota, la dirección MAC de destino en la trama es la del gateway (puerta de enlace) predeterminado del host.

Cuando el gateway recibe una trama Ethernet, desencapsula la información de capa 2. A través de la dirección IP de destino, determina el siguiente dispositivo en la ruta y encapsula el paquete IP en una nueva trama de enlace de datos para la interfaz de salida. En cada enlace a lo largo del camino, el paquete IP se encapsula en una trama específica para la tecnología de enlace de datos correspondiente a ese enlace. Si el siguiente salto es el destino final, la dirección MAC de destino será la de la NIC Ethernet de ese dispositivo.

![ARP 2](assets/images/ud3/img04.png){ width="700" }

Entonces, ¿cómo se asocian las direcciones IPv4 de los paquetes con las direcciones MAC en cada enlace durante el trayecto hacia el destino? Esto se logra mediante un proceso conocido como Protocolo de Resolución de Direcciones (ARP).

Para determinar la dirección MAC de destino, el dispositivo utiliza el protocolo ARP, que proporciona dos funciones esenciales:

- Resolución de direcciones IPv4 a direcciones MAC.
- Mantenimiento de una tabla de asignaciones.

##### Resolución de direcciones IPv4

Cuando un paquete es enviado a la capa de enlace de datos para ser encapsulado en una trama Ethernet, el dispositivo consulta una tabla en su memoria para encontrar la dirección MAC asociada a la dirección IPv4 correspondiente. Esta tabla se conoce como tabla ARP o caché ARP.

El dispositivo emisor busca en su tabla ARP la dirección IPv4 de destino para obtener la dirección MAC correspondiente:

- Si la dirección IPv4 de destino está en la misma red que la dirección IPv4 de origen, el dispositivo busca en la tabla ARP la dirección IPv4 de destino.
- Si la dirección IPv4 de destino está en una red diferente a la de origen, el dispositivo busca en la tabla ARP la dirección IPv4 del gateway (puerta de enlace) predeterminado.

En ambos casos, se realiza una búsqueda para encontrar la dirección MAC asociada a la dirección IPv4. Cada entrada en la tabla ARP vincula una dirección IPv4 con una dirección MAC; esta relación se denomina asignación. La tabla ARP almacena temporalmente estas asignaciones en caché para los dispositivos de la misma LAN.

Si el dispositivo encuentra la dirección IPv4 en la tabla, utiliza la dirección MAC correspondiente como la dirección MAC de destino en la trama. Si no hay una entrada, el dispositivo envía una solicitud ARP.

##### Funcionamiento del protocolo ARP

Se envía una solicitud ARP cuando un dispositivo necesita conocer la dirección MAC asociada a una dirección IPv4 y no tiene una entrada para esa dirección en su tabla ARP.

El protocolo ARP opera en la capa de enlace de datos, por lo que los mensajes ARP se encapsulan directamente dentro de una trama Ethernet. El mensaje de solicitud ARP incluye:

- **Dirección IPv4 objetivo**: La dirección IPv4 del dispositivo cuya dirección MAC se desea conocer.
- **Dirección MAC objetivo**: Esta es la dirección MAC desconocida; en la solicitud ARP, este campo está vacío.

La solicitud ARP se encapsula en una trama Ethernet con el siguiente encabezado:

- **Dirección MAC de destino**: Es la dirección MAC de difusión (FF:FF:FF:FF:FF:FF), lo que obliga a que todos los nodos de la LAN acepten y procesen la solicitud ARP.
- **Dirección MAC de origen**: La dirección MAC del dispositivo que envía la solicitud ARP.
- **Tipo**: Un campo con el valor 0x806, que indica al receptor que los datos de la trama deben ser procesados por ARP.

Dado que las solicitudes ARP son de difusión, el switch las envía por todos los puertos excepto por el que las recibió. Cada dispositivo en la red recibe y procesa la solicitud ARP para verificar si la dirección IPv4 objetivo coincide con la suya. Si coincide, ese dispositivo será el único que envíe una respuesta ARP. Los demás dispositivos no responden.

El mensaje de respuesta ARP incluye:

- **Dirección IPv4 del remitente**: La dirección IPv4 del dispositivo cuya dirección MAC se solicitó.
- **Dirección MAC del remitente**: La dirección MAC buscada por el dispositivo que hizo la solicitud ARP; es decir, el objetivo del proceso.

La respuesta ARP se encapsula en una trama Ethernet con el siguiente encabezado:

- **Dirección MAC de destino**: La dirección MAC del dispositivo que envió la solicitud ARP.
- **Dirección MAC de origen**: La dirección MAC del dispositivo que responde.
- **Tipo**: Un campo con el valor 0x806, indicando que los datos de la trama deben ser procesados por ARP.

Solo el dispositivo que envió la solicitud ARP inicial recibe la respuesta ARP en unidifusión. Una vez que recibe la respuesta, agrega la dirección IPv4 y su correspondiente dirección MAC a su tabla ARP. A partir de ese momento, los paquetes destinados a esa dirección IPv4 pueden ser encapsulados en tramas con la dirección MAC correcta.

Cuando la dirección IPv4 de destino no está en la misma red que la dirección IPv4 de origen—es decir, el paquete está dirigido a otra red—el dispositivo de origen debe enviar la trama al gateway predeterminado, que es la interfaz del router local.

La dirección IPv4 del gateway se almacena en la configuración de red de los hosts. Cuando un host crea un paquete, compara su propia dirección IPv4 con la dirección IPv4 de destino para determinar si están en la misma red. Si no lo están, el dispositivo busca en su tabla ARP una entrada que contenga la dirección IPv4 del gateway. Si no existe dicha entrada, utiliza el proceso ARP para determinar la dirección MAC del gateway, tal como se describió anteriormente.

Cada dispositivo tiene un temporizador en su caché ARP que elimina las entradas que no se han utilizado durante un período específico. Este tiempo varía según el sistema operativo del dispositivo; por ejemplo, algunos sistemas Windows mantienen las entradas ARP en caché durante dos minutos.

Además, es posible utilizar comandos para eliminar manualmente todas o algunas de las entradas de la tabla ARP. Después de eliminar una entrada, el proceso de enviar una solicitud ARP y recibir una respuesta ARP debe repetirse para recrear la entrada en la tabla ARP.

#### Versiones de Ethernet

Hace mucho tiempo que Ethernet se convirtió en el protocolo estándar en nivel de enlace de las LAN. Durante todo este tiempo la tecnología de red ha evolucionado y Ethernet con ella, permitiendo la instalación de una red Ethernet con diferentes topologías, tipos de cableado y velocidades de transmisión. Los principales se resumen en la siguiente tabla.

| Tecnología | Velocidad de transmisión | Tipo de cable | Distancia máxima | Topología |
| ------- | ------ | ------ | ------ | ------ |
| 10Base2 | 10 Mbit/s | Coaxial | 185 m | Bus (Conector T) |
| 10BaseT | 10 Mbit/s | Par trenzado | 100 m | Estrella (Hub o Switch) |
| 10BaseF | 10 Mbit/s | Fibra óptica | 2000 m | Estrella (Hub o Switch) |
| 100BaseT4 | 100 Mbit/s | Par Trenzado (categoría 3UTP) | 100 m | Estrella, half-duplex y full-duplex |
| 100BaseTX | 100 Mbit/s | Par Trenzado (categoría 5UTP) | 100 m | Estrella, half-duplex y full-duplex |
| 100BaseFX | 100 Mbit/s | Fibra óptica | 2000 m | Estrella y full-duplex |
| 1000BaseT | 1000 Mbit/s | Par Trenzado (categoría 5e o 6UTP) | 100 m | Estrella y full-duplex |
| 1000BaseSX | 1000 Mbit/s | Fibra óptica (multimodo) | 550 m | Estrella y full-duplex |
| 1000BaseLX | 1000 Mbit/s | Fibra óptica (monomodo) | 5000 m | Estrella y full-duplex |

### Configuración de switches

La mayor parte de los switches son autoconfigurables. Al conectar dispositivos de red a sus puertos, comienzan a operar de manera independiente, gestionando el flujo de datos en la capa de enlace y dirigiéndolo hacia los recursos conectados en la red. No obstante, existen switches más avanzados que incorporan un sistema operativo y/o una aplicación web para su configuración. Entre estos, los más reconocidos son los de la marca Cisco, una multinacional tecnológica responsable de más del 60% de los routers en Internet.

Los switches de Cisco ejecutan el sistema operativo Cisco IOS y permiten una configuración manual para adaptarse mejor a las necesidades específicas de la red. Esto incluye ajustar parámetros como la velocidad, el ancho de banda y la seguridad de los puertos. Además, los switches Cisco pueden ser administrados tanto localmente como de forma remota. Para gestionar un switch de manera remota, es necesario configurarlo con una dirección IP y una puerta de enlace predeterminada.

#### Archivos de configuración

Existen dos archivos principales que se utilizan para almacenar la configuración de los dispositivos:

- **startup-config**: Este archivo se guarda en la memoria de acceso aleatorio no volátil (NVRAM) y contiene todos los comandos que el IOS ejecutará durante el arranque o reinicio. La NVRAM es una memoria que mantiene su contenido incluso cuando el dispositivo está apagado.

- **running-config**: Este archivo reside en la memoria de acceso aleatorio (RAM) y refleja la configuración actual del dispositivo. Cualquier modificación realizada en la configuración en ejecución afecta de inmediato al funcionamiento del dispositivo Cisco. La RAM es una memoria volátil, por lo que pierde todo su contenido cuando el dispositivo se apaga o reinicia.

Para visualizar el archivo de configuración en ejecución, se puede utilizar el comando show running-config en el modo EXEC privilegiado. Si se desea ver el archivo de configuración de inicio, se ejecuta el comando show startup-config en el mismo modo.

Si el dispositivo pierde energía o se reinicia, todos los cambios de configuración se perderán a menos que hayan sido guardados. Para conservar los cambios realizados en la configuración en ejecución en el archivo de configuración de inicio, se utiliza el comando copy running-config startup-config en el modo EXEC privilegiado.

En caso de que los cambios efectuados en la configuración en ejecución no produzcan el efecto deseado y el archivo running-config aún no haya sido guardado, es posible restablecer el dispositivo a su configuración anterior eliminando los comandos modificados o recargando el dispositivo con el comando reload en el modo EXEC privilegiado para restaurar la configuración de inicio.

La desventaja de usar el comando reload para eliminar una configuración en ejecución no guardada es que el dispositivo estará fuera de servicio durante un breve período, lo que ocasiona un tiempo de inactividad en la red.

Al iniciar una recarga, el IOS detecta que la configuración en ejecución tiene cambios no guardados en comparación con la configuración de inicio. Aparecerá un mensaje preguntando si se desean guardar los cambios. Para descartarlos, se debe introducir n o no.

Si, por el contrario, se han guardado cambios no deseados en la configuración de inicio, puede ser necesario eliminar todas las configuraciones. Esto implica borrar la configuración de inicio y reiniciar el dispositivo. La configuración de inicio se elimina utilizando el comando erase startup-config en el modo EXEC privilegiado.

Después de borrar la configuración de inicio de la NVRAM, se debe recargar el dispositivo para eliminar el archivo de configuración en ejecución actual de la RAM. Al reiniciarse, el switch cargará la configuración de inicio predeterminada que venía originalmente con el dispositivo.

#### Verificación de la configuración del switch

Disponemos de algunas de las opciones del comando show que son útiles para verificar las características configurables comunes de un switch. Se exponen en la siguiente tabla:

| Comando | Descripción |
| ------ | ------ |
| show startup-config | Muestra la configuración de inicio |
| show running-config | Muestra la configuración de funcionamiento actual |
| show flash | Muestra información sobre el sistema de archivos flash |
| show version | Muestra el estado del hardware y software del sistema |
| show history | Muestra el historial de comandos introducidos |

#### Nombres de dispositivo

Al configurar un dispositivo de red, uno de los primeros pasos es asignarle un nombre de dispositivo único o nombre de host. Los nombres de host aparecen en el indicador del CLI, pueden utilizarse en varios procesos de autenticación entre dispositivos y deben incluirse en los mapas de red.

Si no se configura explícitamente un nombre para el dispositivo, Cisco IOS asigna un nombre predeterminado de fábrica. En el caso de los switches Cisco IOS, el nombre predeterminado es Switch. Si todos los dispositivos de red mantuvieran el nombre predeterminado, sería difícil identificar cada uno de manera individual. Por ejemplo, al acceder a un dispositivo remoto mediante SSH, es crucial confirmar que se está conectado al dispositivo correcto.

Elegir nombres adecuados facilita recordar, analizar e identificar los dispositivos en la red. Los requisitos para los nombres son:

- Comenzar con una letra.
- No contener espacios.
- Terminar con una letra o un dígito.
- Contener letras, dígitos o guiones medios.
- Tener una longitud máxima de 64 caracteres.

Los nombres de host utilizados en el IOS del dispositivo distinguen entre mayúsculas y minúsculas. Para configurar un nombre de host en el dispositivo se deben seguir los siguientes pasos:
	
1. Iniciar sesión en el dispositivo utilizando uno de los métodos mencionados anteriormente.
2. Acceder al modo privilegiado con el comando **enable**.
3. Acceder al modo de configuración global con el comando **configure terminal**.
4. Ejecutar el comando **hostname** seguido del nombre deseado para el switch.

#### Contraseñas

Los dispositivos de red, incluso los enrutadores inalámbricos para uso doméstico, deben tener siempre contraseñas configuradas para limitar el acceso administrativo. El Cisco IOS puede configurarse para utilizar contraseñas de forma jerárquica y permitir diferentes niveles de privilegios de acceso al dispositivo de red.

##### Modo EXEC privilegiado

La contraseña más importante a configurar es la que permite el acceso al modo EXEC privilegiado. Para proteger este acceso, utilice el comando de configuración global como se muestra en el siguiente ejemplo, donde establecemos la clave “cisco” para ingresar al modo EXEC privilegiado.

```
Switch>enable
Switch#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#enable secret cisco
Switch(config)#
```

Tras esto, si salimos del modo de configuración global y EXEC privilegiado podemos comprobar que si volvemos a entrar en él nos pedirá la clave.

##### Modo EXEC de usuario

Para proteger el acceso a EXEC de usuario, el puerto de consola debe estar configurado. Para ello podemos realizar los siguiente:

```
Switch>enable
Switch#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#line console 0
Switch(config-line)#password clase
Switch(config-line)#login
Switch(config-line)#exit
Switch(config)#
```

Accedemos al modo de configuración de la línea de consola con el comando global line console 0. El número cero se usa para indicar la primera (y en la mayoría de los casos, la única) interfaz de consola. Luego, establecemos la contraseña para el modo EXEC de usuario con el comando password contraseña. Finalmente, activamos el acceso al modo EXEC de usuario utilizando el comando login. A partir de ahora, el acceso a la consola requerirá una contraseña antes de poder entrar en el modo EXEC de usuario.

##### Cifrado de contraseñas

Los archivos **startup-config** y **running-config** muestran la mayoría de las contraseñas en texto plano. Esto representa una amenaza de seguridad, ya que cualquier persona con acceso a estos archivos puede ver las contraseñas utilizadas.

Para cifrar las contraseñas, se puede utilizar el comando global de configuración **service password-encryption**. Este comando aplica un cifrado básico a todas las contraseñas que no estén cifradas. Cabe destacar que este cifrado solo se aplica a las contraseñas dentro del archivo de configuración y no a las contraseñas mientras se transmiten por la red. El objetivo de este comando es prevenir que individuos no autorizados puedan visualizar las contraseñas en el archivo de configuración.

##### Mensajes de aviso

Si bien las contraseñas son una forma de impedir que personas no autorizadas accedan a la red, es fundamental proporcionar un método que informe que solo el personal autorizado debe intentar acceder al dispositivo. Para lograr esto, podemos añadir un mensaje de aviso al inicio de sesión del dispositivo. Estos avisos pueden ser cruciales en procesos legales en caso de demandas por acceso no autorizado. En algunos sistemas legales, no se permite la acusación ni siquiera el monitoreo de usuarios a menos que haya una notificación visible.

Para crear un mensaje de aviso del día en un dispositivo de red, utilice el comando de configuración global **banner motd #mensaje del día#**. El símbolo # en la sintaxis del comando se denomina carácter delimitador y se coloca antes y después del mensaje. El delimitador puede ser cualquier carácter siempre que no aparezca dentro del mensaje, por lo que a menudo se usan símbolos como #. Una vez ejecutado el comando, el aviso aparecerá en todos los intentos posteriores de acceso al dispositivo hasta que se elimine.

Dado que cualquier persona que intente iniciar sesión puede ver estos avisos, es importante redactar el mensaje con cuidado. El contenido o las palabras exactas del aviso dependerán de las leyes locales y de las políticas de la empresa. Debe dejar claro que solo el personal autorizado tiene permiso para acceder al dispositivo. Además, el aviso puede incluir información sobre cierres programados del sistema y otras notificaciones que afecten a todos los usuarios de la red.

#### Gestión de la tabla de direcciones MAC

Aunque la tabla MAC de un switch se llena habitualmente de manera dinámica, también es posible realizar ciertas operaciones manuales sobre ella. Entre las opciones disponibles encontramos las siguientes.

#### Visualizar la tabla de direcciones MAC

Para comprobar el contenido de la tabla de direcciones MAC, basta con utilizar el comando show mac-address-table desde el modo EXEC privilegiado. A continuación, se muestra un ejemplo:

```
Switch#show mac-address-table
      Mac Address Table
-------------------------------------------
Vlan Mac Address Type Ports
---- ----------- -------- -----
 1 0001.6375.a091 DYNAMIC Fa0/1
 1 0030.f231.a174 DYNAMIC Fa0/6
 1 00d0.ff1a.04a2 DYNAMIC Fa0/5
Switch#
```

#### Borrado de la tabla de direcciones MAC

Cuando un switch se reinicia, su tabla MAC se vacía automáticamente. Sin embargo, si deseamos eliminar manualmente todas las entradas de la tabla MAC sin necesidad de reiniciar el switch, podemos utilizar el comando clear mac-address-table desde el modo EXEC privilegiado. A continuación, se muestra un ejemplo de cómo realizarlo.

```
Switch#show mac-address-table
 Mac Address Table
-------------------------------------------
Vlan Mac Address Type Ports
---- ----------- -------- -----
 1 0001.6375.a091 DYNAMIC Fa0/1
 1 0030.f231.a174 DYNAMIC Fa0/6
 1 00d0.ff1a.04a2 DYNAMIC Fa0/5
Switch#
Switch#clear mac-address-table
Switch#show mac-address-table
 Mac Address Table
-------------------------------------------
Vlan Mac Address Type Ports
---- ----------- -------- -----
Switch#
```

Este procedimiento es especialmente útil para reiniciar el proceso de aprendizaje de direcciones MAC sin afectar el funcionamiento continuo del switch.

### Seguridad del switch

En los siguientes apartados veremos aspectos relativos a la seguridad del switch.

####  Puertos sin utilizar

Un método sencillo que muchos administradores emplean para mejorar la seguridad de la red y prevenir accesos no autorizados es desactivar todos los puertos del switch que no estén en uso. Por ejemplo, si un switch cuenta con 24 puertos y solo tres conexiones Fast Ethernet están activas, es recomendable inhabilitar los 21 puertos restantes que no se están utilizando.

Para deshabilitar un puerto específico, se utiliza el comando shutdown en el modo de configuración del puerto correspondiente.

A continuación, se muestra un ejemplo de cómo desactivar el puerto 4 de un switch:

```
Switch>enable
Switch#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#interface fa0/4
Switch(config-if)#shutdown

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to
administratively down
Switch(config-if)#exit
Switch(config)#
```

En caso de que sea necesario reactivar un puerto previamente deshabilitado, se puede habilitar utilizando el comando no shutdown.

Deshabilitar puertos uno por uno puede ser una tarea extensa y tediosa. Para simplificar este proceso, es posible aplicar configuraciones a varios puertos de forma simultánea. Cuando se necesita configurar un rango de puertos en un switch, se emplea el comando interface range.

A continuación, se muestra un ejemplo de cómo deshabilitar los puertos del 5 al 24 de un switch:

```
Switch>enable
Switch#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#interface range fa0/5-24
Switch(config-if-range)#shutdown

%LINK-5-CHANGED: Interface FastEthernet0/5, changed state to
administratively down
%LINK-5-CHANGED: Interface FastEthernet0/6, changed state to
administratively down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to
administratively down
Switch(config-if-range)#exit
Switch(config)#exit
Switch#
```

En el ejemplo anterior, se ha omitido la salida generada por el cambio de estado de los puertos para simplificar la presentación del comando.

Aunque el proceso de habilitar e inhabilitar puertos pueda resultar largo y laborioso, su implementación contribuye significativamente a mejorar la seguridad de la red, lo que justifica plenamente el tiempo y el esfuerzo invertidos.

#### Seguridad de puertos

Una manera eficaz de proteger los puertos de un switch es mediante la implementación de una funcionalidad llamada seguridad de puertos (port security). Esta característica permite convertir un puerto en seguro configurando los siguientes parámetros:

- **Número máximo de direcciones MAC permitidas en un puerto**: Este parámetro limita la cantidad de dispositivos que pueden conectarse al puerto, ayudando a prevenir accesos no autorizados.
- **Direcciones MAC permitidas en un puerto**: Con este ajuste, se especifican las direcciones MAC de los dispositivos autorizados a conectarse al puerto, de manera que únicamente estos dispositivos puedan acceder al mismo.

Cuando un puerto se configura como seguro y se alcanza el número máximo de direcciones MAC permitidas, cualquier intento de conexión con una dirección MAC no autorizada resultará en una violación de seguridad.

Las direcciones MAC configuradas como seguras no solo se almacenan en la tabla de direcciones MAC estándar del switch, sino también en una tabla específica de direcciones MAC seguras. Esta última se utiliza para gestionar y monitorear las direcciones configuradas como seguras.

A continuación, se muestra un ejemplo basado en un mapa de red para ilustrar esta funcionalidad:

![Seguridad de puertos](assets/images/ud3/img11.png){ width="350" }

En esta red básica, se procederá a configurar la seguridad de puertos en las interfaces Fa0/1 y Fa0/2 para protegerlas frente a accesos no autorizados.

##### Requisitos previos

La funcionalidad de seguridad de puertos requiere los siguientes pasos previos:

1. Configurar el puerto en modo de acceso con el comando switchport mode access.
2. Habilitar la seguridad de puertos en la interfaz utilizando el comando switchport port-security.

##### Configuración en un puerto individual

Para configurar la seguridad de puertos en la interfaz Fa0/1, se ejecutan los siguientes comandos:

```
S0>enable
S0#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
S0(config)#interface fa0/1
S0(config-if)#switchport mode access
S0(config-if)#switchport port-security
S0(config-if)#
```
-->