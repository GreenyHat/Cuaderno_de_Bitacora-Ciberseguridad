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

Las operaciones básicas de la capa 3 son:

- Direccionamiento de dispositivos finales
- Encapsulación
- Enrutamiento
- Desencapsulación

### Características del protoclo IP

![alt text](<Captura de pantalla (471).png>)
![alt text](<Captura de pantalla (472).png>)
![alt text](<Captura de pantalla (473).png>)

#### Paquete IPv4

![alt text](<Captura de pantalla (474).png>)

#### Paquete IPv6

![alt text](<Captura de pantalla (475).png>)

#### Sobre la coexistencia de ambos protocolos

- **Dual-stack**
   Dispositivos y redes soportan tanto IPv4 como IPv6, seleccionando el protocolo más adecuado para cada conexión.
  
![alt text](<Captura de pantalla (476).png>)

- **Tunelización**
  Encapsula tráfico IPv6 dentro de paquetes IPv4 para atravesar redes que solo soportan IPv4.


![alt text](<Captura de pantalla (477).png>)

- **Traducción (NAT64, NAT46)**
  Convierte paquetes entre IPv4 e IPv6, permitiendo la comunicación entre dispositivos que solo soportan uno de los dos protocolos

![alt text](<Captura de pantalla (478).png>)

### Tipos de enrutamiento

## Enrutamiento
- **Elemento principal de capa 3**: Router.
  - Determina la mejor ruta para reenviar paquetes eligiendo la interfaz adecuada.

## ¿Estático o Dinámico?

| Característica       | **Routing Estático**                       | **Routing Dinámico**                     |
|----------------------|--------------------------------------------|------------------------------------------|
| **Complejidad de configuración** | Dependiente del tamaño                         | Independiente del tamaño                 |
| **Cambios de topología**        | Requiere intervención manual                  | Adaptación automática                    |
| **Escalabilidad**               | Limitada                                      | Alta                                     |
| **Seguridad**                   | Inherente                                     | Debe configurarse                        |
| **Uso de recursos**             | Sin recursos adicionales                     | Utiliza CPU, memoria y ancho de banda    |
| **Predictibilidad de ruta**     | Definida por el administrador                | Depende de la topología                  |

---

## Clasificación del Routing Dinámico

### Conceptos clave

1. **Estructura de datos**: Uso de tablas o bases de datos.
2. **Mensajes del protocolo de routing**: Intercambio de información con routers vecinos.
3. **Algoritmo**: Determina el mejor camino.

### Protocolos y Métricas

| **Protocolo** | **Métrica**                                    |
|---------------|-----------------------------------------------|
| **RIP**       | Recuento de saltos                            |
| -             | Cada router equivale a un salto.              |
| -             | Límite de saltos: 15.                        |
| **OSPF**      | Coste (ancho de banda acumulado origen-destino).|
| -             | A mayor velocidad, menor coste.              |
| **EIGRP**     | Basado en el ancho de banda más lento y retardos anormales. |
| -             | Puede incluir carga y fiabilidad.            |

---

### Mejor Camino

- Depende del protocolo y la métrica utilizada (saltos, coste, ancho de banda, etc.).


## Comando: `show ip route`
- **S**: Ruta estática.
- **\***: Ruta predeterminada.
- **O**: Ruta dinámica (OSPF).
- **C**: Red conectada directamente.
- **L**: Interfaz del router.

### Principios de la Tabla de Routing
1. Cada router toma sus decisiones de forma autónoma.
2. Las tablas de enrutamiento entre routers pueden no coincidir.
3. La información de enrutamiento no asegura retorno al origen.

---

## Tipos de Rutas

### Redes Conectadas Directamente
- Indicadas con:
  - **C**: Red conectada directamente, incluye dirección IP y máscara.
  - **L**: Interfaz del router (IPv4: /32, IPv6: /128).

### Rutas Estáticas
- Configuradas manualmente:
  - IPv4: `ip route 10.0.4.0 255.255.255.0 10.0.3.2`
  - IPv6: `ipv6 route 2001:db8:acad:4::/64 2001:db8:acad:3::2`

