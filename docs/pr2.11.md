# Práctica 2.11 - Creación y configuración de un escenario de red completo

## Descripción de la tarea

Crea y configura un escenario en Cisco Packet Tracer que incluya los siguientes dispositivos:

- 10 ordenadores de escritorio.
- 5 ordenadores portátiles.
- 3 switches.
- 1 router inalámbrico.
- 1 servidor exclusivo para el servicio de DHCP.
- 1 servidor exclusivo para Web.

Requisitos:

- La red poseerá una IP de clase A privada.
- La primera IP disponible de la red será asignada a la puerta de enlace predeterminada.
- Los dos primeros switches segmentarán la red con 5 equipos de escritorio por cada uno de ellos.
- El tercer switch aunará las conexiones de los switches anteriores, servidores y router.
- Los servidores de DHCP y Web poseerán las 3 siguientes IP disponibles, en ese orden, y no serán asignadas dinámicamente.
- Los ordenadores de escritorio tendrán IPs estáticas, desde la primera disponible para todos los hosts de la red en adelante, tras la asignación de las IPs anteriores.
- Los ordenadores portátiles se conectarán a la red a través de su conexión inalámbrica y tendrán IPs dinámicas, que serán asignadas por el servidor de DHCP, no pudiendo encontrarse ninguna de las IPs asignadas anteriormente dentro del rango del DHCP.
- La página _index.html_ del servidor web mostrará el mensaje "¡Hola 1º de SMR!".
- Comprueba que desde todos los equipos se puede acceder a la página _index.html_ del servidor web.

Documenta todo el proceso con las capturas de pantallas y las explicaciones que consideres necesarias.

## Entrega

Crea un documento PDF, a partir de un Word o Google Docs, con las siguientes consideraciones:

- El nombre del archivo a entregar en la plataforma Moodle Centros debe tener el siguiente formato: `Apellido1Apellido2_Nombre_RL_UD2_P11.pdf`.
- Portada con imagen, nombre del alumno, título y curso.
- Encabezado con nombre de la práctica y materia.
- Pie de página con nombre, número de página insertado, curso, año escolar, instituto.
- Índice o tabla de contenidos generada automáticamente.
- Tipo de letra: Times New Roman 12 o similar, interlineado sencillo, espaciado anterior 6p.
- Uso correcto de la ortografía.
- Texto justificado con uso de salto de páginas correcto.
- Enlaces web de referencia cuando se coge información de un sitio web.
- Las fotos que se puedan ver correctamente al ampliarlas y que no ocupen mucho espacio.
- Insertar Título de foto que haga referencia al apartado y ejercicio correspondiente.
- Para las capturas de pantalla de Cisco Packet Tracer:
    - Se captura todo el escritorio, con el fondo de su plataforma Moodle, donde se pueda ver imagen de perfil. Toda aquella práctica que no cumpla este requisito no será evaluada.
    - Al igual que en los Títulos de las fotos, en el Título de la captura de pantalla irá referenciado el apartado y ejercicio al que corresponde.