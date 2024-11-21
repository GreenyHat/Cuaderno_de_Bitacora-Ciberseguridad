## **Direcciones IP: Validez y Subredes**

### Validación de Direcciones IP

Una dirección IP es válida si todos sus octetos están dentro del rango permitido de 0 a 255.

**Ejemplos:**

- **Válidas:**
    - `1.1.1.1`
    - `80.34.56.1`
    - `154.22.56.89`
- **No válida:**
    - `91.257.12.34` (El segundo octeto es 257, fuera del rango permitido).

---

### Máscaras de Subred y Notación CIDR

La máscara de subred se utiliza para dividir una red en subredes más pequeñas e indica qué parte de una dirección IP pertenece a la red y qué parte a los hosts. 

#### Notación CIDR

- **/16**:  
    Máscara de subred `255.255.0.0`, donde los primeros 16 bits son para la red y los otros 16 para los hosts. Permite:
    - **65,536 direcciones totales** (256 subredes posibles si se subdividen en /24).
- **/24**:  
    Máscara de subred `255.255.255.0`, donde 24 bits son para la red y 8 bits para los hosts. Permite:
    - **256 direcciones por subred** (254 utilizables).

##### Características de la notación CIDR /16 
Máscara de subred: 255.255.0.0 
Número de direcciones IP disponibles: 65.536 
Número de subredes posibles: 256 Longitud de la máscara de subred: 16 bits 

**Por que 256 redes?**


La notación **CIDR /16** indica que los primeros 16 bits de la dirección IP están destinados a la parte de red, dejando los otros 16 bits restantes para asignar a los hosts dentro de esa red o para subdividir en subredes.

##### Razón por la que se obtienen **256 subredes**:

1. **Dirección IP total**: Una dirección IPv4 tiene 32 bits. En una red /16, los primeros 16 bits son para la parte de red y los otros 16 bits son para los hosts o subredes.
    
2. **Posibilidades de subredes**: Si decides subdividir la parte de host (los 16 bits restantes), puedes usar algunos de esos bits para identificar subredes.
    
    - Cada bit adicional que uses para subredes **duplica** la cantidad de subredes disponibles.
    - Si usas los 8 bits adicionales, tendrás 28=2562^8 = 25628=256 subredes.
3. **Cada subred tiene una máscara de /24**: Una subred en este esquema tendrá una máscara de **/24 (255.255.255.0)** porque ahora estás usando 24 bits para identificar la red/subred, dejando los últimos 8 bits para los hosts.
#### Cálculo de Subredes

Dividir una red en subredes más pequeñas incrementa el número de redes posibles a costa de reducir el número de hosts por subred.

- Ejemplo para /16 subdividido en /24:  
    **256 subredes posibles**, cada una con **254 hosts utilizables**.

#### Otros prefijos 
El prefijo, como **/16** o **/24**, indica cuántos bits en la máscara de subred están configurados en **1** (bits reservados para la red). Por ejemplo:

- **/16**: 255.255.0.0  
    Esto significa que los primeros 16 bits están configurados como 1. Ofrece un rango más grande de direcciones IP en una subred.
    
- **/24**: 255.255.255.0  
    Significa que los primeros 24 bits están configurados como 1. Ofrece 256 direcciones por subred, de las cuales 254 son utilizables.
    
- **/27**: 255.255.255.224  
    Tiene los primeros 27 bits configurados como 1. Esto ofrece un rango más pequeño, con 32 direcciones, de las cuales 30 son utilizables.
    
- **/30**: 255.255.255.252  
    Esta máscara se usa comúnmente en enlaces punto a punto, con solo 4 direcciones en la subred (2 utilizables).

##### **Cuándo usar cada prefijo:**

1. **/8, /16:**  
    Se usan para redes grandes, como en los rangos asignados a organizaciones o ISPs. Por ejemplo, un rango de tipo **10.0.0.0/8**.
    
2. **/24:**  
    Es el prefijo más común, ya que ofrece un buen balance entre la cantidad de direcciones IP utilizables (254) y el desperdicio de espacio.
    
3. **/30 y mayores (hasta /32):**  
    Se usan en casos muy específicos, como enlaces punto a punto (que necesitan solo 2 IPs) o para asignar una dirección única a un dispositivo específico en una red.
    