### Rutas Dinámicas
- Redes descubiertas automáticamente mediante protocolos de enrutamiento.

### Ruta Predeterminada
- Utilizada en ausencia de una ruta más específica.
- Reduce el número de rutas en la tabla.
- Representada como:
  - IPv4: `0.0.0.0/0`
  - IPv6: `::/0`

---

## Elección de la Mejor Ruta

### Definición
- La mejor ruta es la de **coincidencia más larga** en la tabla de enrutamiento.

### Ejemplo de IPv4
1. Dirección del host de destino: `172.16.0.10`
   - Binario: `10101100.00010000.00000000.00001010`
2. Longitudes de prefijo en la tabla:
   - `172.16.0.0/12`: Coincidencia parcial.
   - `172.16.0.0/18`: Coincidencia más larga.
   - `172.16.0.0/26`: **Mejor ruta** (coincidencia más específica).

## Rutas estáticas

### Ventajas y Desventajas de las Rutas Estáticas

| **Ventajas**                           | **Desventajas**                           |
|----------------------------------------|-------------------------------------------|
| Fácil de implementar en redes pequeñas | Baja escalabilidad                        |
| Muy seguro                             | Mayor complejidad en redes grandes        |
| Ruta fija y predecible                 | Requiere intervención manual, no recalcula rutas automáticamente |
| No utiliza recursos adicionales        |                                          |

---

### Verificación de Rutas Estáticas
- Comando: `show ip route`
- Permite comprobar la configuración y estado de las rutas.

---

### Ejemplo de Configuración en Packet Tracer

#### Topología de Red
- Redes IPv4:
- `192.168.1.0/24`, `192.168.2.0/24`, `192.168.3.0/24`
- `10.0.0.0/32`, `10.0.0.4/32`, `10.0.0.8/32`
- Rutas:
- **Estándar**: Redes conectadas directamente.
- **Flotantes**: Alternativas en caso de fallo.

#### Configuración de Rutas Predeterminadas
- Configuración básica para rutas principales y redundantes en IPv4 e IPv6.

## RIP

### Descripción General
- Protocolo de encaminamiento dinámico interior. Establece las entradas del router se aprenden mediante los mensajes del protocolo dentro de una **intranet**
  
- Utiliza **UDP** en el puerto **520**.
- Métrica: **número de saltos**.
- Algoritmo: **Bellman-Ford**. Calcula una ruta óptima con un máximo de saltos en 15
- Límite de saltos: **15**.
- Implementa:
  - **Regla de horizonte dividido**.
  - **Envenenamiento de rutas**.
- Versiones:
  - **RIPv1**: IPv4 sin clase.
  - **RIPv2**: IPv4 con clase, soporta VLSM y CIDR.
  - **RIPng**: RIP para IPv6.

---

### Comparativa RIPv1 vs RIPv2

| Característica             | **RIPv1**                                | **RIPv2**                                |
|----------------------------|------------------------------------------|------------------------------------------|
| Métrica                    | Número de saltos                        | Número de saltos                        |
| Límite de saltos           | 15                                      | 15                                      |
| Distancia administrativa   | 120                                     | 120                                     |
| Tipo de transmisión        | **Broadcast** (255.255.255.255)         | **Multicast** (224.0.0.9)               |
| Soporte VLSM y CIDR        | No                                      | Sí                                      |
| Autenticación              | No                                      | Sí (MD5)                                |

---

### Modos de Funcionamiento y Tipos de Mensajes

#### Modos de Funcionamiento

1. **Activo**:
   - Operado por routers.
2. **Pasivo**:
   - Conexión con usuarios finales.

#### Tipos de Mensajes
1. **Request**:
   - Solicita parte o toda la tabla de rutas.
2. **Reply**:
   - Respuesta a un request.
   - Actualizaciones periódicas o por cambios de topología.

---

## Temporizadores de RIP

