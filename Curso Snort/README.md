# Curso Snort

## ¿Qué es un IDS?

Un **IDS** (Sistema de Detección de Intrusos) es una herramienta de monitorización que detecta ataques de red o violación de políticas y las reporta a un sistema de gestión. Si, además, el sistema es capaz de detener estos ataques o violaciones cortando el tráfico, estamos hablando de un **Sistema de Prevención de Intrusos** o **IPS**.

Los IDS se pueden clasificar en dos grandes grupos, según su forma de detección:

1. Aquellos que detectan vigilando **estadísticas de red**.
2. Aquellos que los detectan mediante **firmas o patrones**.

Un ejemplo del primer tipo podría ser un sistema que vigile el número de paquetes ICMP que atraviesan una red cada minuto. Si, en un momento dado, se detectan muchos más paquetes ICMP de los que normalmente se detectan, estaríamos ante una anomalía que debe ser reportada, ya que podrían estar intentando realizar un escaneo de la red.

**Snort** se clasifica dentro del segundo tipo, es decir, por **firmas o patrones**, aunque también posee algunas características del primer tipo.

## Situación de un IDS en la red

Podemos colocar Snort IDS en varias posiciones dentro de la red. Si solo queremos defender un equipo, podemos ejecutarlo como un proceso del equipo, monitorizando su tarjeta de red. Este modo es conocido como **Host IDS**. Al ser tan especializado, el consumo de recursos se reduce, por lo que puede ser una alternativa viable.

Sin embargo, **Snort** es un IDS orientado a red, por lo que lo ideal es colocarlo vigilando el tráfico que atraviesa un segmento de la red. Normalmente, un **cortafuegos** o **conmutador de red** clonará todo el tráfico por una de sus bocas y lo enviará al IDS mediante un **puerto espejo** (mirror). Otra opción es reenviar el tráfico mediante un **dispositivo TAP**.

### Clonado del tráfico

Si hacemos el clonado del tráfico justo antes del cortafuegos, **Snort** será capaz de detectar incluso los ataques que el cortafuegos detenga, como un intento de conexión hacia la red de los usuarios. Esta posición suele generar mucho ruido, por lo que no es recomendable.

Si el clonado se realiza después del cortafuegos, **Snort** no podrá detectar los ataques que el cortafuegos corte. Además, si **Snort** gobierna el cortafuegos, tampoco será capaz de saber cuándo ha detenido un ataque para levantar la protección del cortafuegos. Sin embargo, el ahorro de CPU y la reducción de ruido en las alertas pueden compensar estas desventajas.

En caso de que la carga para **Snort** sea elevada, podemos hacer que varias instancias de **Snort** monitoricen partes más pequeñas de la red, distribuyendo así la carga de procesamiento.

## Herramientas del curso

### Tcpdump

**Tcpdump** es una herramienta de diagnóstico que muestra los paquetes que atraviesan una interfaz de red en la consola o los guarda en un fichero.

Para hacer que imprima todos los paquetes que atraviesan la interfaz `eth0`, usamos el siguiente comando:

$ tcpdump -i eth0

Si ahora hacemos pasar un paquete por esa interfaz de red, **Tcpdump** debería ser capaz de mostrarlo en pantalla.

Si queremos aplicar un filtro **BPF**, lo añadimos al final de la línea de órdenes:

$ tcpdump -i eth0 host 8.8.8.8  
$ tcpdump -i eth0 (udp) and (port 53)

Podemos almacenar lo que **Tcpdump** capture en un archivo **pcap** con el parámetro `-w`:

$ tcpdump -i eth0 port 53 -w dns.pcap -v

Al desplegar **Snort**, esta herramienta es muy útil, ya que podemos mostrar y almacenar exactamente lo que ve **Snort**, pues utiliza la misma librería para la captura de paquetes.

### Docker

El uso de **Docker** en este curso será algo anecdótico, no es necesario para ejecutar **Snort** en absoluto; solo se utilizará para generar los escenarios del curso.

Lo ideal sería probar **Snort** en un entorno real, pero necesitaríamos varias máquinas para hacerlo y conectarlas de manera específica para que **Snort** pudiera ver el tráfico correctamente.