4. **/27, /28, /29:**  
    Se emplean en redes que requieren subredes más pequeñas, como oficinas o departamentos, para optimizar el uso de direcciones.

### **Riesgos de seguridad asociados:**

1. **Ataques de exploración (Scanning):**
    
    - Un atacante que sabe cuántos bits son de red y cuántos son de host **puede calcular el rango de direcciones IP disponibles**. Esto le permite realizar un **escaneo de red** para identificar dispositivos activos en esa subred.
	    - Hay herramientas para calcular el rango de direcciones IP disponibles en una máscara de subred? 


### **1. Herramientas en Línea**

- **Subnet Calculator (Calculadora de Subredes):** Estas herramientas te permiten introducir una dirección IP y su máscara de subred para calcular el rango disponible. Ejemplos:
    - Subnet Calculator de IP Calculator
    - Calculator.net Subnet Calculator

---

#### **2. Comandos de Línea de Comandos**

En sistemas operativos como **Linux**, **macOS**, o **Windows** con herramientas adicionales instaladas, puedes usar:

- **ipcalc** (Linux):
    
    
    `ipcalc <IP>/<PREFIX>`
    
    Ejemplo:
    
    `ipcalc 192.168.1.0/24` (**CIDR/24**)
			** el **prefijo `/24`** indica cuántos bits de los 32 totales (en una dirección IPv4) están reservados para identificar la red. En este caso:

- **24 bits** están destinados a la red.
- Los **8 bits restantes** están destinados a los hosts dentro de esa red.
    
    Resultado: Mostrará el rango de IPs, máscara de subred, y la dirección de broadcast.
    
- **Sipcalc**: Sipcalc también es una herramienta similar que da más detalles. Instalable en distribuciones como Debian o Ubuntu:
    
    bash
    
    Copiar código
    
    `sudo apt install sipcalc sipcalc <IP>/<PREFIX>`
    

---

#### **3. Uso de Scripts o Programación**

Puedes escribir un script sencillo en **Python** para calcular el rango de subredes usando el módulo `ipaddress`:

`import ipaddress  # Dirección IP y subred network = ipaddress.ip_network('192.168.1.0/24', strict=False)  # Rango de direcciones IP print(f"Rango de IPs disponibles: {network[1]} - {network[-2]}") print(f"Total de IPs en la subred: {network.num_addresses}")`

Resultado:

`Rango de IPs disponibles: 192.168.1.1 - 192.168.1.254 Total de IPs en la subred: 256`

---

#### **4. Software Dedicado**

- **Advanced IP Scanner**: Escanea redes y muestra rangos de IP.
- **SolarWinds IP Address Manager**: Administra y calcula subredes en redes más grandes.

---

#### **Cálculo Manual**

Si prefieres hacerlo manualmente:

1. **Convierte la máscara de subred a formato CIDR**. Ejemplo: `/24` equivale a `255.255.255.0`.
    
2. **Identifica el bloque de subred**. Para `/24`, el rango es `192.168.1.0` a `192.168.1.255`.
    
3. **Excluye las IP reservadas**:
    
    - Primera dirección: Dirección de red (`192.168.1.0`).
    - Última dirección: Broadcast (`192.168.1.255`).
    
    IPs utilizables: `192.168.1.1` a `192.168.1.254`.


    - Herramientas como **Nmap pueden ser usadas para escanear el rango completo de IPs, buscar puertos abiertos y obtener información de los servicios que están corriendo en los dispositivos.**
    
    
	
2. ***Spoofing* de IP:**
    
    - Si un atacante conoce la configuración de red, puede intentar falsificar (spoof) una dirección IP dentro del rango permitido. Esto puede usarse para acceder a servicios restringidos a direcciones IP específicas.
3. **Ataques de broadcast:**
    
    - **Las direcciones de broadcast (por ejemplo, `192.168.1.255` en una red `255.255.255.0`) pueden ser utilizadas para enviar paquetes maliciosos** o lanzar ataques de denegación de servicio (DoS) si no están configuradas correctamente.
4. **Acceso no autorizado en subredes mal segmentadas:**
    
    - Si todas las direcciones IP de una organización están en una sola subred (sin segmentación adecuada), un atacante puede moverse lateralmente más fácilmente tras comprometer un dispositivo.

---

### **Medidas de mitigación:**