| **Temporizador**           | **Intervalo**                            |
|----------------------------|------------------------------------------|
| Periódico (update timer)   | 30 segundos                             |
| Caducidad (invalid timer)  | 180 segundos                            |
| Purga (flush timer)        | 240 segundos                            |
| Retención (holddown timer) | 180 segundos                            |

![alt text](<Captura de pantalla (479).png>)
---

## Configuración de RIPv2

`R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network <network-address>
R1(config-router)# passive-interface <interface-id>
R1(config-router)# redistribute static
R1(config)# ip route 0.0.0.0 0.0.0.0 <next-hop-address | exit-intf>`

## RIP Poisoning: Concepto, Ejemplo y Contramedidas

## ¿Qué es RIP Poisoning?

El **RIP Poisoning** (envenenamiento de RIP) es un ataque en redes que utiliza el protocolo de enrutamiento RIP (**Routing Information Protocol**). Este ataque ocurre cuando un actor malintencionado inyecta rutas falsas o engañosas en las tablas de enrutamiento de los routers, provocando interrupciones en el tráfico de red. 

El objetivo del ataque puede ser:
- Redirigir el tráfico
- Interceptar información sensible.
- Denegar el acceso a ciertas rutas (causar una denegación de servicio).

RIP Poisoning explota la simplicidad de RIP, que carece de mecanismos sólidos de autenticación en su diseño original.

---

## Ejemplo de RIP Poisoning

Supongamos la siguiente red:

- **Router A** tiene conectividad con la red `192.168.1.0/24`.
- **Router B** tiene conectividad con la red `192.168.2.0/24`.
- Ambos routers utilizan RIP para intercambiar información de enrutamiento.

Un atacante introduce un dispositivo en la red (por ejemplo, una computadora con software malicioso) que simula ser un router legítimo. Este dispositivo envía actualizaciones RIP indicando que puede alcanzar `192.168.1.0/24` con una métrica muy baja (cercana a 1), incluso si no es cierto.

### Resultado:

1. **Router B** actualiza su tabla de enrutamiento con la información falsa del atacante.
2. Todo el tráfico destinado a `192.168.1.0/24` es redirigido hacia el dispositivo malicioso.
3. El atacante puede:
   - Interceptar el tráfico.
   - Modificar los datos.
   - Bloquear la comunicación.

---

## Contramedidas para prevenir RIP Poisoning

### 1. Autenticación de RIP MD5

- Utiliza versiones de RIP que soporten autenticación (por ejemplo, RIP v2).
- Configura claves compartidas para verificar la legitimidad de las actualizaciones RIP.

**Ejemplo de configuración en un router Cisco:**

router rip
version 2
network 192.168.1.0
network 192.168.2.0
passive-interface default
ip rip authentication mode md5
ip rip authentication key-chain RIP-KEYS

### 2. Filtrado de Rutas

Implementa filtros para aceptar solo actualizaciones provenientes de fuentes confiables.

### Ejemplo de configuración en un router Cisco

access-list 10 permit 192.168.1.1
access-list 10 deny any
router rip
distribute-list 10 in

### 3. Uso de Protocolos de Enrutamiento Más Seguros

Considera migrar a protocolos más modernos y seguros, como OSPF o EIGRP, que incluyen mecanismos robustos contra ataques de envenenamiento.

### 4. Monitorización de la Red

Implementa sistemas de detección de intrusiones (IDS) para identificar actividades sospechosas relacionadas con RIP.
Revisa periódicamente las tablas de enrutamiento para detectar rutas anómalas o inesperadas.

---

## OSPF

- **Protocolo de enrutamiento**: OSPF es un protocolo de enrutamiento de estado de enlace utilizado en redes IP para calcular rutas óptimas mediante el **algoritmo de Dijkstra.** Es tambien de encaminamiento dinámico interior, igual que RIP. Hay dos versiones:
  - v2 --> IPv4
  - v3 --> IPv6
- **Áreas**: Las redes OSPF se dividen en áreas para optimizar el uso de recursos y reducir el tamaño de las tablas de enrutamiento. **Todas las áreas deben conectarse al Área 0 (backbone)**.
  ![alt text](<Captura de pantalla (484).png>)
