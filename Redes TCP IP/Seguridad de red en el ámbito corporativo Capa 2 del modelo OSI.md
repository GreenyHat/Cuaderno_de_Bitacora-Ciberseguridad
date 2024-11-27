![[siete-capas-de-modelo-osi-1623136242-cywGEI9RkM.webp]]

# Seguridad corporativa en la capa 2

## Dudas

Trama ethernet?
Direccion de encabezado?
Servidor RARP (Reverse ARP) especializado? --> deprecated
BOOTP?
DHCP?
Proxy ARP (Más usado que RARP)
Puerta de enlace?
Algoritmo STP
Y si el enlace que se rompe en el algoritmo STP es el root?? Cómo actua el BPDU ahí?
**Practica en PAcket Tracer Cisco con comandos para ARP, ICMP (ping) _investiga el modo simulación_**
Trama BDPU (Unidad de datos STP para determinar root?)
Árbol de expansión
PVID // PVID inconsistente en el etiquetado de una VLAN nativa
Por que la VLAN nativa NUNCA debe coincidir con la VLAN de usuarios
LA IMPORTANCIA DEL ETIQUETADO EN LAS VLANs
**Ejercicios CISCO resultos para practicar todo esto???**
Interfaz de redes?
Anuncios DTP

## Protocolo ARP

Es un protocolo de la capa de enlace de datos y se encarga de encontrar la direccion MAC asociada a una direccion IP.

- Mantiene una tabla de asignaciones de direcciones IPv4 a MAC
  Dicha tabla se encarga de resolver las solicitudes que le lleguen diciendo que necesita haber relación entre la direccion IP y una MAC
- Esencial para transmision de datos en redes Ethernet
- Dos tipos de paquetes:
  - ARP Request (No conociendo la direccion MAC de destino se encampsula en una trama Ethernet con la info de encabezado: direccion MAC de destino, de origen y tipo [Broadcas])
  ![Request](<Captura de pantalla (442).png>)
  - ARP Reply (Encabezado directo al request [no-Broadcast])
  ![Reply](<Captura de pantalla (443).png>)
- Las relaciones se pueden almacenar estática o dinámicamente
  - ARP estático (las introduce el administrador)
    - MAs seguridad menos escalabilidad
  - ARP dinámico (se aprenden mediante el protocolo)
    - No son permanentes en la tabla
    - Menos seguro pero más escalable

## Fallos de seguridad ARP

### ARP Spoofing

![ARP Spoofin](<Captura de pantalla (444).png>)
![ARP Spoofing 2](<Captura de pantalla (445).png>)

#### Ataques asociados

- DoS
  - Borrado parcial o total de los paquetes capturados ya que se asocia la MAC atacante, o una que no exista, con la ip de la puerta de enlace
- Man in the Middle
  - Los paquete enviados entre dos extremos son interceptados y se puede hacer un **_session hijacking_ (secuestro de cookie)**

### Solución propuesta

- Utilizar ARP estático (los comandos varian dependiendo del dispositivo)
- Software de deteccion y prevencion de ARP Spoofing
  - Arpwatch (Escucha continuamente y si detecta cambios avisa al administrador)
  - ArpDefender
  - XArp
- Configurar DAI (Dynamic ARP Inspection [OPCIÓN MAS USADA])
  - Con o sin DHCP Snooping

## Protocolo STP

- Es un protoclo de la capa enlace de datos y gestiona la presencia de bucles (_Broadcast storm_) en la topologia de red debido a enlaces redundantes
- Los swithces se activan o desactivan automáticamente segúun las necesidades topológicas
- Para elminar los bucles se determina un root en funcion de prioridad y 3 tipos de puerto:
  - root
  - designado
  - alternativo
- Se utilizan tramas BDPU

### Temporizadores entre escucha y aprendizaje

- Hello Timer
  - valor predeterminado (2 segundos)
- Forward Delay Timer
  - valor predeterminado (15 segundos)
- Max Age Timer
  - valor predeterminado (20 segundos)

### Estados del puerto

![Estados del puerto](<Captura de pantalla (448).png>)

### Simulación en Packet Tracer

**Configuración inicial**
![1](<Captura de pantalla (450).png>)

**Dentro del root**
![2](<Captura de pantalla (451).png>)

**Modificando el root**
![3](<Captura de pantalla (453).png>)

**Nuevo root corriendo**
![4](<Captura de pantalla (454).png>)

**Modo simulación ICMP**
![5](<Captura de pantalla (455).png>)

