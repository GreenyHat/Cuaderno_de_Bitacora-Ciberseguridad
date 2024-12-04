# Ethical Hacking

## ¿Qué entendemos por seguridad digital?

La seguridad digital se refiere a la protección de sistemas, redes y datos frente a amenazas cibernéticas. En el contexto de la seguridad informática, implica técnicas de detección de activos vulnerables y la explotación de vulnerabilidades asociadas a los mismos. Existen explotaciones públicas que los agentes de seguridad ofensiva pueden utilizar para llevar a cabo pruebas de penetración y otras actividades de hacking ético.

## Seguridad Informática
- Técnicas de detección de activos vulnerables y explotación de las vulnerabilidades asociadas a ellos.
- Existen explotaciones públicas que el agente de seguridad ofensiva puede emplear.



# Escaneo de Redes (Nmap)

## Activos y Servicios

### ¿Qué es Nmap?
Nmap (Network Mapper) es una herramienta de código abierto para exploración de red y auditoría de seguridad. Se utiliza para descubrir hosts y servicios en una red informática.

### ¿Qué es Nmap?
- Identificación rápida de dispositivos activos en la red.
- Comandos básicos para escanear rangos de IP:
  - nmap -sP 192.168.0.1/24
  - nmap 192.168.0.*

## Usos de Nmap

### Detección de Servicios y Aplicaciones
- Detección de servicios y aplicaciones que se ejecutan en los puertos abiertos.
- Escaneo de **todos** los puertos:
  - nmap -sV [IP o dominio]
  - nmap -p80,443,8080 [IP o dominio]
  - nmap -p- -sV [IP o dominio]

### Identificación del Sistema Operativo
- Mediante los servicios detectados, puedes hacerte una idea del sistema operativo, si este no es exfiltrado en el proceso de reconocimiento de versiones de servicios. Sin embargo, podemos valernos de:
  - nmap -O [IP o dominio] (Este comando requiere permisos administrativos).

### Escaneo de Puertos UDP
- Escaneo de puertos UDP:
  - nmap -sU [IP o dominio]

### Escaneo Rápido
- Para un escaneo rápido:
  - nmap -F [IP o dominio]

### Formatos de Salida en Nmap
- Podemos controlar la salida de Nmap a ficheros distintos. Los formatos de salida de Nmap son:
  - Nmap
  - Grep output
  - XML
- Comando para salida:
  - nmap <ip> -oA output_filename

## Más en Nmap

### Nmap Scripting Engine (NSE)
- Nmap incluye scripts que automatizan ciertas detecciones o ataques contra servicios. Estos usan el Nmap Scripting Engine (NSE), basado en el lenguaje LUA:
  - nmap --script vuln [IP o dominio]
  - nmap --script firewall-bypass [IP o dominio]

### Precauciones y Consideraciones Éticas
- El escaneo de redes ha implicado un debate sobre su legitimidad y responsabilidad de uso. Siempre obtener permiso antes de escanear redes que no te pertenezcan.
- Puede existir un impacto en la red derivado de posibles riesgos de sobrecarga en dispositivos de red durante escaneos agresivos.

# Escaneo Básico de Vulnerabilidades
## CVEs y Exploits Públicos

### Exploits & CVEs
- Fuentes de malware
- Análisis de explotaciones

## CVE & Exploits

### ¿Qué es un Exploit?
Un exploit es un fragmento de software, un conjunto de datos o una secuencia de comandos que aprovecha una vulnerabilidad en software para causar comportamientos no previstos, como la obtención de control sobre un sistema.

### ¿Qué es un CVE?
Un CVE (Common Vulnerabilities and Exposures) es un identificador estándar para una vulnerabilidad de seguridad conocida. El sistema CVE proporciona una referencia de seguridad pública para cada vulnerabilidad conocida.

El formato para las entradas CVE es:
- CVE-YYYY-NNNN
  (YYYY indica el año y NNNN el número de vulnerabilidad)
- Desde enero de 2014, este identificador puede contener, si es necesario, más de cuatro dígitos.

## Fuentes Públicas de Exploits
- Shodan
- Exploit-DB
- Sploitus
- CVE Database

## Reconocimiento de Explotabilidades

### Fuentes Públicas
- **cvedetails.com**
- **EXPLOIT-DB** (API - searchsploit)
  - Ejemplo: searchsploit apache 2.4

### Análisis de Explotaciones
- **Sploitus:**
  - Ejemplo de uso: Navegar a sploitus.com y buscar por la tecnología o el tipo de vulnerabilidad. Ej. "remote code execution"