- **Tipos de routers**:
  - **Router Interno (IR)**: Pertenece a una sola área.
  - **Router Backbone (BR)**: Forma parte del Área 0.
  - **Router de Área Fronteriza (ABR)**: Conecta múltiples áreas.
  - **Router de Frontera del Sistema Autónomo (ASBR)**: Conecta redes OSPF con sistemas autónomos externos.
- **Coste de enlace**: Métrica basada en el ancho de banda para calcular rutas óptimas.

---

## Estados de funcionamiento de OSPF (estados del router)

1. **Down**: El router no ha recibido paquetes Hello del vecino.
2. **Init**: Se reciben paquetes Hello, pero la relación de vecindad aún no se ha establecido.
3. **Two-Way**: Vecinos reconocidos, pero no seleccionados como DR (Designated Router) o BDR (Backup Designated Router).
4. **ExStart**: Los routers negocian roles y sincronizan bases de datos.
5. **Exchange**: Intercambio inicial de descripciones de la LSDB (Link State Database).
6. **Loading**: Solicitud y carga de enlaces adicionales necesarios.
7. **Full**: Relación de vecindad completa y sincronización de la LSDB.

### Tipos de paquete

![alt text](<Captura de pantalla (482).png>)

## Funcionamiento de OSPF (Área Única)

1. **Configuración inicial**:
   - Asignar ID del router (`router-id`).
   - Habilitar OSPF en interfaces (`network`).
   - Definir el área (`area 0` para área única).
2. **Descubrimiento de vecinos**:
   - Los routers intercambian paquetes Hello para identificar vecinos.
   - Verifican parámetros comunes: área, temporizadores y autenticación.
3. **Establecimiento de la adyacencia**:
   - Los routers negocian roles y sincronizan bases de datos mediante LSA (Link State Advertisement).
4. **Cálculo de rutas**:
   - Usan el algoritmo SPF (Shortest Path First) para calcular las rutas más óptimas y generar la tabla de enrutamiento.

---

## Fallos de seguridad y contramedidas

### Fallos de seguridad

1. **Intercepción de tráfico (Man-in-the-Middle)**:
   - Un atacante puede interceptar o modificar el tráfico OSPF.
2. **Inyección de LSA falsas**:
   - Un atacante podría enviar LSA falsas para desestabilizar la red.
3. **Negación de servicio (DoS)**:
   - Sobrecarga de procesadores de routers enviando paquetes OSPF maliciosos.
4. **Vecinos no autorizados**:
   - Establecimiento de relaciones de vecindad no autorizadas.

### Contramedidas

1. **Autenticación OSPF**:
   - Configurar autenticación MD5 o SHA para proteger paquetes OSPF:
     ```
     interface GigabitEthernet0/0
     ip ospf authentication message-digest
     ip ospf message-digest-key 1 md5 <contraseña>
     ```
2. **Filtrado de acceso**:
   - Usar ACLs para limitar el acceso a los routers solo desde dispositivos autorizados.
3. **Configuración de temporizadores**:
   - Ajustar temporizadores Hello y Dead para minimizar el impacto de ataques DoS.
4. **Segmentación de áreas**:
   - Dividir la red en áreas para reducir la propagación de fallos y mejorar la seguridad.
5. **Monitoreo y logs**:
   - Habilitar registros de OSPF para detectar actividades inusuales:
     ```ogging ospf events```
6. **Actualizaciones y parches**:
   - Mantener el software del router actualizado para mitigar vulnerabilidades conocidas.

---

**Nota**: Estas configuraciones son ejemplos básicos para el estudio.

## Protocolos de Redundancia de Primer Salto (FHRP)

### ¿Qué son?

Los protocolos de redundancia de primer salto (First Hop Redundancy Protocols, FHRP) aseguran la alta disponibilidad en las redes al permitir la conmutación transparente entre puertas de enlace de primer salto. Esto elimina puntos únicos de fallo (SPOF).

### Ejemplos