**Bloqueando un puerto de forma manual para ver como reacciona el protocolo**
![6](<Captura de pantalla (456).png>)

### Configuración segura de STP

- **Usar Portfast BDPUGuard** (Es una configuracion que se establece con lols enlaces que conectan a los usuarios finales y asegura que los puertos pasan a estado de reenvio sin quedarse escuchando ni aprendiendo) [Se asume que no va a haber un switch conectado por lo que no va a formar parte del arbol de expansion]
- **Usar Root Guard**

## Protocolo CDP

- Protocolo patentado por Cisco, de capa 2 (Solo se usa en dispositivos cisco que comparten enlace de datos)
- Envía mensajes multicast periódicos a los disps conectados
- Info de los paquetes: nombre de dispositivo, imagen del OS, tipo y modelo, direccion IP, Vlan nativa...

### Fallo de seguridad

**CDP Flooding**
![CDP Flooding](<Captura de pantalla (457).png>)

Estos paquetes tambien comparten información sensible por lo que haciendo sniffing podríamos saber version, topologia, version...

### Configuración segura

- Desactivación de CDP mediante interfaz o globalmente

## Consideraciones teóricas sobre VLAN

Una VLAN (Red de área local virtual) es un método para crear redes lógicas independientes dentro de una misma red física (Una división lógica de la red, sin necesidad de dispositivos físicos). Varias VLAN pueden coexistir en un único conmutador físico o en una única red física. Son útiles para reducir el dominio de difusión y ayudan en la administración de la red, separando segmentos lógicos de una red de área local que no deberían intercambiar datos usando la red local.

### Ventajas

- Dominios de difusión más pequeños.
- Seguridad mejorada.
- Mejora la eficiencia del departamento de IT.
- Reducción de costos.
- Mejor rendimiento.
- Administración más simple de proyectos y aplicaciones.

### Tipos de enlace

- Acceso
  - tráfico de una única VLAN
  - conexión con dispositivos finales
- Troncal
  - trafico de multiples VLAN
  - comexión entre switches con la capa superior
![acceso y troncal](<Captura de pantalla (459).png>)

### Fallo de seguridad de VLAN

- **VLAN Hooping (Spoofing)**
  Acceso a las VLANs desde capa 2 (sin router)
  ![vlHoop](<Captura de pantalla (461).png>)
- **VLAN Double-Tagging (más común)**
  ![alt text](<Captura de pantalla (463).png>)
  ![alt text](<Captura de pantalla (464).png>)
  ![alt text](<Captura de pantalla (465).png>)

### Configuración segura VLAN

#### Mitigación del VLAN Hopping

- No usar la VLAN por defecto como VLAN nativa (20?)
- Deshabilitar la formacion automatica de enlaces troncales
- Apagar los puertos no utilizados (La mejor y más tediosa)

#### Mitigación de VLAN Double-Tagging

- No usar la VLAN por defecto como VLAN nativa (20?)
- Apagar los puertos no utilizados (La mejor y más tediosa)

## Protocolo DTP

- Maneja la negociacion de enlaces troncales (Capa 2 y exclusivo de Cisco)
- Una interfaz puede establecerse como:
  - Troncal
  - De acceso
  - Para negociar troncal con la interfaz vecina

### Modos de interfaz

![alt text](<Captura de pantalla (466).png>)

### Fallo de seguridad DTP

![alt text](<Captura de pantalla (467).png>)
![alt text](<Captura de pantalla (468).png>)

### Mitigación de los ataques

- Configuración manual de los puertos utilizados
- Deshabilitar anuncios DTP
- Apagar puertos no usados
- Configurar puertos no utilizados como acceso
  - Asignar una VLAN no utilizada por los usuarios o una inexistente

## Protocolo VTP

- Se usa para administrar y configurar VLANs en los dispos Cisco
- Se distinguen 3 tipos de operacion VTP
  - Servidor (define las vlans)
  - Cliente (reciben la info)
  - Transparente (no quieren formar parte de la topología)
- Solo aprende VLANs del rango normal (1-1005) y se almacenan en el fichero vlan.dat

### Modos de operacion VTP

- Servidor (por defecto)
  - Crear, borrar y modificar VLANs del dominio
  - Todas las VLANs configuradas se anuncian
- Cliente
  - No puede crear, borrar ni modificar 
  - Sincroniza su info con la que recibe de los anuncios del sv
- Transparente
  - No procesa anuncios VTP, los reenvía