- **Acceso a CVE Database:**
  - Ejemplo de uso: Navegar a cvedetails.com y buscar información detallada de CVEs específicos, tecnologías asociadas, incluso pruebas de concepto públicas.

  El almacenaje de exploits y CVEs es crucial para la comprensión de los mismos y la mejora en cuanto a seguridad informática. Herramientas y recursos proporcionados son esenciales para mantenerse informado y protegido.


# Introducción a la Seguridad Ofensiva

La seguridad ofensiva es el conjunto de prácticas y técnicas utilizadas para identificar, explotar y mitigar vulnerabilidades en sistemas y redes, anticipándose a los ataques maliciosos. En lugar de adoptar una postura reactiva (defensiva), se toma un enfoque proactivo al simular ataques para evaluar la seguridad de los sistemas.

## Fundamentos Básicos

La seguridad ofensiva implica diversos aspectos, entre los cuales se destacan:

- **Exploración de vulnerabilidades**: Analizar sistemas para identificar fallas que puedan ser explotadas.
- **Pruebas de penetración (Pen Testing)**: Intentos controlados de hackear sistemas para encontrar debilidades.
- **Simulación de ataques avanzados**: Emular ataques sofisticados como los de grupos APT (Amenazas Persistentes Avanzadas).

## Ramas de la Seguridad Ofensiva

### 1. Sistemas (Linux/Windows)
- **Explotación**: Desarrollo y ejecución de exploits específicos para vulnerabilidades en sistemas operativos.
- **Hardening**: Fortalecimiento de la configuración de sistemas para prevenir ataques.
- **Explotación de Kernel**: Explotación de vulnerabilidades en el núcleo del sistema operativo.
- **Powershell Scripting**: Uso de scripts para automatizar tareas de hacking.

### 2. Programación y Desarrollo de Malware (MalDev)
- **Scripting**: Automatización de tareas de hacking con lenguajes como Python, Bash y PowerShell.
- **Desarrollo de Malware**: Creación de software malicioso para probar las defensas.
- **Ingeniería Inversa**: Análisis de malware existente para comprender su funcionamiento y desarrollar mitigaciones.
- **Simulación de Ransomware**: Pruebas de ataques de ransomware para evaluar la respuesta de los sistemas.

### 3. Tecnologías Web
- **Hacking de Aplicaciones Web**: Explotación de vulnerabilidades como SQL Injection, XSS, CSRF, entre otros.
- **API Security**: Identificación de vulnerabilidades en interfaces de programación de aplicaciones (REST, SOAP).
- **Explotación de CMS**: Ataques dirigidos a plataformas como WordPress, Joomla, y Drupal.

### 4. Otros Vectores de Ataque
- **OSINT (Open Source Intelligence)**: Recolección de información pública para preparar ataques.
- **Seguridad en Redes Inalámbricas**: Ataques a redes WiFi y Bluetooth.
- **Seguridad Móvil**: Explotación de vulnerabilidades en dispositivos Android/iOS.
- **Cloud Security**: Explotación de infraestructuras en la nube como AWS, Azure, y Google Cloud.

### 5. Red Team Operations
- **Simulación APT**: Evaluación de la capacidad de respuesta a ataques avanzados y persistentes.
- **Escenarios Basados en Ataques**: Pruebas de políticas de seguridad mediante simulaciones realistas.

### 6. Seguridad en IoT
- **Explotación de Dispositivos IoT**: Ataques dirigidos a dispositivos como cámaras de seguridad, electrodomésticos inteligentes, y sistemas de control industrial.

### 7. Machine Learning y AI Security
- **Adversarial ML**: Manipulación de modelos de aprendizaje automático para realizar ataques.

### 8. Seguridad en Automoción
- **Explotación de Redes Vehiculares**: Ataques a redes de vehículos como CAN (Controller Area Network).
- **Hacking de Sistemas de Infotainment**: Explotación de vulnerabilidades en sistemas de entretenimiento de vehículos.

## Herramientas de Seguridad Ofensiva

### Linux
- **Chkrootkit**: Herramienta para detectar rootkits.
- **Linux Exploit Suggester**: Sugerencias de exploits para versiones específicas de Linux.

### Windows
- **Mimikatz**: Herramienta para extraer contraseñas y hashes.
- **BloodHound**: Utiliza grafos para analizar la seguridad de Active Directory.

### Tecnologías Web
- **SQLMap**: Herramienta automatizada para explotación de inyección SQL.
- **XSSer**: Framework para detectar y explotar vulnerabilidades XSS.