- **HSRP (Hot Standby Router Protocol)**: Propiedad de Cisco. Utiliza una dirección IP y MAC virtuales para conmutación transparente en IPv4.
- **VRRP (Virtual Router Redundancy Protocol)**: Protocolo estándar abierto con funcionalidad similar a HSRP.
- **GLBP (Gateway Load Balancing Protocol)**: Propiedad de Cisco. Además de la conmutación, permite balanceo de carga entre routers.

---

## Funcionamiento del HSRP

### Descripción General

- Protocolo de capa 3 propiedad de Cisco.
- Utiliza puertas de enlace redundantes para garantizar conmutación por fallo transparente.
- Define direcciones IP y MAC virtuales.
- Forma un grupo HSRP con un router activo y uno o más de respaldo, eliminando puntos únicos de fallo (SPOF).
- Comunicación mediante:
  - **Dirección multicast**: 224.0.0.2 (versión 1) o 224.0.0.102 (versión 2).
  - **Puerto UDP**: 1985.

### Estados del Protocolo

1. **Inicial**: El router comienza el proceso HSRP.
2. **Aprendizaje**: El router espera escuchar al activo. No tiene IP virtual.
3. **Escucha**: No es activo ni de respaldo, pero escucha a estos.
4. **Hablar**: Participa en la elección del activo y respaldo; envía mensajes de saludo cada 3 segundos.
5. **Standby**: Router de respaldo esperando 10 segundos para asumir como activo.
6. **Activo**: Gana la elección y asume el rol de router activo.

### Configuración Básica

1. Configuración del router de respaldo:

``R2(config)# interface interface-id``
``R2(config-if)# standby version 2``
``R2(config-if)# standby 1 ip virtual-ip-address``

2. Configuración del router activo:

`R1(config)# interface interface-id`
`R1(config-if)# standby version 2`
`R1(config-if)# standby 1 ip virtual-ip-address`
`R1(config-if)# standby 1 priority priority-value`
`R1(config-if)# standby 1 preempt`

### Fallos de Seguridad en HSRP

#### Tipos de Ataques

- Robo del rol de router activo: Interceptación de datos (Man in the Middle): Lectura, modificación o borrado de información transmitida.
- Denegación de servicio (DoS): Impide acceso a recursos o dispositivos
- Degradación de servicio: Paquetes fraudulentos intermitentes generan cortes​

#### Configuración Segura de HSRP

##### Buenas Prácticas

- Autenticación MD5:
- Asegura la integridad de la comunicación: `R1(config-if)# standby 1 authentication md5 key-chain key-chain-name`
- Prioridad Máxima al Router Activo
- Define el router activo claramente: `R1(config-if)# standby 1 priority 255`
- Uso de ACLs:
  - Standard: Filtra por origen.
  - Extendida: Filtra por origen, destino y aplicación.
- Implementación de IPSec:
- Garantiza confidencialidad, integridad y autenticación de las conexiones:

## Capa 7

La capa de aplicación es fundamental en el modelo OSI y TCP/IP, ya que define las herramientas y protocolos necesarios para que los usuarios interactúen con las redes de manera eficiente. A través de modelos como Cliente/Servidor o P2P, y protocolos clave como HTTP o DNS, garantiza que la información fluya correctamente entre aplicaciones y dispositivos.

- Es la **capa más cercana al usuario final**.
- Proporciona la interfaz para la **comunicación entre el usuario y la red**.
- Facilita servicios y aplicaciones utilizados directamente por los usuarios.

## Ejemplos de Aplicaciones en la Capa de Aplicación

- Terminal virtual.
- Gestión de ficheros.
- Servicio de correo electrónico.
- Servicio de directorio.

## La Capa de Aplicación en el Modelo TCP/IP

En el modelo TCP/IP, las funciones de las capas **Aplicación, Presentación y Sesión** del modelo OSI están combinadas en una sola capa llamada **Aplicación**:

- **Aplicación**: Interfaz entre las aplicaciones del usuario y la red
- **Presentación**: Tratamiento y formato de los datos.
- **Sesión**: Creación y mantenimiento de diálogos.

## Modelos de Comunicación

### Modelo Cliente/Servidor