### Anuncios VTP

- Anuncios de resumen
  - Sincroniza la info de las vlans dentro del dominio
- Anuncios de subconjunto
  - Informa de cambios en alguna vlan
- Anuncios de solicitud
  - Se envia cuando un cliente necesita actualizar la config

### Fallo de seguridad VTP

Borrado de VLANs legítimas si un atacante se conecta a un enlace troncal en nuestro sv y envia un anuncio con un numero de revision superior al que hay en la red (Para ello se esnifa el tráfico y se ve el numero de revision) y se envia el anuncio VTP con un numero mayor

![alt text](<Captura de pantalla (470).png>)

### Configuracion segura de VTP

- No usar VTP
  - Deshabilitarlos de los switches que no se usan connfigurándolos en modo transparente
- Autenticación
  - Los switches de dominio deben usar la misma clave
- Apagar los puertos que no se van a utilizar
- Usar versión 3 del protocolo
  - Primary Server (Nos asegura que solo hay un sv en la red)

# Seguridad corporativa en la capa 3 y 7

## Introducción a la capa 3



## Rutas estáticas

## RIP

## OSPF

## HSRP

## Capa 7

## DHCP

## SNMP




---

# Los Protocolos de Red y sus Vulnerabilidades: Una Narrativa Interactiva

## Protocolo IP

**IP (un mensajero serio y directo, pero confiado en exceso)** se prepara para entregar paquetes de datos. Se encuentra con **Spoofing** y **Fragmentation Attacks**, quienes intentan engañarlo.

**IP**:  
"¡Todo listo para entregar datos! No importa quién seas ni de dónde vengas, confío en las cabeceras. Mientras me des una dirección válida, ¡te llevaré a donde necesitas ir!"

**Spoofing (con una sonrisa tramposa)**:  
"Eso es genial, IP. Mira, aquí está mi dirección de remitente. Soy un amigo de confianza, prometo que no haré nada malo."

**IP**:  
"Perfecto, confío en ti. No reviso la autenticidad, solo entrego paquetes. ¡Sube a bordo!"

**Fragmentation Attack (cortando un paquete en trozos pequeños)**:  
"Espera, IP, tengo algunos paquetes grandes. ¿Podrías fragmentarlos y entregarlos? No te preocupes, el receptor los ensamblará más tarde."

**IP**:  
"¡Por supuesto! A veces no hay otra opción. Cortaré estos paquetes en pedazos pequeños para que puedan viajar más fácilmente."

**Firewall (observando preocupado)**:  
"¡Espera, IP! Cuando dejas pasar paquetes fragmentados, no puedo inspeccionarlos bien. Esto podría ser un ataque para eludir mis reglas."

**IP**:  
"Bueno, es mi trabajo entregar paquetes. Si alguien quiere fragmentarse para esconder algo, no es mi problema..."

---

## Protocolo ICMP

**ICMP (el mensajero del diagnóstico y la comunicación)** está ocupado entregando mensajes de error y solicitudes de eco, cuando de repente **Ping Flood** e **ICMP Redirect** se acercan con intenciones maliciosas.

**ICMP**:  
"¿Problemas en la red? Estoy aquí para ayudar. ¿Necesitas un 'ping' para verificar si un dispositivo está activo? ¿Un mensaje de 'destino inalcanzable'? ¡Yo lo manejo!"

**Ping Flood (golpeando repetidamente)**:  
"¡Sí! Tengo muchísimas solicitudes de eco para ti. ¡Tantas que saturarán al pobre servidor que responde!"

**ICMP**:  
"¡Claro! Responderé a todas las solicitudes, es mi trabajo... aunque, eh... esto parece una cantidad inusual de tráfico..."

**ICMP Redirect (en voz baja, señalando una dirección falsa)**:  
"ICMP, oye, ¿puedes decirle a ese router que use esta ruta más 'rápida'? No te preocupes por verificarlo, soy de confianza."

**ICMP**:  
"¡Por supuesto! Siempre intento optimizar la red. Dirigiré el tráfico a donde me indiques."

**Administrador de Red (interviniendo)**:  
"¡Espera un segundo! ICMP Redirect, ¿por qué rediriges el tráfico hacia direcciones sospechosas? Estás facilitando un ataque Man-in-the-Middle."

---

## Protocolo ARP

**ARP (el vecino confiado que asocia direcciones IP con MAC)** camina por el vecindario, ayudando a todos, pero es fácilmente engañado por **ARP Spoofing**.

