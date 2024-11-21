# Windows Básico

## Herramientas

Historian: Nos permite exportar las cookies para su analisis (fecha, hora)
Dumpit: Captura la memoria volatil. Se crea un fichero donde se ejecuta
Disk investigator: Para leer en RAW la informacion extraida con Dumpit

## Conceptos

- Memoria de paginación (**shadowcopy**) Es la memoria intermedia entre el kernel de windows y la memoria RAM y nos permitirá ver datos en esa zona. Se denomina ***pagefile.sys*** (permite buscar usuarios, contraserñas...) Se puede realizar una copia en caliente con Shadow Copy
- BSS (servicio de sombra en windows)
- RAT
- Backdoor
- Captura de MBR (mbrutil)

Los siguientes comandos son variantes usadas en el tallerdel comando dir de Windows, que se utiliza para listar el contenido de directorios. A continuación, desglosaré cada uno, explicando su funcionalidad, parámetros, y uso habitual:

### Comando: dir cookie.*.* /s /p

**Desglose**:
`dir`: Es el comando principal utilizado para listar los archivos y directorios en una ubicación específica.
`cookie.*.*`: Especifica un patrón de búsqueda. En este caso:
`cookie`: Busca archivos cuyo nombre comience con "cookie".
`.*.*`: Indica que puede haber cualquier extensión, incluyendo nombres con múltiples puntos (por ejemplo, cookie.log.txt o cookie.abc.def).
`/s`: Busca en el directorio actual y todos sus subdirectorios.
`/p`: Muestra los resultados página por página, deteniéndose después de cada pantalla llena de resultados para facilitar la lectura.

**Función**:
Busca todos los archivos en el directorio actual y sus subdirectorios que cumplan con el patrón cookie.*.* y los muestra de forma paginada.

**Usabilidad habitual**:
Este comando se usa para localizar archivos específicos relacionados con cookies en un sistema de archivos, útil en tareas de auditoría o limpieza.
Puede ser útil para desarrolladores, administradores de sistemas o personas que investigan rastros de actividad web en el sistema.

### Comando: dir index.dat /s /p /a

**Desglose**:
`dir`: Comando principal para listar archivos y directorios.
`index.dat`: Busca archivos específicos llamados index.dat. Este archivo está relacionado con datos almacenados por navegadores web antiguos como Internet Explorer.
`/s`: Busca en el directorio actual y todos sus subdirectorios.
`/p`: Muestra los resultados página por página, útil si hay muchos resultados.
`/a`: Incluye todos los archivos, independientemente de sus atributos (archivos ocultos, de sistema, de solo lectura, etc.).
**Función**:
Busca todos los archivos llamados index.dat en el directorio actual y sus subdirectorios, incluyendo archivos ocultos y del sistema, y los muestra de forma paginada.

**Usabilidad habitual**:
Este comando suele emplearse en tareas de análisis forense, limpieza de datos antiguos, o resolución de problemas relacionados con el historial de navegación de Internet Explorer.
También es útil para identificar archivos que el sistema podría estar ocultando al usuario normal.
**MINUTO 14:00**