- **Cliente**: Solicita información.
- **Servidor**: Provee información.
- Requiere autenticación en algunos casos.
- Es una red **centralizada**, con el servidor como punto central.
- Problema: **SPOF** (Single Point of Failure).

### Modelo P2P (Peer-to-Peer)

- Consta de dos elementos principales:
  - **Red P2P**:
    - Cada dispositivo actúa como cliente y servidor.
    - Permite compartir recursos como conexión a Internet o juegos.
  - **Aplicaciones P2P**:
    - Gestionan la funcionalidad para actuar simultáneamente como cliente y servidor.

## Protocolos de la Capa de Aplicación

### Resolución de Nombres de Dominio

- **DNS**: Traduce nombres de dominio a direcciones IP.

### Configuración de Dispositivos

- **DHCP**: Asigna automáticamente direcciones IP y otros parámetros de red.

### Correo Electrónico

- **SMTP**: Envío de correos.
- **POP3/IMAP**: Recepción de correos.

### Transferencia de Ficheros

- **FTP**: Transferencia completa y confiable de archivos.
- **TFTP**: Transferencia simplificada y menos segura.

### Navegación Web

- **HTTP/HTTPS**: Protocolo para la transferencia de hipertexto y su versión segura.


## DNS y DHCP: Funcionamiento, Configuración y Seguridad

## Funcionamiento de DNS

El **Sistema de Nombres de Dominio (DNS)** es una base de datos jerárquica y distribuida que traduce nombres de dominio a direcciones IP. Opera bajo un modelo cliente-servidor y utiliza el puerto 53, compatible con UDP y TCP.

### Características principales

- Traduce nombres de dominio web a direcciones IP.
- Utiliza una arquitectura cliente/servidor.
- Verifica primero la caché local y, en caso de no encontrar la dirección, consulta al servidor DNS del ISP.

### Jerarquía DNS

- Root Servers
- Servidores de Dominio de Nivel Superior (TLD)
- Servidores Autoritativos

### Comando para consultas

``
nslookup <dominio>
``

---

## Funcionamiento de DHCP

El **Protocolo de Configuración Dinámica de Host (DHCP)** asigna dinámicamente direcciones IP y otros parámetros de red. Se encapsula en los puertos UDP 67 (servidor) y 68 (cliente).

### Características principaales

- Modelo cliente/servidor.
- Asignación dinámica y fácil administración de direcciones IP.
- Compatible con IPv4 (DHCP) e IPv6 (DHCPv6).

### Flujo de funcionamiento

1. **Descubrimiento (Discover)**: El cliente busca un servidor DHCP.
2. **Oferta (Offer)**: El servidor responde con una dirección IP disponible.
3. **Solicitud (Request)**: El cliente solicita la dirección ofrecida.
4. **Confirmación (ACK)**: El servidor confirma la asignación.

### Configuración básica

`` R1(config)# ip dhcp pool <pool-name>``
``R1(dhcp-config)# network <network-address> <mask>``
``R1(dhcp-config)# default-router <gateway-address>``
``R1(dhcp-config)# dns-server <dns-server-address>``
``R1(dhcp-config)# domain-name <domain>``
``R1(dhcp-config)# lease {days [hours [minutes]] | infinite``

---

## Fallos de Seguridad en DHCP

Los ataques comunes a DHCP incluyen:

### DHCP Starvation

- Objetivo: Agotar las direcciones IP del servidor.
- Método: El atacante envía múltiples solicitudes falsas.

### DHCP Spoofing

- Objetivo: Suplantar al servidor legítimo.
- Método: El atacante responde antes que el servidor real.

### DHCP ACK Injection

- Variante avanzada de DHCP Spoofing donde el atacante envía respuestas fraudulentas tras las respuestas legítimas.

---

## Configuración Segura de DHCP

### Mitigación de Ataques

1. **DHCP Snooping**: Filtra mensajes DHCP en puertos no confiables.
  ``R1(config)# ip dhcp snooping``
  ``R1(config)# ip dhcp snooping vlan <vlan-number>``
  ``R1(config-if)# ip dhcp snooping trust``
  ``R1(config-if)# ip dhcp snooping limit rate <rate>``
  