1. **Segmentación de red (VLANs):**
    
    - Divide la red en subredes más pequeñas mediante VLANs para limitar el rango de direcciones IP accesibles a cada segmento.
    - Esto minimiza el impacto de un escaneo o movimiento lateral.
2. **Firewall y listas de control de acceso (ACLs):**
    
    - Configura reglas de firewall para restringir el tráfico entre subredes y evitar el acceso no autorizado.
    - Implementa listas de control de acceso (ACLs) para permitir solo IPs confiables.
3. **Monitorización y detección de intrusos:**
    
    - Utiliza **herramientas como un sistema de detección de intrusos (IDS) o un sistema de prevención de intrusos (IPS)** para **identificar** y **bloquear** **actividades sospechosas**, como escaneos de red o intentos de spoofing.
4. **Deshabilitar respuestas ICMP innecesarias:**
    
    - Las respuestas ICMP (como los pings) pueden ser útiles para los atacantes que intentan mapear la red. Limita el ICMP en tu red si no es esencial.
5. **Filtrado de IP en switches:**
    
    - Usa controles de seguridad en switches, como **ARP inspection** o **IP source guard**, para evitar que dispositivos no autorizados se conecten a la red con IPs falsificadas.
6. **Direcciones IP privadas y NAT:**
    
    - Usa direcciones IP privadas (no enroutables en Internet) para la red interna, y emplea un firewall con NAT para exponer solo los servicios necesarios al exterior.
7. **Configuración del broadcast:**
    
    - Asegúrate de que las direcciones de broadcast no sean accesibles desde fuera de la red, y configura reglas para minimizar el impacto de ataques DoS que exploten esta dirección.
8. **Implementación de límites en herramientas de escaneo:**
    
    - Muchas herramientas de monitoreo de red permiten limitar el rango de IPs que pueden ser escaneadas incluso por administradores. Esto reduce la exposición a ataques internos.

---

### **Resumen:**

El conocimiento de las máscaras de subred puede ser aprovechado por atacantes para mapear y explotar redes. Sin embargo, con una configuración adecuada (segmentación, firewalls, monitorización y políticas estrictas), estos riesgos pueden mitigarse significativamente.

---

## **Encapsulación de Paquetes y Niveles de Red**

### Encapsulación de Datos en Red

Cuando un paquete viaja por una red:

1. Se desencapsula hasta el nivel **Capa 2 (enlace, Ethernet)** en cada salto (router).
2. La dirección IP (Capa 3) no cambia, pero las direcciones físicas (MAC) se actualizan.

#### Encapsulación Típica:

1. **ICMP** (Protocolo de Control de Mensajes de Internet): Mensaje de error o diagnóstico.
2. **Paquete IP**: Contiene información de origen/destino.
3. **Trama Ethernet**: Encierra el paquete IP para la transmisión física.

---

## **Protocolos Importantes**

### **TCP (Transmission Control Protocol)**

- **Fiable y orientado a conexión.**
- Garantiza:
    - Orden correcto de los datos.
    - Retransmisión en caso de pérdida.
- Usos: Navegación web, FTP, SSH.

---

### **UDP (User Datagram Protocol)**

- **No fiable y sin conexión.**
- Características:
    - Sin confirmación de entrega.
    - No asegura el orden ni retransmite datos.
- Usos: Streaming, juegos en línea, VoIP.

#### Vulnerabilidades y Contramedidas

- **Explotaciones comunes:**
    - **Ataques de reflexión/amplificación:** Aprovechan la falta de conexión para redirigir tráfico hacia la víctima.
- **Medidas de mitigación:**
    - Filtrado de paquetes en firewalls.
    - Límites de respuesta en servidores.
    - Uso de protocolos más seguros como QUIC.

---

### **ICMP (Internet Control Message Protocol)**

- Utilizado para diagnósticos y notificaciones de errores.
- Ejemplo: Herramientas como `ping` y `traceroute`.
- Encapsulado en paquetes IP para la transmisión.

---

### **ARP (Address Resolution Protocol)**

- Traduce direcciones IP a direcciones MAC en redes locales.
- Almacena temporalmente relaciones en una tabla ARP para futuras consultas.

---

## **Direcciones MAC**