Tradicionalmente, se han usado **máquinas virtuales** para este tipo de cursos. Sin embargo, los **contenedores** resultan mucho más ligeros y ágiles de desplegar, y dado que no vamos a hacer un uso avanzado de ellos, encajan perfectamente en el curso.

#### Desplegar un contenedor

Para desplegar un contenedor, es necesario que ejecute al menos un programa. Podemos empezar por desplegar un contenedor con **bash**, lanzándolo de la siguiente forma:

$ docker run --rm -it soullivaneuh/docker-bash

Los parámetros significan:

- `--rm`: El contenedor se borrará en cuanto deje de ser usado.
- `-it`: Permite interactuar con el contenedor.
- Nombre de la imagen.

Desde otra terminal, podemos ver que el contenedor está en ejecución en la lista de contenedores:

$ docker ps

La primera columna muestra el ID del contenedor. Podemos lanzar un comando dentro del contenedor con **docker exec**:

$ docker exec <image id> touch /hola

Si en la shell que se ha generado en la primera terminal hacemos un `ls /`, debemos poder ver el archivo **/hola**.

#### Ver la IP del contenedor

Usando **exec**, podemos ver la IP que se le ha asignado al contenedor:

$ docker exec <image id> ip a

Con esta información, si vemos las direcciones IP de todas nuestras interfaces (usando `ip a` en el anfitrión), debemos ser capaces de encontrar una interfaz virtual con la misma red que el contenedor.

El contenedor debe estar unido al anfitrión mediante un puente o conmutador Linux. Podemos ver todos los puentes del sistema y las interfaces asociadas a ellos con el siguiente comando:

$ brctl show

Por ejemplo, si el contenedor tiene la IP `172.17.0.2/16` y vemos que nuestra interfaz virtual `docker0` tiene la IP `172.17.0.1/16`, debemos tener un puente virtual que las conecte.


# Curso Snort - Reglas de Snort

## Introducción

La forma que **Snort** tiene de distinguir los paquetes o flujos de los que debería alertar es mediante las **reglas de Snort**. Estas reglas tienen un formato sencillo, aunque contienen muchos parámetros, la mayoría de ellos enfocados en hacerlas más eficientes, ya que el motor de búsqueda es lo que más recursos consume de Snort.

En esta sección, explicaremos cómo funcionan las reglas de Snort y cómo podemos crear nuestras propias reglas para que Snort detecte violaciones de política en nuestra red o, por ejemplo, algún malware reciente para el cual no tengamos una regla oficial disponible.

Es posible que durante las pruebas de esta sección no se detecten patrones más allá del byte 1400 del flujo de datos, debido a características que poseen algunas tarjetas de red avanzadas. Este tema será abordado en la sección correspondiente al DAQ.

## Formato de las reglas

Como ejemplo, vamos a ver una regla para detectar paquetes UDP en nuestra red:

alert udp any any -> any any (msg:"Paquete udp";sid: 1000000;)

De izquierda a derecha, la regla se compone de:

- **Identificador de la regla**: `alert` (tipo de alerta que se generará).
- **Protocolo sobre el que detectar la regla**: `UDP`.
- **Dirección IP origen**: `any` (cualquier dirección).
- **Puerto origen**: `any` (cualquier puerto).
- **Sentido del paquete**: `->` (hacia dónde se dirige el paquete).
- **Dirección IP destino**: `any` (cualquier dirección).
- **Puerto destino**: `any` (cualquier puerto).
- **Opciones de la regla**: Mensaje a mostrar e ID de la regla/firma (signature ID).

Si añadimos esta línea al final de nuestro fichero `snort.conf` y hacemos pasar un paquete UDP por la interfaz que estamos monitorizando (por ejemplo, una petición DNS), deberíamos ver una salida similar a la siguiente:

$ sudo snort -A console --daq-dir /usr/local/lib/daq/ -c /etc/snort/snort.conf -i wlp3s0  
Commencing packet processing (pid=4503)  
03/31-10:43:19.777996  [**] [1:1000000:0] Paquete udp [**] [Priority: 0] {UDP} 192.168.0.155:46302 -> 192.168.0.1:53  
03/31-10:43:19.778012  [**] [1:1000000:0] Paquete udp [**] [Priority: 0] {UDP} 192.168.0.155:46302 -> 192.168.0.1:53  
03/31-10:43:19.781162  [**] [1:1000000:0] Paquete udp [**] [Priority: 0] {UDP} 192.168.0.1:53 -> 192.168.0.155:46302  
03/31-10:43:19.781178  [**] [1:1000000:0] Paquete udp [**] [Priority: 0] {UDP} 192.168.0.1:53 -> 192.168.0.155:46302  

De izquierda a derecha, los campos son los siguientes:

1. **Marca de tiempo de la alerta**.
2. **ID de la alerta**. Por ahora, nos quedamos con que el número central es el **SID** (Signature ID).
3. **Mensaje de la alerta**.
4. **Prioridad asignada**.
5. **Protocolo**.
6. **Dirección y puerto origen y destino de la conversación**.

### Filtrando por puertos

Si, por ejemplo, sólo queremos obtener los paquetes UDP que correspondan a peticiones DNS, podemos especificar en qué puertos queremos:

alert udp any any -> any 53 (msg:"Petición DNS";sid: 1000001;)  
alert udp any 53 -> any any (msg:"Petición DNS";sid: 1000002;)

Con estas reglas, las alertas deberían reflejarse en la salida de Snort si enviamos un paquete DNS por la interfaz seleccionada.

### Filtrando por dirección IP

Si queremos encontrar todas las peticiones DNS que se realicen en nuestra red a dispositivos distintos de nuestro DNS (por ejemplo, que tenga la dirección `192.168.1.1`), podemos hacerlo de la siguiente manera:

alert udp any any -> !192.168.1.1 53 (msg:"Petición DNS";sid: 1000005;)  
alert udp !192.168.1.1 53 -> any any (msg:"Petición DNS";sid: 1000006;)

### Filtrando por red de confianza

Si queremos alertar de todas las peticiones DNS que **NO** vengan de una red de confianza (por ejemplo, nuestra DMZ `192.168.2.0/24`), podemos hacerlo con las siguientes reglas:

alert udp any any -> !192.168.2.0/24 53 (msg:"Petición DNS";sid: 1000007;)  
alert udp !192.168.2.0/24 53 -> any any (msg:"Petición DNS";sid: 1000008;)


## Se le llama salida de **Snort** a tres cosas distintas:

1. Salida por consola de Snort
2. Salida de eventos de Snort
3. Salida de paquetes que ocasionaron los eventos

En esta sección veremos los tres tipos de salida.

## Salida por consola de Snort

Si eliminamos el parámetro `-q` de **Snort**, podemos ver la siguiente información:

### Al iniciar Snort:
- Modo en el que ejecutamos
- Variables de las reglas
- Información sobre preprocesadores (más adelante)
- Memoria reservada para distintos procesos
- Estadísticas del motor de reglas
- Versión de Snort

### Y al finalizar:
- Tiempo, paquetes/seg
  - Una forma de depurar las reglas es, con un **pcap** dado, comprobar cómo tarda más en analizarse con muchas reglas.
- Memoria usada
- Paquetes IO, por protocolo.
- Veredictos
- Estadísticas de preprocesadores

## Salida de eventos textual

Ya hemos visto qué salida de eventos produce el parámetro `-A console`.

Al eliminarlo, podemos ver que **Snort** crea la siguiente salida en el directorio que le especifiquemos con `-l` (por defecto, `/var/log/snort`):

- Un fichero **alert**, con el texto del evento.
- Varios ficheros `snort.log.*`, que son ficheros **pcap** con los paquetes asociados a los distintos eventos.

Por defecto, es como si en la configuración tuviésemos el siguiente módulo de salida de eventos:

`output alert_fast: alert`

Podemos cambiar el nombre del fichero a **snort_fast**, indicarle que guarde información del paquete, y limitar esa salida a 10k con:

`output alert_fast: snort_fast packet 10k`