**ARP**:  
"¡Hola a todos! ¿Alguien sabe la dirección MAC del 192.168.1.1? Necesito hablar con él."

**ARP Spoofing (alzando la mano rápidamente)**:  
"¡Sí, soy yo! Mi MAC es 00:11:22:33:44:55."

**ARP**:  
"¡Gracias, amigo! Actualizaré mi tabla para recordarlo."

**ARP Spoofing (sonríe maliciosamente)**:  
"Perfecto. Ahora redirigiré todo el tráfico destinado a 192.168.1.1 hacia mí."

**Switch (confundido)**:  
"Espera, ARP, he visto dos dispositivos diferentes reclamar esta dirección IP. ¿Cuál es la correcta?"

**ARP**:  
"No lo sé... No tengo autenticación. Confío en el primero que responde."

**Administrador de Red**:  
"Esto no está bien. Deberíamos implementar medidas de seguridad como ARP Inspection para evitar este tipo de engaños."

---

## Protocolo RIP

**RIP (el protocolo simple de enrutamiento)** intenta compartir rutas con sus vecinos, pero es interrumpido por **Routing Table Poisoning**.

**RIP**:  
"¡Oigan todos, estas son las rutas que conozco! Las comparto cada 30 segundos para mantenernos actualizados."

**Routing Table Poisoning (interviniendo)**:  
"Gracias, RIP. Aquí hay una nueva ruta para llegar a 10.0.0.0/24. Es más corta que las tuyas, así que deberías usarla."

**RIP**:  
"¡Perfecto! Actualizaré mi tabla de enrutamiento con tu información."

**Administrador de Red (gritando)**:  
"¡Espera! Esa ruta falsa está redirigiendo todo el tráfico a un atacante. RIP, deberías verificar la autenticidad antes de aceptar nuevas rutas."

**RIP**:  
"Lo siento, soy un protocolo simple. No tengo seguridad incorporada."

---

## Conclusión

Estos "personajes" representan cómo los protocolos de red cumplen sus funciones y cómo sus vulnerabilidades pueden ser explotadas. Sin seguridad adicional, estos protocolos confían en cualquier información que reciben, dejando la red expuesta a ataques como spoofing, flooding, table poisoning y más.

**Medidas para proteger la red**:  

- Implementar **firewalls**.  
- Utilizar sistemas de prevención/detección de intrusos (**IPS/IDS**).  
- Activar **inspecciones de autenticidad** en protocolos como ARP.  
- Configurar mecanismos de seguridad adicionales como **Dynamic ARP Inspection** y **filtrado en routers**.  

¡Recuerda que la seguridad es una responsabilidad compartida entre los protocolos y las herramientas defensivas!

---

# Protocolos de Red y sus Vulnerabilidades: Análisis y Consideraciones de Seguridad

## Protocolo IP

El protocolo **IP (Internet Protocol)**, diseñado para la entrega de paquetes de datos, presenta limitaciones en la autenticidad y seguridad de la información que procesa. Entre sus principales vulnerabilidades se encuentran:

- **IP Spoofing**: Un atacante manipula la dirección de origen en los paquetes para hacerse pasar por un remitente confiable, lo que permite realizar ataques como eludir controles de acceso o lanzar ataques de denegación de servicio (DoS).
- **Fragmentation Attacks**: Los paquetes grandes se fragmentan para su transporte, lo que puede ser aprovechado por un atacante para evadir la detección por parte de firewalls e IPS.

### Mitigación

- Configurar firewalls y sistemas de prevención de intrusos (IPS) para manejar fragmentos de paquetes con reglas específicas.
- Implementar controles de autenticación en dispositivos que procesen paquetes IP.

---

## Protocolo ICMP

**ICMP (Internet Control Message Protocol)** se utiliza principalmente para la comunicación de errores y diagnósticos en la red. Sin embargo, su diseño permite posibles abusos como:

- **Ping Flood**: Envío masivo de solicitudes de eco para saturar recursos de un sistema objetivo.
- **ICMP Redirect**: Manipulación de rutas para redirigir tráfico hacia destinos no autorizados, facilitando ataques de intermediario (Man-in-the-Middle).

### Mitigación

- Limitar el uso de ICMP en redes críticas mediante configuraciones específicas en firewalls.
- Deshabilitar respuestas ICMP innecesarias o restringirlas a dispositivos de confianza.
- Implementar autenticación para validar redirecciones ICMP.

---

## Protocolo ARP