### Redes y MITM
- **MITM6**: Herramienta para ataques MITM sobre IPv6.
- **BetterCAP**: Framework de ataques MITM.



# Introducción al Hacking Web

El hacking web implica la explotación de vulnerabilidades en aplicaciones web con el objetivo de obtener acceso no autorizado o realizar acciones no previstas por los desarrolladores de la aplicación.

## Web Sec

- Dada la naturaleza crítica de muchas aplicaciones web, que manejan datos personales y financieros, entender y poder mitigar estas vulnerabilidades es esencial para la seguridad en línea.
- El hacking web es un campo desafiante pero crucial en la seguridad ofensiva.
- Herramientas como **Burp Suite** son indispensables para los profesionales de la seguridad para realizar pruebas efectivas y profundas de las aplicaciones web.

## Vulnerabilidades Web

### Ataques del Lado del Cliente

#### Cross-Site Scripting (XSS)
- Sucede cuando los atacantes logran inyectar scripts maliciosos en el contenido de una página web que luego es visto por otros usuarios.
- Este script puede ejecutarse en el navegador del usuario para robar información o defraudar al usuario.

#### Cross-Site Request Forgery (CSRF)
- Un ataque CSRF engaña al navegador del usuario para que realice una acción no deseada en un sitio web al que está autenticado, como cambiar la contraseña de su cuenta o transferir dinero.

### Ataques del Lado del Servidor

#### Inyección SQL
- Ocurre cuando un atacante puede insertar o "inyectar" un código SQL malicioso en los inputs de una aplicación para ser ejecutado por el servidor de base de datos.
- Esto puede permitir la manipulación de bases de datos y la revelación de información sensible.

#### Inclusión de Archivos (LFI/RFI[Local File Inclusion/Remote File Inclusion])
- Los ataques de inclusión de archivos se producen cuando un atacante consigue que un archivo, script o comando se ejecute en el servidor, lo que puede resultar en la ejecución de código, escalada de privilegios, o divulgación de información.

## Metodología de Trabajo

### Burp Suite Proxy
- Burp Suite es una de las herramientas más populares y completas para el testing de seguridad de aplicaciones web.
- Ofrece una variedad de funcionalidades diseñadas para facilitar el proceso de encontrar y explotar vulnerabilidades en aplicaciones web.

#### Funcionalidades Principales
- **Proxy Interceptor**: Permite ver y modificar el tráfico entre el navegador y el servidor web en tiempo real.
- **Escáner de Seguridad**: Automatiza la detección de vulnerabilidades.
- **Intruder**: Herramienta para realizar ataques automatizados para encontrar y explotar vulnerabilidades.
- **Repeater**: Permite manipular y reenviar peticiones HTTP individuales.
- **Sequencer**: Analiza la calidad de los tokens de sesión para determinar su aleatoriedad y previsibilidad.

### Uso de Burp Suite Proxy
- Para iniciar con Burp Suite, configure el navegador para que utilice el proxy de Burp y empiece a interceptar solicitudes.
- Utilice el escáner para realizar un análisis inicial y determinar puntos débiles.
- Explore vulnerabilidades específicas con Intruder y Repeater para entender su alcance y potencial impacto.

## Prueba de Concepto con BurpSuite


![alt text](1.png)
![alt text](2.png)
![alt text](3.png)
![alt text](4.png)
![alt text](5.png)

> **Sobre BurpSuite** es interesante resaltar el _scope_. Si nos vamos a HTTPS history y veremos mucho ruido de peticiones, pero si nos centramos en el dominio o dominios que nos interesan y los scopeamos (CDR >> add to scope) y luego en _Intercept_ filtramos show only in-scope items nos mostrará solo lo que nos interesa

Al interceptar una solicitud, en el caso de la prueba de concepto, estamos observando una petición POST que es para cambiar la clave. En el cuerpo de la Request podremos hacer CDR y enviarla al **Repeater** (Tambien con ctrl + R)

![alt text](<Captura de pantalla (485).png>)

para enviar la petición modificada en algun encabezado para probar distintas interacciones en la API

![alt text](<Captura de pantalla (486).png>)

# ARPSpoofing básico. Ataque MITM en LAN

## Componentes de una Red Local (LAN)
- **Dispositivos de Red**: Incluyen switches, routers, puntos de acceso, que facilitan la comunicación entre dispositivos en la red.
- **Medios de Transmisión**: Cables (como Ethernet) o inalámbricos (como WiFi) que conectan los dispositivos.
- **Direcciones IP y MAC**: Identificadores únicos para dispositivos en la red; IP para la capa de red y MAC para la capa de enlace de datos.