2. **Seguridad de Puertos**: Restringe dispositivos conectados por puerto.

    ``Switch(config-if)# switchport port-security``
    ``Switch(config-if)# switchport port-security mac-address sticky``

3. **ACLs (Listas de Control de Acceso)**:
    - **Standard**: Filtra por origen.
    - **Extendida**: Filtra por origen, destino y aplicación.

4. **Apagar puertos no utilizados**:
``Router(config-if)# shutdown``

---

### Verificaciones

#### Verificar configuración DHCP

``show ip dhcp pool``
``show ip dhcp binding``


#### Verificar cliente DHCP

``ipconfig /all  # En Windows``
``dhclient -v     # En Linux``

## Conclusión

El correcto funcionamiento y la seguridad de DNS y DHCP son pilares esenciales en cualquier infraestructura de red. Implementar configuraciones seguras y realizar verificaciones periódicas garantiza una red eficiente y protegida contra amenazas.

## SNMP y OSPF: Funcionamiento, Configuración y Seguridad

## Funcionamiento de SNMP

El **Protocolo Simple de Administración de Red (SNMP)** facilita el intercambio de información de administración en dispositivos de red. Opera en los puertos UDP 161 (administración) y 162 (traps).

### Características principales

- Modelo cliente/servidor.
- Componentes: Administrador SNMP (NMS), agente SNMP, dispositivos administrados y MIB (Base de Información de Gestión).
- Versiones: SNMPv1, SNMPv2c, SNMPv3 (más segura).

### Tipos de mensajes SNMP

- **get-request**: Solicita el valor de una variable MIB.
- **get-next-request**: Busca variables secuenciales.
- **set-request**: Modifica el valor de una variable MIB.
- **trap**: Mensajes de eventos enviados por el agente.

### Configuración básica SNMPv2c

``R1(config)# snmp-server community <password> ro``
``R1(config)# snmp-server community <password> rw``

---

## Configuración Segura de SNMP

### Mitigación de ataques

1. **Desactivar SNMP en dispositivos innecesarios**:

  ``R1(config)# no snmp-server community <password>``
  
2. **Usar SNMPv3**:
   - Autenticación con MD5/SHA.
   - Cifrado con DES/AES.
3. **Configurar SNMP view**:
  
  ``R1(config)# snmp-server view <view-name> <oid {included | excluded}>``

---

## Fallos de Seguridad de SNMP

### Ataques comunes

1. **Acceso ilegítimo**:
   - Uso de comunidades por defecto.
   - Captura de paquetes con sniffers.
2. **SNMP Reconnaissance**:
   - Obtención de información de red y sistema.

---

## Funcionamiento de OSPF

El **Protocolo de Enrutamiento de Estado de Enlace (OSPF)** facilita el enrutamiento dinámico dentro de redes autónomas.

### Características principaless

- Algoritmo Dijkstra.
- Áreas jerárquicas para escalabilidad.
- Intercambio de información mediante LSAs (Anuncios de Estado de Enlace).

---

## Fallos de Seguridad en OSPF

### Ataques comuness

1. **Denegación de Servicio (DoS)**:
   - Saturación con LSAs fraudulentos.
2. **Remote False Adjacency**:
   - Introducción de routers "fantasma".
3. **Disguised LSA**:
   - Suplantación de LSAs legítimos.

---

## Configuración Segura de OSPF

### Mitigación de ataques

1. **Autenticación MD5**:
``R1(config-if)# ip ospf message-digest-key <key-number> md5 <password>``

2. **Definir interfaces pasivas**:
``R1(config-router)# passive-interface <interface-id>``
3. **Habilitar TTL Security**:
``R1(config-router)# ttl-security all-interfaces hops <hop-count>``


## Conclusión
La seguridad y configuración adecuada de SNMP y OSPF son esenciales para garantizar la integridad y disponibilidad de las redes. Utilizar las versiones más seguras y aplicar medidas de mitigación protege contra amenazas comunes.


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