- **Identificador único de 48 bits** grabado en hardware.
- Usada en la **Capa 2** para identificar dispositivos en la red.
- Ejemplo de obtención:
    - **Windows:** `ipconfig /all`
    - **Linux/Mac:** `ifconfig` o `ip addr`.

---

## **Mecanismos de Traducción de Direcciones**

### **CGNAT (Carrier-Grade Network Address Translation)**

- Permite que múltiples usuarios compartan una sola dirección IP pública.
- Ventajas:
    - Conserva direcciones IPv4.
- Desventajas:
    - Dificultades en configuración de puertos.
    - Problemas de rendimiento en servicios como juegos en línea.

---

## **Configuración de Redes en Debian**

Archivo principal: `/etc/network/interfaces`.

Ejemplo de configuración estática:

`auto eth0 iface eth0 inet static   address 192.168.1.100   netmask 255.255.255.0   gateway 192.168.1.1   dns-nameservers 8.8.8.8 8.8.4.4`

---

## **Unidad de Transmisión Máxima (MTU)**

- Tamaño máximo de paquete sin fragmentación.
- Ejemplo:
    - Ethernet estándar: **1500 bytes**.
    - Tramas gigantes: **9000 bytes**.
- Fragmentación ocurre si el paquete excede la MTU.

---

## **Ataques Relacionados y Contramedidas**

### **Ataques de UDP**

- **Reflexión:** Redirige tráfico hacia la víctima.
- **Amplificación:** Multiplica el tráfico enviado a la víctima.

#### Contramedidas:

1. Filtrado de tráfico UDP.
2. Uso de protocolos avanzados (QUIC).
3. Implementación de límites de tasa.

---

### **Modificación de ARP y DNS**

- **Riesgos:**
    - Alterar `/etc/hosts` o la tabla ARP puede redirigir tráfico legítimo.
- **Contramedidas:**
    - Monitoreo de cambios en archivos críticos.
    - Uso de herramientas como `Tripwire`.

---

## **Herramientas para Redes**

### **Escaneo y Subredes**

- **`ipcalc`:** Calcula rango y detalles de subred.
    - Ejemplo: `ipcalc 192.168.1.0/24`.
- **`sipcalc`:** Similar a `ipcalc` con más detalles.
- **Advanced IP Scanner:** Escanea redes para identificar dispositivos.

- **ipcalc** (Linux):
    
    
    `ipcalc <IP>/<PREFIX>`
    
    Ejemplo:
    
    `ipcalc 192.168.1.0/24` (**CIDR/24**)
			** el **prefijo `/24`** indica cuántos bits de los 32 totales (en una dirección IPv4) están reservados para identificar la red. En este caso:

- **24 bits** están destinados a la red.
- Los **8 bits restantes** están destinados a los hosts dentro de esa red.
    
    Resultado: Mostrará el rango de IPs, máscara de subred, y la dirección de broadcast.

### **Análisis de Seguridad**

- **Nmap:** Escaneo de puertos y servicios.
- **Wireshark:** Análisis de tráfico en tiempo real.

### **Diagnóstico**

- **Ping:** Verifica conectividad.
- **Traceroute:** Rastrea la ruta de paquetes en la red.


# _Mis preguntas_

## Cómo aprovechar en un ataque la no fiabilidad del protocolo UDP y cómo se contraresta? Puedo experimentar?

### UDP Inseguridad en Ataques

La no fiabilidad del protocolo UDP se puede aprovechar en ataques denominados “ataques de reflexión” o “ataques de amplificación”. Estos ataques explotan la falta de conexión y la no garantía de entrega de paquetes en UDP.

1. **Ataque de reflexión**: Un atacante envía un paquete UDP con una dirección IP de origen falsificada y una dirección IP de destino válida. El servidor intermedio, creyendo que el paquete proviene de la dirección IP falsificada, responde con un paquete UDP dirigido a esa dirección IP falsificada. El atacante, entonces, redirige el tráfico de respuesta hacia la víctima, inundándola con un gran volumen de tráfico y sobrecargando su capacidad de procesamiento.
    
2. **Ataque de amplificación**: Similar al ataque de reflexión, pero el atacante utiliza un servidor intermedio con una alta capacidad de procesamiento y respuesta rápida. El atacante envía paquetes UDP con direcciones IP de origen falsificadas y direcciones IP de destino válidas. El servidor intermedio responde con paquetes UDP dirigidos a las direcciones IP falsificadas, y el atacante redirige el tráfico hacia la víctima, amplificando la cantidad de tráfico y aumentando la carga de trabajo.
    