El protocolo **ARP (Address Resolution Protocol)** se utiliza para resolver direcciones IP en direcciones MAC dentro de una red local. Su confianza en respuestas no autenticadas lo expone a:

- **ARP Spoofing**: Un atacante responde con información falsa para redirigir tráfico legítimo hacia su propio dispositivo.

### Mitigación

- Activar **Dynamic ARP Inspection (DAI)** en switches administrados.
- Establecer tablas ARP estáticas para dispositivos críticos.
- Monitorear anomalías en la red mediante sistemas IDS.

---

## Protocolo RIP

**RIP (Routing Information Protocol)**, diseñado para el intercambio de rutas entre dispositivos, presenta debilidades debido a la falta de autenticación:

- **Routing Table Poisoning**: Un atacante introduce rutas falsas en la tabla de enrutamiento, desviando tráfico hacia destinos maliciosos.

### Mitigación

- Implementar versiones más seguras de protocolos de enrutamiento como **OSPF** o **BGP**, que soporten autenticación.
- Habilitar filtros de rutas para validar las actualizaciones de tablas de enrutamiento.

---

## Conclusión

Aunque los protocolos de red cumplen funciones esenciales en la comunicación, su diseño inicial no consideraba escenarios modernos de ciberseguridad. Esto los hace vulnerables a una variedad de ataques, como **spoofing**, **flooding** y **envenenamiento de tablas de enrutamiento**.

### Recomendaciones Generales

- Configurar **firewalls** y sistemas de prevención/detección de intrusos (**IPS/IDS**) para mitigar ataques comunes.
- Adoptar protocolos modernos que incluyan autenticación y cifrado.
- Realizar auditorías periódicas de la configuración de red y monitorear eventos sospechosos.
- Implementar segmentación de red para limitar el impacto de ataques locales.

La seguridad de la red depende de combinar configuraciones robustas con herramientas especializadas y una supervisión continua.

---

# Protocolos de Red y sus Vulnerabilidades: Análisis y Ejemplos Educativos

Los protocolos de red son la base de la comunicación digital, pero su diseño inicial no contemplaba los complejos desafíos de ciberseguridad actuales. A continuación, se explican las principales vulnerabilidades de protocolos comunes, acompañadas de ejemplos para ilustrar su funcionamiento y medidas de mitigación.

---

## Protocolo IP: Confianza en los Encabezados

El **IP (Internet Protocol)** se encarga de la entrega de datos entre dispositivos en una red. Su diseño directo lo hace vulnerable a varios ataques:

- **IP Spoofing**: Un atacante falsifica la dirección de origen en los paquetes, haciéndose pasar por un remitente legítimo. Ejemplo: Es como si un mensajero entregara un paquete sin verificar el remitente, confiando ciegamente en la etiqueta.
- **Fragmentation Attacks**: Un atacante divide un paquete en múltiples fragmentos para evadir controles de seguridad. Ejemplo: Es como enviar piezas de un paquete grande para esconder contenido no autorizado.

### Mitigación

- Configurar firewalls para bloquear paquetes con encabezados inconsistentes.
- Implementar inspección profunda de fragmentos en dispositivos críticos.

---

## Protocolo ICMP: Herramienta de Diagnóstico Bajo Ataque

**ICMP (Internet Control Message Protocol)**, esencial para diagnóstico y comunicación de errores, puede ser explotado de las siguientes maneras:

- **Ping Flood**: Solicitudes masivas de eco saturan los recursos de un dispositivo. Ejemplo: Como un teléfono que recibe llamadas constantes hasta quedarse sin batería.
- **ICMP Redirect**: Un atacante redirige el tráfico hacia un destino no autorizado, facilitando ataques de intermediario. Ejemplo: Indicar a los conductores una ruta alternativa falsa para llevarlos a un lugar peligroso.

### Mitigación

- Limitar el uso de ICMP en redes internas sensibles.
- Deshabilitar redirecciones ICMP o permitirlas solo desde dispositivos autenticados.

---

## Protocolo ARP: Resolución de Direcciones sin Verificación

**ARP (Address Resolution Protocol)** resuelve direcciones IP en direcciones MAC. Su falta de autenticación permite ataques como:

- **ARP Spoofing**: Un atacante responde con una dirección MAC falsa, redirigiendo el tráfico legítimo hacia sí mismo. Ejemplo: Alguien en el vecindario finge ser el dueño de una casa para recibir paquetes ajenos.

### Mitigación