## Prácticas de Seguridad en Enrutamiento y Switching
### ACL (Listas de Control de Acceso)
- Las ACL se utilizan para permitir o denegar tráficos específicos basados en direcciones IP, puertos o protocolos.
- Implementadas en routers y switches para controlar el acceso y limitar la propagación de ataques.

### Seguridad de Capa 2
- Técnicas como el **Dynamic ARP Inspection (DAI)** y **DHCP Snooping** aseguran la autenticidad y seguridad de los mensajes ARP y las asignaciones DHCP dentro de la LAN, protegiendo contra ataques como **ARP Spoofing** y **Rogue DHCP Servers**.

### Hardening de Dispositivos
- La configuración segura de routers y switches es crucial.
  - Cambiar contraseñas predeterminadas.
  - Deshabilitar servicios innecesarios.
  - Actualizar firmware y software a las versiones más seguras.
  - Monitorear los dispositivos para detectar actividades sospechosas.

## Vulnerabilidades en Redes LAN
- **Accesos no Autorizados**: Ingreso a la red por usuarios no autorizados.
- **Ataques Internos**: Ataques originados dentro de la propia red.
- **Sniffing**: Captura pasiva de tráfico en la red para obtener información sensible (MITM).

## Mecanismos de Defensa
### Autenticación y Control de Acceso
- Restricciones de acceso a la red a través de autenticación para garantizar que solo dispositivos autorizados puedan acceder.

### Cifrado
- Uso de tecnologías como **VPNs** para cifrar el tráfico de datos y proteger la información en tránsito.

### Segmentación de Red
- Dividir la red en subredes para limitar el acceso y minimizar los riesgos.

---

# Ataques MITM (Man-in-the-Middle)

![alt text](<Captura de pantalla (487).png>)

## Definición
- Los ataques de **Man-in-the-Middle (MITM)** son técnicas utilizadas por los atacantes para intervenir en la comunicación entre dos entidades sin que ellas lo sepan.
- Los objetivos de estos ataques pueden variar desde la intercepción de datos hasta la alteración o el bloqueo de la comunicación.

## Interceptar Tráfico
### Pasiva
- El atacante escucha el tráfico entre las partes sin alterar los datos.
- Ejemplo: sniffing de paquetes en redes no seguras, como WiFi público.

### Activa
- El atacante inserta un intermediario entre las dos partes para capturar y potencialmente alterar la comunicación, aunque inicialmente solo puede estar escuchando los datos.

#### Herramientas

- **WireShark**: Herramienta de analisis de red para capturar paquetes de datos a tiempo real
- **Sniffers** de red de terceros o propio (exsiten librerias para tal fin): Captura tráfico de red en interfaces donde este pasa con cidrado debil o sin cifrar
- **MITMProxy**: navega a traves de un servicio. Permite interpectar HTTP y HTTPS

---

# Pruena de concepto (Maquinas Windows y Kali)


1. Crea maquina W
2. Comprueba el direccionamiento ARP en W arp -a o /a??
3. Abre ettercap con Kali
   1. scanea host
   2. selecciona el objetivo
   3. inicia el sniffing
4. Abre paralelamente Wireshark y captura solo trafico DNS
5. Observa que pasa al realizar un ping a una pagina desde la maquina W en el wireshark de Kali


# Ataques RCE (Remote Code Execution) y algunas metodologías

RCE se refiere a la capacidad de ejecutar código en un sistema remoto, es decir, en un servidor o una máquina víctima, desde una máquina atacante. Esto permite al atacante ejecutar comandos en la máquina víctima, lo que puede ser utilizado para extraer información, tomar control del sistema, o realizar acciones malintencionadas.

## **Reverse Shells y Acceso Remoto**

En el ámbito de la **seguridad ofensiva** y el **hacking ético**, obtener acceso remoto a un sistema comprometido es crucial para realizar análisis y pruebas de penetración. Existen varias técnicas que permiten establecer este acceso, entre las cuales destacan **Reverse Shells**, **Bind Shells** y **Webshells**. A continuación, se profundiza en cada una de ellas.

## **1. Reverse Shell**

Una **Reverse Shell** es un tipo de shell donde el **sistema comprometido** (cliente) establece la conexión hacia el **atacante** (servidor). Este método es útil cuando el objetivo está detrás de un firewall o en una red interna que no permite conexiones entrantes.

### **Características clave:**
- El sistema objetivo se conecta a un puerto específico en el servidor atacante.
- Evita bloqueos de firewall o NAT que podrían prevenir conexiones entrantes.
- El atacante puede ejecutar comandos de manera remota en el sistema comprometido.