### Contrarrestar

Para contrarrestar estos ataques, se pueden implementar las siguientes medidas:

1. **Filtrado de tráfico**: Implementar firewalls y sistemas de detección de intrusiones que filtran el tráfico UDP y bloqueen paquetes con direcciones IP de origen falsificadas.
    
2. **Limitación de la tasa de respuesta**: Configurar los servidores para limitar la tasa de respuesta a paquetes UDP y evitar que se sobrecarguen con tráfico de ataque.
    
3. **Diversificación de la infraestructura**: Distribuir la infraestructura de red y servicios en múltiples centros de datos para reducir la vulnerabilidad a ataques de amplificación.
    
4. **Autenticación y autorización**: Implementar mecanismos de autenticación y autorización para verificar la identidad de los clientes y servidores, reduciendo la posibilidad de ataques de reflexión.
    
5. **Monitoreo y detección**: Implementar sistemas de monitoreo y detección de intrusiones para detectar y responder rápidamente a ataques de UDP.
    
6. **Diseño de aplicaciones seguras**: Diseñar aplicaciones que sean resistentes a ataques de UDP, utilizando técnicas como checksums y verificación de integridad de datos.
    
7. **Seguridad en la capa de aplicación**: Implementar seguridad en la capa de aplicación, como autenticación y autorización, para proteger contra ataques de UDP y otros tipos de ataques.


### Mejorar Robustez en UDP Apps

Para mejorar la robustez de aplicaciones que utilizan UDP para manejar pérdida y corrupción de paquetes, se pueden implementar las siguientes estrategias:

1. **Reenvío de paquetes**: La aplicación puede implementar un mecanismo de reenvío para paquetes que no se confirmaron como recibidos por el receptor. Esto garantiza que los datos importantes se envíen nuevamente en caso de pérdida.
2. **Checksums y verificación de integridad**: La aplicación puede calcular checksums (como CRC-32 o MD5) para los paquetes y verificar su integridad en el receptor. Si un paquete se corrompe durante el transporte, la aplicación puede detectarlo y solicitar reenvío.
3. **Nivel de redundancia**: La aplicación puede utilizar técnicas de redundancia, como envío de paquetes duplicados o triplicados, para garantizar que los datos importantes se reciban correctamente.
4. **Timeouts y retransmisión**: La aplicación puede establecer timeouts para paquetes que no se han recibido y retransmitirlos automáticamente en caso de pérdida.
5. **Implementación de protocolos de corrección de errores**: La aplicación puede utilizar protocolos de corrección de errores, como el protocolo de corrección de errores de Reed-Solomon, para detectar y corregir errores en los paquetes.
6. **Monitorización y análisis de paquetes**: La aplicación puede monitorear y analizar los paquetes perdidos o corruptos para identificar patrones y problemas en la red, lo que puede ayudar a mejorar la robustez y la fiabilidad de la aplicación.
7. **Uso de protocolos de transporte avanzados**: La aplicación puede utilizar protocolos de transporte avanzados, como el protocolo QUIC (Quick UDP Internet Connections), que incluyen mecanismos de corrección de errores y retransmisión integrados.
8. **Implementación de mecanismos de detección de duplicados**: La aplicación puede implementar mecanismos para detectar y eliminar paquetes duplicados, lo que ayuda a evitar problemas de orden y fiabilidad.
9. **Uso de redes y routers con capacidad de retransmisión**: La aplicación puede utilizar redes y routers que tienen capacidad de retransmisión, lo que ayuda a reducir la pérdida de paquetes y mejorar la robustez.
10. **Pruebas exhaustivas**: La aplicación debe ser sometida a pruebas exhaustivas para garantizar que funcione correctamente en condiciones de pérdida y corrupción de paquetes.

Es importante destacar que, aunque estas estrategias pueden mejorar la robustez de aplicaciones que utilizan UDP, no pueden garantizar la entrega segura y confiable de paquetes en todas las circunstancias. La aplicación debe ser diseñada y probada para funcionar correctamente en condiciones de red variables y con un nivel razonable de pérdida y corrupción de paquetes.