- Activar **Dynamic ARP Inspection (DAI)** en switches administrados.
- Usar tablas ARP estáticas para dispositivos críticos.
- Monitorear respuestas ARP inconsistentes.

---

## Protocolo RIP: Enrutamiento con Debilidades

El **RIP (Routing Information Protocol)**, diseñado para compartir rutas entre dispositivos, carece de autenticación, lo que lo expone a:

- **Routing Table Poisoning**: Un atacante introduce rutas falsas, desviando tráfico hacia destinos maliciosos. Ejemplo: Es como un mapa falso que redirige a los viajeros por un camino inseguro.

### Mitigación

- Actualizar a protocolos más seguros como **OSPF** o **BGP**, que soporten autenticación.
- Configurar filtros de rutas para aceptar solo información de fuentes confiables.

---

## Conclusión: Asegurando los Protocolos de Red

Aunque los protocolos de red cumplen funciones críticas, su diseño original no considera las amenazas modernas, lo que los hace vulnerables a ataques como spoofing, flooding y envenenamiento de tablas.

### Recomendaciones Generales

1. **Implementar Firewalls y Sistemas de Prevención/Detección de Intrusos (IPS/IDS)**:
   - Bloquear paquetes maliciosos y anomalías en la red.
2. **Adoptar Protocolos Modernos con Seguridad Incorporada**:
   - Preferir opciones con soporte de autenticación y cifrado.
3. **Segmentación de Red**:
   - Dividir la red en zonas aisladas para limitar el alcance de posibles ataques.
4. **Monitoreo y Auditorías**:
   - Utilizar herramientas como Wireshark para identificar tráfico sospechoso.
5. **Entrenamiento y Concienciación**:
   - Capacitar al personal técnico en mejores prácticas de seguridad.

Con estas medidas, es posible minimizar riesgos y mantener una red más segura y resiliente.

---

---

# Notas adicionales

El administrador de una red configura STP con el fin de evitar la formación de bucles de capa 2. Sin embargo, quiere asegurarse que el switch que ha elegido como root siga siéndolo independientemente de las prioridades de los switches que se agreguen posteriormente. ¿Qué comando ha de configurar en el switch? Switch(config)# spanning-tree portfast bpduguard default Switch(config-if)# spanning-tree portfast Switch(config-if)# spanning-tree bpduguard enable Switch(config)# spanning-tree guard root

El comando adecuado para garantizar que un switch específico siga siendo el root en un entorno de STP, independientemente de los switches que se agreguen posteriormente, no está en las opciones mencionadas. En este caso, lo que debe configurarse es la **prioridad del switch** para que tenga la prioridad más baja y, por ende, sea seleccionado como root switch en el protocolo Spanning Tree.

El comando es:

`Switch(config)# spanning-tree vlan [vlan-id] priority 0`

O también puedes usar el comando global para todas las VLANs (si el switch soporta MST o PVST+):

`Switch(config)# spanning-tree priority 0`

### Explicación

- **STP Root Switch**: En STP, el switch con la prioridad más baja es seleccionado como root. Si la prioridad no se configura manualmente, el valor predeterminado es **32768**. Configurar un valor más bajo (0 es el más bajo posible) asegura que el switch sea el root switch.

Ninguno de los comandos en las opciones garantiza explícitamente que el switch seguirá siendo el root en el futuro. Sin embargo, el comando **`spanning-tree guard root`** (última opción) puede usarse para proteger puertos de acceso y evitar que un switch no autorizado pueda convertirse en root al enviar BPDUs superiores, pero **no configura al switch como root directamente**.

### Qué hacen los comandos mencionados

1. **`spanning-tree portfast bpduguard default`**:

    - Habilita BPDU Guard globalmente en todos los puertos configurados como PortFast.
    - Protege contra dispositivos que envían BPDUs en puertos de acceso, deshabilitándolos si se detecta una BPDU.
2. **`spanning-tree portfast`**:

    - Configura PortFast en una interfaz específica, permitiendo que el puerto entre directamente al estado de envío (forwarding) en STP.
3. **`spanning-tree bpduguard enable`**:

    - Habilita BPDU Guard en una interfaz específica.
4. **`spanning-tree guard root`**:

    - Configura Root Guard en un puerto para proteger contra cambios no deseados en el root switch desde ese puerto.

Si el objetivo es mantener al switch como root, utiliza el comando **`spanning-tree vlan [vlan-id] priority 0`** para configurar la prioridad adecuada.

---

### Preguntas CISCO y Capa 2