En otro formato (son dos plugins de salida bastante parecidos):

`output alert_full: snort_full 10k`

## Salida de eventos por syslog

La directiva para configurar la salida de eventos por **syslog** con la facility **LOG_AUTH** y la prioridad **LOG_ALERT** es:

`output alert_syslog: LOG_AUTH LOG_ALERT`

## Unified2

### Motivación

**Snort** tiene un diseño antiguo (que no obsoleto), y su funcionamiento es fundamentalmente monohilo. El envío de alertas se realiza en el mismo hilo que el procesado de paquetes, por lo que todo el tiempo que pasemos enviando una alerta (y esperando la respuesta del receptor) es tiempo en el que estamos encolando paquetes y no estamos procesando, lo que puede provocar que tengamos que dejar de procesar algunos paquetes.

La situación se agrava si el receptor es otro programa que debe esperar que la CPU lo ejecute, es decir, **comunicación entre procesos**, como ocurre con la pantalla, **syslog**, una base de datos, etc.

Para solucionarlo, **Snort** encolará las alertas y los paquetes que la han provocado en un fichero binario, con un formato liviano, y otro programa (quizás, incluso, con menos prioridad de CPU) se encargará de transformar esas alertas en registros en una base de datos.

Este formato es **unified2**. Para configurarlo, añadimos en la configuración, en la sección correspondiente (output plugins):

`output unified2: filename merged.log, limit 128, mpls_event_types, vlan_event_types`

Podemos añadir también la opción `nostamp`, que hará que los ficheros de logs no se roten.

Para visualizar estos eventos en consola, podemos hacerlo con `u2spewfoo <fichero>`, y podemos transformarlos en ficheros **pcap** con:

`u2boat <fichero u2> <fichero pcap>`

### Formato unified2

**Unified2** es un fichero binario, los datos no están en texto plano, con el fin de conseguir eficiencia. En un fichero **unified2**, tenemos los siguientes tipos de registros:

- Evento
- Paquete
- Otros

Es decir, si empezamos en el offset 0 del fichero, lo primero que nos aparece es qué tipo es el siguiente registro. Normalmente, el primero de todos será un **evento**, en el que viene información del mismo, como el **sid**, el tiempo en el que fue lanzado, etc.

Tras ello, podemos ver **paquetes** asociados a ese evento (si los hay)

# Barnyard2

## Instalación

Para descargar **Barnyard2**:

[https://github.com/firnsy/barnyard2/releases/tag/v2-1.13](https://github.com/firnsy/barnyard2/releases/tag/v2-1.13)

La instalación de **Barnyard2** es similar a la de **Snort**, pero si queremos enviar los eventos a una base de datos, necesitamos indicarle la ruta de los plugins de **mysql**/**postgresql**.

### Pasos de instalación:

1. Ejecutamos los siguientes comandos:

(./autoconf.sh)  
(./configure --with-mysql --with-mysql-libraries=/usr/lib/ --with-postgresql)  
(make)  
(make install)

## Syslog

La salida por **syslog** se configura con el mismo parámetro que en **Snort**:

(output alert_syslog: LOG_AUTH LOG_INFO)

Podemos incluir el paquete en distintas codificaciones:

(output log_syslog_full: sensor_name snort-b2, local, operation_mode complete)  
(output log_syslog_full: sensor_name snort-b2, local, operation_mode complete, \    payload_encoding ascii)  
(output log_syslog_full: sensor_name snort-b2, local, operation_mode complete, \    payload_encoding base64)

## Salida a base de datos

**Nota:** **NO** usar **MariaDB** como reemplazo de **MySQL**, porque no funciona.

Con los servidores 172.17.0.2 para **PostgreSQL** y 172.17.0.3 para **MySQL**:

Tenemos las distintas bases de datos soportadas en las fuentes descargadas anteriormente, bajo `barnyard2-2-1.13/schemas`. Necesitamos ejecutar esos ficheros antes de emitir ninguna alerta, para crear los esquemas en la BBDD:

(cat barnyard2-2-1.13/schemas/create_postgresql | \  
psql -h <host> -U snort snort)  
(cat barnyard2-2-1.13/schemas/create_mysql | \  
mysql -h <host> -D snort -u <db_user> -p)

Para añadir el plugin de salida, en **barnyard2.conf**:

(output database: log, postgresql, host=<host> user=postgre password=postgre dbname=snort)  
(output database: log, mysql, host=<host> user=snort password=snort dbname=snort)

## BASE

**BASE** (Basic Analysis and Security Engine) es una aplicación para la gestión y visualización de alertas de **Snort**. Puedes descargarla desde:

[https://sourceforge.net/projects/secureideas/](https://sourceforge.net/projects/secureideas/)

Se necesita un servidor con **PHP** para que se ejecute, e instalar en el servidor los archivos de la aplicación `*.php`.

## Fichero Waldo

Si ejecutamos **Barnyard2** varias veces sobre el mismo directorio, emitirá las mismas alertas. Con el fichero **waldo**, dejamos un marcador para que no se vuelvan a emitir las alertas del mismo fichero.

Se puede indicar a **Barnyard2** que use el fichero **waldo** con la opción `-w <waldo>`.

---

La configuración de Snort es extensa y compleja, aunque está muy bien organizada en bloques, y muy orientada para su ajuste para la red a proteger.

Lo primero que debemos saber, que ya se dejó entrever en lecciones anteriores, es:

- Es texto plano, por lo que es fácil editarla.
- Permite la inclusión de otros ficheros de configuración (directiva include). Esto ficheros serán “empotrados”, es decir, será como si el texto que contienen estuviese escrito en el lugar de la directiva. Es parecido a la directiva #include de C.
- Permite variables, aunque hay que tener cuidado con su tipo: ipvar (para almacenar rangos ip), portvar para almacenar rangos de puertos y var para almacenar texto

Por ejemplo, si partimos del siguiente archivo snort.conf:

`var RULES_PATH rules/`
`portvar HTTP_PORTS [80,8080]`
`ipvar HTTP_SERVER [192.168.1.24]`
`include $RULES_PATH/my-http.rules`
Y del siguiente rules/my-http.rules:

`alert tcp any any -> $HTTP_SERVER $HTTP_PORTS (flags:S; msg:"SYN packet";)`

El parseo del fichero por parte de snort se hará de la siguiente forma:

1. Lectura de las variables RULES_PATH, HTTP_PORTS y HTTP_SERVER, las tres primeras líneas.
2. Sustitución de la variable RULES_PATH, quedando la tercera línea en include rules/my-http.rules
3. Inclusión del fichero rules/my-http.rules
4. Sustitución de las variables de la regla.
5. Parseo de la regla, y adición de la misma al árbol de reglas.
   La principal ventaja es aplicar Dont Repeat Yourself (DRY), es decir, el archivo de reglas (y, en general, la configuración) se pueden trasladar de una red a otra y de un entorno a otro, con la única salvedad de tener que modificar esos pocos puntos en los que definimos las variables.

En caso de tener una configuración compleja y/o querer facilitar la configuración de snort mediante sistemas de distribución de configuración como chef, es recomendable extraer cada una de las secciones indicadas en archivos separados, mediante la directiva include, de forma que la localización de parámetros que se modifican habitualmente sea ligera.

## Variables de red

El principal ajuste de snort son las variables de la red a proteger, y es uno de los ajustes más importantes. Por ejemplo, si un paquete no se dirige a tu red (está saliendo de ella), snort lo sabe y no intentará aplicar las reglas que detectan ataques HACIA tu red, y viceversa. Al mismo tiempo, no intentará examinar paquetes que no pertenezcan a puertos de un protocolo dado contra las reglas de un protocolo dado, haciendo que ese mismo paquete tenga que analizarse contra muchas menos reglas.

Profundizando un poco más en el ejemplo, si tenemos un servidor HTTP expuesto en el que el 90% del tráfico es de bajada, y nos preocupan los ataques dirigidos al servidor (como shellshock), snort sólo analizará, del 10% de los paquetes que se dirigen al servidor, aquellos dirigidos al puerto en el que exponemos HTTP. ¡Una gran mejora!

Además de los `*_NETS, *_SERVERS` and `*_PORTS`, tenemos en esta sección las carpetas de reglas, reglas_so y reglas de preprocesador.

## Configuración del decodificador

### El decodificador

El decodificador es la parte previa al motor de detección, y mediante esta sección podemos configurar su comportamiento. No es muy recomendable modificar parámetros de esta sección a no ser que realmente sea necesario o bien hayamos adquirido mucha más destreza en snort.

Como primeros pasos, podemos incrementar un poco el rendimiento de snort si desactivamos la comprobación de la suma de verificación con `checksum_mode: none` o bien algún valor intermedio (sólo alguna de las capas). Sin embargo, esto no supondrá una gran diferencia.

## Reglas del decodificador

Una parte interesante del decodificador es que puede lanzar eventos por sí solo, relativos a distintas partes del mismo. Por ejemplo, que la longitud que dice en la cabecera TCP no es realmente la del paquete.

El formato de estas reglas es algo distinto a las que hemos usado hasta ahora:
En lugar de asignarle nosotros un id de firma (sid) a un patrón o `content`, Snort ya tiene prefijados una serie de ids para ellas, y las reglas se identifican por la tupla formada por el id de la firma (sid) y el id del generador (gid), que en el caso del decodificador es 116. En realidad, siempre se han identificado así, pero el motor de detección tiene el gid por defecto, 0.

Por ejemplo, si queremos detectar el ataque LAND (un ataque a la pila IP del SO con un paquete con la IP de loopback, `127.0.0.1`, como origen y/o destino), activamos la siguiente regla:

`alert (sid:150; gid:116; msg:"LAND attack!!";)`
Podemos ver el resto de alertas del decodificador con:

`grep '^116' /etc/snort/gen-msg.map`
Nota: Algunas reglas son buenas para diagnóstico rápido del DAQ, especialmente con el tamaño de captura.

En resumen:

- Gid describe el origen de la regla (0 para el motor de detección, 116 para el decoder)
- Las alertas con gid distinto de 0 no son con expresión o contenido, sino que se encuentran prefijadas en el código de snort.

## Motor de detección

Podemos ver estadísticas de reglas o de preprocesadores si descomentamos:

`config profile_rules: print all, sort avg_tick`
`config profile_preprocs: print all, sort avg_ticks`
Se imprimirán a la salida estandar al finalizar la ejecución de snort

# Frag3 - Prueba del preprocesador

Para ver los límites del ping (o de cualquier paquete fragmentado):

`$ tcpdump -i <interfaz> -X`

En Linux, el primer fragmento termina en `|be bf|` y el siguiente empieza en `|c0 c1|`, por lo que la regla para detectarlo con el motor de detección es:

`(alert icmp any any -> any any (msg:"frag attack!"; content: "|be bf c0 c1|"; sid:1000012;))`

Se puede comprobar la diferencia al comentar (=desactivar) frag3 y descomentarlo, sólo salta en el último caso. Al desactivar **frag3**, es aconsejable desactivar todos los preprocesadores, ya que la mayoría dependen de él.

## Configuración

Se divide en **global** y **engine**, para particularizar cada elemento de la red. No es lo mismo defender la pila IP de un Windows que de un Linux o un BSD.

Si se trabaja en una red en la que puede haber fragmentos, y hay memoria de sobra, se recomienda poner todo al máximo. Si siempre hay fragmentos, se puede pre-reservar la memoria, y si no hay, ponerla al mínimo.

## Alertas interesantes

### **Frag3 Teardown Attack**

Ocurre cuando la longitud total de los fragmentos dice ser más pequeña que la real. En algunos sistemas antiguos, este ataque podía tumbar un servidor, y la presencia de estos suelen indicar un ataque intencionado.

Con el pcap:

`https://wiki.wireshark.org/SampleCaptures?action=AttachFile&do=get&target=teardrop.cap`

Activamos la regla:

`(alert (msg: "FRAG3_SHORT_FRAG"; sid: 3; gid: 123; rev: 1;))`

# Session - Preprocesador

Se puede probar descargando la web de `www.example.com` con las optimizaciones de la tarjeta desactivadas y con una regla que detecte el límite, igual que en el preprocesador frag3, pero en TCP.

## Configuración

Al igual que en **frag3**, pero es mucho más común que haya segmentos, por lo que se recomienda dedicar el máximo de RAM posible.

Si no estamos interesados en **UDP/ICMP**, se recomienda desactivarlo, y dividir todo lo posible en **streams**, a fin de no hacer trabajo extra.

La configuración se divide en **session** (anteriormente llamado **stream5_global**) y **stream** (anteriormente, la especialización de **stream5_{tcp,udp,icmp}**).

## Alertas interesantes

### **Session Hijacking**

Es el cambio de MAC en mitad de la conversación, lo que indica un "man in the middle".

Se habilita con:

`preprocessor stream5_tcp: ..., check_session_hijacking, ...`

Y lanza las siguientes reglas:

`alert ( msg: "STREAM5_SESSION_HIJACKED_CLIENT"; sid: 9; gid: 129; rev: 1;`
`metadata: rule-type preproc ; classtype:attempted-user; )`

`alert ( msg: "STREAM5_SESSION_HIJACKED_SERVER"; sid: 10; gid: 129; rev: 1;`
`metadata: rule-type preproc ; classtype:attempted-user; )`

Para prepararlo, se capturó una conversación HTTP normal y se modificaron las direcciones MAC usando un editor hexadecimal.

### **NULL Scan**

Paquetes TCP sin flag. Podemos enviar uno con **scapy** con la siguiente orden:

`$ scapy`

`send(IP(dst='192.168.0.1')/TCP(dport=81, flags='')/"hello!")`

Y detectarlo con:

`alert ( msg: "STREAM5_DATA_WITHOUT_FLAGS"; sid: 11; gid: 129; rev: 1;`
`metadata: rule-type preproc ; classtype:protocol-command-decode; )`

# Uso Básico de Reglas en Snort

## Activar las reglas `icmp-info.rules` para detectar el PING UNIX

1. Activa la regla `icmp-info.rules` para detectar el PING UNIX.
2. Realiza un ping a `8.8.8.8` y observa que genera una alerta.
3. Añade `8.8.8.8` a la lista negra. Verás que no se genera alerta, ya que el preprocesador de reputación lo está bloqueando.
4. Si deseas saber cuándo el preprocesador de reputación bloquea, debes activar la siguiente regla:

`alert (msg: "REPUTATION_EVENT_BLACKLIST"; sid: 1; gid: 136; rev: 1;` `metadata: rule-type preproc; classtype:bad-unknown;)`

## Uso de Whitelist

- Por defecto, la lista blanca elimina direcciones IP de la lista negra.

### Ejemplo:

1. Añade la red `8.8.0.0/16` a la lista negra.
2. Añade `8.8.4.4` a la lista blanca.
3. Al realizar un ping a `8.8.8.8` y `8.8.4.4`, se comportará de la siguiente manera:
   - Se generará una alerta para `8.8.8.8`, indicando que fue bloqueado por el preprocesador de reputación.
   - No habrá alerta para `8.8.4.4` porque está en la lista blanca.

4. Si desactivamos las firmas de detección para el PING, sólo el ping a `8.8.8.8` generará una alerta.

## White Trust

- **Whitelist** no sólo saca de la lista negra, sino que también puede evitar que los paquetes lleguen al motor de detección. Es útil para nodos seguros, como servidores de respaldo.

- Para depurar el comportamiento de la lista blanca y ver cuándo un paquete es omitido de la lista negra, usa la siguiente regla de alerta:

`alert (msg: "REPUTATION_EVENT_WHITELIST"; sid: 2; gid: 136; rev: 1; metadata: rule-type preproc; classtype:bad-unknown;)`

- Si añades `white trust` a los parámetros del preprocesador, podrás ver este comportamiento en las pruebas anteriores.

- El orden de las direcciones IP es importante. Si una dirección IP está tanto en la lista blanca como en la lista negra, el preprocesador dará prioridad a la última procesada.

- En las pruebas de ping anteriores, verás que:
  - Un ping genera una alerta de bloqueo por la lista negra.
  - El otro genera una alerta de que ha sido omitido por la lista blanca.

## `scan_local`

- Por defecto, los paquetes con IPs privadas no son analizados.
- `scan_local` permite que los paquetes con IPs privadas sean analizados, lo que facilita definir nodos seguros o malignos dentro de redes privadas.

## `priority`

- El argumento `priority` **no** se utiliza para desempatar entre las listas de whitelist y blacklist.
  - **Whitelist trusted**: Si se encuentran en ambas listas, el desempate lo da el orden en el que se procesen las IPs.
  - **White unblack**: La lista blanca tiene siempre prioridad sobre la negra.

- Este argumento es útil para paquetes donde la dirección origen es de la lista negra y la dirección destino es de la lista blanca, o viceversa. 
  - **Por defecto**, la prioridad la tiene la lista blanca (priority whitelist), pero podemos cambiar este comportamiento con `priority blacklist.`

## Peticiones comprimidas

Para que el servidor nos devuelva la página comprimida:

`$ gzip: curl -H 'Accept-Encoding: gzip, deflate' www.example.org`

## Acceso a rutas transversales

Creamos un árbol de directorios con la página mal.html, que será en a que no queremos que los clientes accedan, y un directorio evasion al mismo nivel para intentar la evasión.

Usamos el docker de tutum/apache-php: Situados en la raíz del árbol que acabamos de crear:

`$ DOCKER_PHP_ID=$(docker run -v ${PWD}:/app -d tutum/apache-php)
$ docker exec ${DOCKER_PHP_ID} ip a`

Realizamos una petición malintencionada, y vemos que nos devuelve el HTML que queremos ocultar:

`$ curl --path-as-is '172.17.0.3/evasion/../mal.html'`
Cambiamos en el decoder de snort checksum mode: none, ya que no se hace suma de verificación para el tráfico entre contenedores, y activamos la normalización de directorios de http_inspect_server: directory yes.

Activamos la alerta correspondiente:

`alert tcp $EXTERNAL_NET any -> $HTTP_SERVERS $HTTP_PORTS \
    (msg: "ruta no autorizada!"; content:"/mal.html"; http_uri; offset:0; \
    depth: 9; sid: 1000090;)`

Y vemos como Snort alerta si se intenta acceder a ese archivo. Si queremos que nos avise cada vez que se intenta acceder de este modo a algún archivo:

`alert (gid 119; sid 11 directory traversal)`

## Captura de malware javascript ofuscado

Codificar cadena:

Podemos codificar una cadena en notación porcentar con el siguiente código python:

`>>> cadena = "soy un malware"`
`>>> '%' + "%".join("{:02x}".format(ord(c)) for c in cadena)`
Si intentamos encontrar el patrón “un malware” en el siguiente archivo html y regla:

 `<p><script>document.write(unescape("un malwar%65"))</script>`

`alert tcp any any -> any any \
    (msg: "malware!"; content:"un malware"; sid: 1000091;)`

Vemos que Snort normaliza el javascript antes de que pase al motor de reglas.
Se puede probar también con varios espacios, y codificando diferentes cosas.

Inspección en descargas comprimidas
Podemos hacer una regla que coincida con parte del contenido de la página www.example.org:

`alert tcp any any -> any any ( msg:"Example.org"; file_data; \
    content:"to be used for illustrative examples"; sid: 1000095;)`

E intentar que la regla se active cuando se descarga comprimida con el comando de la primera sección

# Unified2 y Seguridad en Snort

## Activar log_uri y log_hostname en `http_inspect_server`

Si activamos `log_uri` y `log_hostname` en la sección `http_inspect_server`, y descargamos el script de la sección de Javascript ofuscado, podemos ver en el unified2 de salida de Snort:

- **Javascript normalizado** (extradata type 13)
- **Hostname** (extradata type 10)
- **URI** (extradata type 9)


![alt text](<Captura de pantalla 2024-12-17 a las 21.00.02.png>)