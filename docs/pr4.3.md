# Práctica 4.3 – Creación y comprobación de rosetas de red RJ45

## Parte 1 – Montaje de la roseta de red

### Descripción

Las rosetas de red constituyen el punto final de una instalación de cableado estructurado. Se emplean para proporcionar una conexión fija y ordenada entre el cableado de red y los equipos finales, como ordenadores, impresoras o teléfonos IP, mejorando la organización, seguridad y mantenimiento de la red.

### Materiales

- Cable de red de categoría 5e o 6.
- Roseta de superficie o empotrada RJ45.
- Módulo o conector hembra RJ45 (keystone).
- Herramienta de inserción (impact tool o punzón).
- Pelacables.
- Destornillador.
- Tester de cableado de red.
- Latiguillo de red.

### Explicación

Las rosetas de red utilizan conectores hembra RJ45 en los que los hilos del cable se insertan siguiendo un código de colores normalizado. Los estándares más utilizados son T568A y T568B, que definen el orden de los pares trenzados en los pines del conector.

En instalaciones de red habituales se emplea el estándar T568B, que garantiza compatibilidad con el cableado existente y con los latiguillos comerciales. Es fundamental respetar el mismo estándar en ambos extremos del cable para asegurar el correcto funcionamiento de la red.

### Desarrollo de la práctica

Cortamos un tramo de cable de red de la longitud necesaria, teniendo en cuenta la distancia desde la canalización hasta la roseta. Se recomienda dejar un pequeño margen adicional de cable para facilitar el trabajo y posibles futuras modificaciones.

Con ayuda del pelacables, retiramos cuidadosamente la cubierta exterior del cable, eliminando aproximadamente 3 o 4 centímetros de aislamiento, procurando no dañar los hilos internos. Una vez retirada la funda, se dejan visibles los cuatro pares trenzados. Se separan y desenrollan ligeramente los pares, manteniendo el trenzado lo más próximo posible al punto de inserción para reducir interferencias.

A continuación, se identifican las ranuras del módulo RJ45, donde suele aparecer serigrafiado el código de colores correspondiente a los estándares T568A y T568B. Se selecciona el estándar T568B y se colocan los hilos siguiendo este orden:

Blanco-Naranja, Naranja, Blanco-Verde, Azul, Blanco-Azul, Verde, Blanco-Marrón, Marrón.

Cada hilo se introduce en su ranura correspondiente presionándolo con la herramienta de inserción hasta que quede firmemente sujeto. El exceso de cable se corta automáticamente o se retira con la herramienta.

Es importante comprobar visualmente que todos los hilos están bien insertados, que siguen el orden correcto y que no sobresalen restos de cobre.

Una vez terminado el conexionado del módulo, se coloca éste dentro de la roseta y se fija correctamente utilizando los tornillos o el sistema de anclaje correspondiente. Finalmente, se cierra la tapa de la roseta asegurándose de que queda bien ajustada y alineada.

<iframe width="560" height="315" src="https://www.youtube.com/embed/rLXrCwlTNGo?si=99qf6xg4P-O9RxlY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Parte 2 - Comprobación del funcionamiento de la roseta

### Descripción

Una vez montada la roseta, es necesario comprobar que la conexión se ha realizado correctamente y que todos los hilos transmiten la señal sin errores.

### Materiales

- Roseta RJ45 terminada.
- Cable de red (latiguillo).
- Tester de cableado.

### Explicación

Para comprobar el funcionamiento, se conecta un latiguillo desde la roseta hasta el tester de red. El otro extremo del cable estructurado se conecta a la unidad remota del tester.

El tester realiza una comprobación secuencial de continuidad y orden de los conductores. Si el cableado es correcto, los LEDs se encenderán de forma ordenada y simultánea en ambos extremos, indicando que no existen cortes ni cruces incorrectos.

Las pruebas deben realizarse siempre con el cable desconectado de cualquier equipo activo y sin tensión eléctrica.

## Criterios evaluación

Esta práctica evalúa todos los criterios de evaluación del **RA2**. Para su corrección se tendrá en cuenta:

- Montaje correcto de la roseta RJ45 siguiendo el estándar indicado (40%).
- Comprobación del funcionamiento mediante tester (30%).
- Documentación clara y completa del proceso con fotografías propias (30%).

## Entrega de la tarea

Documenta todo el proceso con fotografías realizadas por ti, además de escribir el proceso completo que has realizado. Entrega el documento .PDF en el lugar de la plataforma Moodle Centros habilitado para ello, con el siguiente nombre:

**Apellido1Apellido2_Nombre_RL_UD4_P3.pdf**

Además, el profesor deberá comprobar que todo funciona correctamente.