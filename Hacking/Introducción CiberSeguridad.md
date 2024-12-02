# Introducción a la CiberSeguridad

## Conceptos clave

- **La triada CIA**: Factores a defender en el software: Confindencialidad, Integirdad y Disponibilidad. Otras: trazabilidad y no repudio.

>Nota externa al curso (En adelante NEC) Lo elementos clave para realizar una correcta auditoría de seguridad son los siguientes, en orden: Analisis del entorno o entornos con posibles vulnerabiliadades, compartimentación de los mismos (servidores, redes, protocolos, conexiones, puntos de acceso, hardware, versiones de software...), Descripción clara y concisa de cada vector de ataque encontrado y del proceso para llegar a dicho vector y documentación técnica con imágenes y muestras de código de todo lo realizado.
**Nota adicional:** Todo lo que puede llevarse a cabo por el pentester (sobre todo si no es un _intern_) debe quedar bien específicado por contrato para no tener problemas legales.

- **BIA (Business Impact Analysis)**: Muestra el coste de los servicios o elementos críticos de una empresa interrumpidos tras un ataque. Esto a su vez nos permite analizar la "criticidad" de los distintos proceso de la empresa, elaborar planes de recuperación y estimar el tiempo de recuperación para distintos niveles de interrupción.
- **Defensa en profundidad (o en capas)**: Se basa en la premisa de que todo componenete de un sistema puede ser explotado por lo que no se recomienda usar una única defensa o delegar la seguridad a un único método de protección.
- **Incidente de seguridad**: Cualquier elemento que viole cualquier apartado de la triada CIA o ponga en riesgo cualquier servicio de la empresa de cualquier otra forma (elementos sospechosos).
- **SGSI**: Es un sistema de gestión de seguridad de la información que facilitan los huecos que hay que tapar para no incurrir en violaciones de la CIA. Un SGSI evalúa los riesgos asociados con los datos e información y define las aplicaciones de control necesarias para eliminar o minimizar sus consecuencias negativas.

## Modelo RMIAS (Modelo de referencia del aseguramiento y seguridad de la información)

![alt text](<Captura de pantalla 2024-12-02 a las 13.41.12.png>)

1. Ciclo de vida de desarrollo de seguridad: Este dimension se centra en la planificación y ejecución de la seguridad en el desarrollo de sistemas y aplicaciones.
2. Taxonomía de la información: Esta dimensión se enfoca en la clasificación y categorización de la información según su nivel de sensibilidad y riesgo.
3. Objetivos de seguridad: Esta dimensión define los objetivos de seguridad, incluyendo la confidencialidad, integridad, disponibilidad, rendición de cuentas, no repudio, auditabilidad, autenticidad y confiabilidad, y privacidad.
4. Dimensiones de contramedidas de seguridad: Esta dimensión se centra en la implementación de medidas de seguridad efectivas para proteger la información y sistemas.

El RMIAS también incluye el IAS-octave, un conjunto de ocho metas de seguridad que reemplazan la tríada CIA (Confidencialidad, Integridad y Disponibilidad). El IAS-octave se basa en una extensa análisis de la literatura sobre seguridad de la información y sistemas de ingeniería, y fue evaluado a través de entrevistas con expertos en seguridad. Puede ser utilizado para:

Desarrollar documentos de política de seguridad de la información
Structurar el pensamiento en seguridad de la información en una organización
Catalogar la investigación existente en el dominio
Ayudar a los nuevos entrantes en el dominio de la seguridad de la información a comprender la complejidad y diversidad del mismo
El RMIAS se publica bajo una licencia Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International.

### Ventajas

Proporciona un marco de referencia para la ciberseguridad que abarca todas las dimensiones
Ayuda a las organizaciones a desarrollar una cultura de seguridad
Permite la catalogación y comprensión de la investigación existente en el dominio
Es independiente de la tecnología y puede ser aplicado por organizaciones de cualquier tamaño y dominio

### Desventajas

Puede ser complejo de implementar y requerir un alto nivel de conocimiento en ciberseguridad
Requiere una cultura organizacional que apoye la seguridad y la privacidad de la información.

## Principales amenazas

### Sophos Threat Report 2024/25

#### Principales Vulnerabilidades Identificadas en 2024

- **Ransomware**: Continúa siendo la mayor amenaza para las pequeñas y medianas empresas (PYMES), causando interrupciones significativas en las operaciones y pérdidas financieras considerables.  
  Fuente: [Sophos News](https://news.sophos.com/en-us/2024/03/12/2024-sophos-threat-report/)

- **Malware de Robo de Datos y Credenciales**: Casi el 50% del malware detectado en PYMES incluye keyloggers, spyware y stealers, utilizados para sustraer información sensible y credenciales de acceso.  
  Fuente: [Sophos Press](https://www.sophos.com/es-es/press/press-releases/2024/03/2024-sophos-threat-report-cybercrime-main-street-details-cyberthreats)

- **Ataques de Ingeniería Social**: Incremento en ataques que emplean técnicas de ingeniería social más sofisticadas, como la "carnicería de cerdos" (shā zhū pán), que combinan estafas románticas con fraudes financieros.  
  Fuente: [Sophos News](https://news.sophos.com/en-us/2024/03/12/2024-sophos-threat-report/)

- **Abuso de Controladores**: Los atacantes explotan controladores vulnerables o maliciosos, firmados con certificados robados o fraudulentos, para evadir y desactivar las defensas contra malware en sistemas gestionados.  
  Fuente: [Sophos News](https://news.sophos.com/en-us/2024/03/12/2024-sophos-threat-report/)

- **Ataques Basados en la Web**: Aumento en la distribución de malware a través de malvertising o SEO malicioso, superando las restricciones impuestas por el bloqueo de macros maliciosos en documentos.  
  Fuente: [Sophos News](https://news.sophos.com/en-us/2024/03/12/2024-sophos-threat-report/)

- **Ataques a dispositivos móviles**:  
  Se ha observado un aumento en estafas basadas en ingeniería social dirigidas a usuarios de dispositivos móviles, afectando tanto a individuos como a PYMES.  
  [Más información](https://news.sophos.com/en-us/2024/03/12/2024-sophos-threat-report/?utm_source=chatgpt.com)

- **Distribución de malware basada en la web**:  
  Se ha intensificado el uso de técnicas como malvertising y SEO envenenado para propagar malware, especialmente tras la restricción de macros maliciosos en documentos.  
  [Más información](https://news.sophos.com/en-us/2024/03/12/2024-sophos-threat-report/?utm_source=chatgpt.com)  

- **Dispositivos desprotegidos en la red**:  
  Equipos sin protección adecuada o con configuraciones incorrectas representan puntos de entrada críticos para ataques cibernéticos.  
  [Más información](https://news.sophos.com/en-us/2024/03/12/2024-sophos-threat-report/?utm_source=chatgpt.com)

- **Ingeniería social sofisticada**:  
  Los atacantes emplean tácticas avanzadas de ingeniería social para engañar a empleados y usuarios, aumentando la efectividad de sus ataques.  
  [Más información](https://news.sophos.com/en-us/2024/03/12/2024-sophos-threat-report/?utm_source=chatgpt.com)

#### Vulnerabilidades Previstas para 2025

- **Ataques Potenciados por Inteligencia Artificial (IA)**: Se anticipa que los ciberdelincuentes utilizarán IA para desarrollar ataques más sofisticados y personalizados, aumentando la eficacia de sus campañas maliciosas.  
  Fuente: [Cinco Días](https://cincodias.elpais.com/smartlife/lifestyle/2024-11-26/ciberseguridad-en-2024-apuesta-2025-inteligencia-artificial.html)

- **Riesgos en la Cadena de Suministro**: La creciente interconexión entre empresas podría ser explotada para comprometer sistemas a través de proveedores o socios menos protegidos, afectando a múltiples organizaciones.  
  Fuente: [Cinco Días](https://cincodias.elpais.com/smartlife/lifestyle/2024-11-26/ciberseguridad-en-2024-apuesta-2025-inteligencia-artificial.html)

- **Adopción de Tecnologías Cuánticas para Cifrado**: Aunque aún en desarrollo, la computación cuántica podría representar una amenaza para los métodos de cifrado actuales, requiriendo la adaptación a nuevos estándares de seguridad.  
  Fuente: [Cinco Días](https://cincodias.elpais.com/smartlife/lifestyle/2024-11-26/ciberseguridad-en-2024-apuesta-2025-inteligencia-artificial.html)

- **Proliferación de Dispositivos IoT**: El aumento de dispositivos conectados amplía la superficie de ataque, especialmente si estos dispositivos carecen de medidas de seguridad adecuadas.  
  Fuente: [Cinco Días](https://cincodias.elpais.com/smartlife/lifestyle/2024-11-26/ciberseguridad-en-2024-apuesta-2025-inteligencia-artificial.html)

- **Convergencia de Roles Tecnológicos**: La integración de diversas funciones tecnológicas puede crear brechas de seguridad si no se gestionan adecuadamente, facilitando el acceso no autorizado a sistemas críticos.  
  Fuente: [Cinco Días](https://cincodias.elpais.com/smartlife/lifestyle/2024-11-26/ciberseguridad-en-2024-apuesta-2025-inteligencia-artificial.html)

Es esencial que las organizaciones permanezcan vigilantes y adopten medidas proactivas para mitigar estas amenazas emergentes, incluyendo la implementación de soluciones de seguridad avanzadas, la capacitación continua del personal y la colaboración con expertos en ciberseguridad.

### Predicciones Ciso Mag para 2025

- **Riesgos en la cadena de suministro**: Los ciberdelincuentes se enfocarán en atacar cadenas de suministro críticas, utilizando phishing mejorado con IA y suplantaciones deepfake para eludir las defensas (Check Point).
- **Computación cuántica y cifrado**: El avance de esta tecnología obligará a industrias a adoptar cifrado cuántico seguro para proteger datos sensibles (Check Point).
- **Proliferación de dispositivos IoT**: Se prevé que para 2025 habrá 32.000 millones de dispositivos IoT, lo que ampliará la superficie de ataque. Las organizaciones deben adoptar arquitecturas de Zero Trust para mitigar los riesgos asociados (Check Point).
- **Convergencia de CIO y CISO**: La creciente adopción de la IA y de entornos de nube híbrida llevará a la convergencia de las funciones de CIO y CISO, con un enfoque más integrado de gestión de riesgos (Check Point).
- **Combate a amenazas sofisticadas**: El enfoque se centrará en combatir amenazas como el ransomware y el phishing, mientras se mejoran las estrategias defensivas para garantizar la resistencia operativa y seguridad (Agnidipta Sarkar, Vice President, CISO Advisory, ColorTokens).
- **Finanzas y seguridad**: La integración de métricas financieras con datos de seguridad técnica se convertirá en crítica. El sector llamará a esto “CRQ” (cybersecurity risk management), pero en realidad es la gestión de riesgos de seguridad (Mayuresh Dani, Manager, Security Research for Qualys Threat Research Unit).
- **Expansión del attack surface**: La adopción de AI/ML crecerá el attack surface, con nuevos componentes, puertos abiertos y cuentas de shadows, lo que llevará a más vulnerabilidades e infraestructuras inseguras (Mayuresh Dani, Manager, Security Research for Qualys Threat Research Unit).
- **Continuidad de operaciones digitales**: La capacidad de continuar operaciones digitales a pesar de ataques cibernéticos se convertirá en una preocupación prioritaria en discusiones de consejo de administración. La efectiva reportación de incidentes de seguridad también será un requisito importante para los CISO (Check Point).
  
En resumen, para 2025, los CISO (Chief Information Security Officer) deben estar preparados para:

Combatir amenazas sofisticadas y mejorar las estrategias defensivas
Integrar finanzas y seguridad para una gestión de riesgos efectiva
Manejar la expansión del attack surface y reducir la exposición a brechas
Fomentar la convergencia de CIO y CISO para una gestión de riesgos integrada
Proteger la privacidad y seguridad de datos en la era de la computación cuántica y la proliferación de dispositivos IoT.
>Más información en [Ciso Mag](https://cisomag.com/)

### OWASP Top 10

1. **Broken Access Control**  
   Fallos en la implementación de control de acceso que permiten a usuarios no autorizados acceder a datos o realizar acciones restringidas.  
   [Más información](https://owasp.org/www-project-top-ten/)

2. **Cryptographic Failures**  
   Deficiencias en el uso de criptografía, como el almacenamiento inseguro de datos sensibles o el uso de algoritmos débiles.  
   [Más información](https://owasp.org/www-project-top-ten/)

3. **Injection**  
   Vulnerabilidades donde comandos maliciosos son enviados al sistema a través de entradas de usuario, como SQL, NoSQL o LDAP.  
   [Más información](https://owasp.org/www-project-top-ten/)

4. **Insecure Design**  
   Problemas inherentes al diseño de software que no considera principios de seguridad desde el inicio.  
   [Más información](https://owasp.org/www-project-top-ten/)

5. **Security Misconfiguration**  
   Configuraciones inseguras o por defecto que exponen el sistema a ataques.  
   [Más información](https://owasp.org/www-project-top-ten/)

6. **Vulnerable and Outdated Components**  
   Uso de librerías, frameworks o software desactualizados que contienen vulnerabilidades conocidas.  
   [Más información](https://owasp.org/www-project-top-ten/)

7. **Identification and Authentication Failures**  
   Fallos en los mecanismos de autenticación y gestión de identidades que permiten accesos no autorizados.  
   [Más información](https://owasp.org/www-project-top-ten/)

8. **Software and Data Integrity Failures**  
   Ausencia de mecanismos para verificar la integridad del software y los datos, abriendo puertas a ataques como deserialización insegura.  
   [Más información](https://owasp.org/www-project-top-ten/)

9. **Security Logging and Monitoring Failures**  
   Insuficiente registro y monitoreo de eventos de seguridad, dificultando la detección y respuesta a incidentes.  
   [Más información](https://owasp.org/www-project-top-ten/)

10. **Server-Side Request Forgery (SSRF)**  
    Vulnerabilidad que permite a un atacante manipular solicitudes realizadas por el servidor hacia otros recursos.  
    [Más información](https://owasp.org/www-project-top-ten/)

## Tipología de controles de seguridad

### Controles de acuerdo a su aplicabilidad

1. **Administrativos**:  
   Involucran políticas, procedimientos y regulaciones que establecen cómo las personas deben actuar para garantizar la seguridad de la información.
   - Ejemplos: políticas de seguridad, formación de empleados, gestión de riesgos.

2. **Lógicos** (o técnicos):  
   Son controles implementados a través de software y hardware para proteger los sistemas y datos.
   - Ejemplos: firewalls, antivirus, sistemas de detección de intrusos (IDS), encriptación.

3. **Físicos**:  
   Diseñados para proteger los entornos físicos donde se encuentran los sistemas y datos.
   - Ejemplos: cerraduras, cámaras de seguridad, controles de acceso biométrico.

### Controles de acuerdo a su funcionalidad

1. **Preventivos**:  
   Se implementan para evitar que ocurra un incidente de seguridad.
   - Ejemplos: autenticación de usuarios, políticas de acceso, cifrado de datos.

2. **Detectivos**:  
   Ayudan a identificar cuándo ha ocurrido un incidente de seguridad.
   - Ejemplos: sistemas de monitoreo, auditorías de seguridad, registros de eventos.

3. **De recuperación**:  
   Se utilizan para restablecer operaciones normales después de un incidente.
   - Ejemplos: planes de recuperación ante desastres, copias de seguridad, herramientas de restauración de sistemas.

---

### Políticas básicas para una organización

1. **Gestión de activos**:  
   Inventario y clasificación de los activos de la organización para garantizar su protección.

2. **Clasificación de la información**:  
   Establecimiento de niveles de sensibilidad (confidencial, restringido, público) para proteger adecuadamente los datos.

3. **Uso aceptable**:  
   Definición de cómo se deben utilizar los recursos de TI (equipos, redes, software) de manera segura y ética.

4. **Control de acceso**:  
   Restricción de acceso a información y sistemas únicamente a usuarios autorizados.

5. **Seguridad para proveedores**:  
   Evaluación y gestión de los riesgos relacionados con terceros o socios que interactúan con los sistemas de la organización.

6. **Continuidad de negocio**:  
   Planificación para mantener operaciones críticas ante eventos disruptivos.

7. **Gestión de incidentes**:  
   Definición de procesos para identificar, responder y aprender de incidentes de seguridad.

8. **BYOD (Bring Your Own Device)**:  
   Regulación del uso de dispositivos personales en la red corporativa para minimizar riesgos.

9. **Dispositivos móviles y teletrabajo**:  
   Políticas específicas para garantizar la seguridad de los equipos y datos en entornos de trabajo remoto.

10. **Gestión de claves y certificados**:  
    Administración segura de claves criptográficas y certificados digitales.

11. **Eliminación y destrucción de la información**:  
    Procedimientos seguros para deshacerse de datos confidenciales, como borrado seguro o trituración física.

12. **Escritorios y pantallas limpios**:  
    Políticas para evitar la exposición de información confidencial en áreas de trabajo.

13. **Gestión del cambio**:  
    Procesos para garantizar que los cambios en sistemas o software no introduzcan riesgos de seguridad.

14. **Copias de seguridad**:  
    Planificación y ejecución de backups periódicos para asegurar la disponibilidad y recuperación de datos.

15. **Transferencia de información**:  
    Establecimiento de protocolos seguros para el intercambio de datos dentro y fuera de la organización.

---

### Recomendaciones adicionales

### Formación y concienciación

- Capacitar regularmente a los empleados sobre buenas prácticas de ciberseguridad y riesgos actuales.

### Evaluaciones de riesgos

- Realizar análisis periódicos para identificar vulnerabilidades y amenazas emergentes.

### Actualizaciones y parches

- Mantener todos los sistemas, aplicaciones y dispositivos actualizados con los últimos parches de seguridad.

### Monitoreo continuo

- Implementar herramientas y procesos para detectar actividades sospechosas en tiempo real.

### Cumplimiento normativo

- Asegurarse de cumplir con las normativas legales aplicables, como GDPR, ISO 27001, o PCI DSS, según corresponda.

### Gestión de identidades

- Implementar mecanismos de autenticación robustos, como autenticación multifactor (MFA).

### Cifrado

- Utilizar cifrado para proteger datos en tránsito y en reposo.

### Respuesta a incidentes

- Establecer un equipo y plan de respuesta ante incidentes para actuar con rapidez y minimizar impactos.

Con estas prácticas, una organización puede construir una estrategia sólida para proteger sus activos y datos frente a amenazas cibernéticas

## Principios del Gobierno de la Seguridad de la Información

![alt text](<Captura de pantalla 2024-12-02 a las 16.31.52.png>)

El objetivo principal de la seguridad de la información es desarrollar, implementar y administrar un programa que cumpla con los siguientes resultados clave:

1. **Alineación estratégica**:  
   Garantizar que la seguridad de la información esté alineada con los objetivos estratégicos del negocio.

2. **Gestión de riesgos**:  
   Implementar medidas efectivas para mitigar riesgos y minimizar el impacto en los activos de información.

3. **Entrega de valor**:  
   Maximizar el retorno de las inversiones en seguridad de la información.

4. **Gestión de recursos**:  
   Optimizar el uso del conocimiento y la infraestructura relacionados con la seguridad para lograr eficiencia y eficacia.

5. **Medición del desempeño**:  
   Monitorizar y reportar métricas relevantes para evaluar y garantizar el cumplimiento de los objetivos de seguridad.

Estos principios aseguran un gobierno eficaz de la seguridad de la información, contribuyendo al fortalecimiento de la resiliencia organizacional.

## Controles Físicos en Seguridad de la Información

### Áreas y espacios seguros

1. **Perímetro de seguridad física**: Controles para prevenir accesos no autorizados, como muros, vallas, cerraduras y alarmas.  
2. **Áreas atendidas**: Recepción controlada para restringir el acceso solo al personal autorizado.  
3. **Barreras**: Protección física contra accesos no autorizados y amenazas ambientales.  
4. **Sistemas anti-incendios**: Protección contra incendios cumpliendo normativas legales.  
5. **Detección de intrusión**: Uso de sistemas como alarmas para detectar accesos no autorizados.  
6. **Segmentación de espacios**: Separación de áreas gestionadas por personal externo de las internas.  
7. **Controles de acceso físico**: Sistemas como tarjetas, cámaras, tornos y registro de accesos para limitar entradas.  
8. **Seguridad en oficinas e instalaciones**: Diseño para minimizar la exposición de información sensible a visitantes o accesos públicos.  
9. **Protección contra amenazas externas y ambientales**: Preparación frente a desastres naturales, accidentes y ataques maliciosos.  
10. **Trabajo en áreas seguras**: Supervisión y restricción de acceso bajo el criterio de necesidad de saber.  
11. **Áreas de entrega y carga**: Controles en puntos de carga para prevenir riesgos, como horarios definidos y revisión de mercancías.

### Protección del equipamiento

1. **Ubicación y protección**: Colocar equipos para minimizar riesgos de accesos no autorizados y protegerlos contra daños (fuego, agua, polvo, etc.).  
2. **Condiciones ambientales**: Monitorizar temperatura y humedad para evitar daños al equipamiento.  
3. **Protección eléctrica**: Uso de filtros y sistemas contra sobrecargas eléctricas y fugas electromagnéticas.  
4. **Elementos de soporte**: Garantizar el suministro eléctrico y comunicaciones estables.  
5. **Seguridad en el cableado**: Protección contra intercepción y daño mediante enterramiento, blindajes y accesos controlados.  
6. **Mantenimiento**: Seguir especificaciones de fabricantes para garantizar la vida útil y el correcto funcionamiento.  
7. **Retirada de bienes**: Registro y autorización para retirar equipamiento, software o información.  
8. **Equipamiento fuera de las instalaciones**: Proteger activos contra riesgos externos con candados y controles adicionales.  
9. **Reutilización o eliminación de equipos**: Asegurar la eliminación segura de datos antes de reutilización o destrucción de equipos.  
10. **Equipamiento desatendido**: Bloqueo de equipos y cierre de sesiones cuando no estén en uso.

## Gestión de Activos en Seguridad de la Información

### Objetivo General

Preservar los activos de información como soporte del negocio, implementando medidas específicas para garantizar su protección y correcta gestión.

### Responsabilidad de los Activos

1. **Inventario de activos**:  
   - Identificar y mantener un registro actualizado de los activos y su importancia.  
   - Incluir propietario y clasificación en el inventario.  
2. **Propiedad de los activos**:  
   - Asignar propietarios responsables del ciclo de vida del activo (inventario, protección y destrucción).  
3. **Uso aceptable de los activos**:  
   - Definir reglas de uso correcto de información y activos asociados.  
4. **Devolución de activos**:  
   - Formalizar procesos para garantizar la devolución de activos al finalizar contratos o empleos, asegurando la eliminación de información en medios propios.

### Clasificación de la Información

1. **Clasificación de la información**:  
   - Clasificar información en función de su valor, criticidad y sensibilidad.  
   - Alinear el esquema de clasificación con los controles de acceso.  
2. **Etiquetado de la información**:  
   - Implementar procedimientos para etiquetar información en formatos digitales y físicos según el esquema de clasificación.  
3. **Gestión de activos clasificados**:  
   - Restringir accesos, registrar receptores autorizados y proteger copias temporales y permanentes.  

### Manejo de los Soportes de Almacenamiento

1. **Gestión de soportes extraíbles**:  
   - Limitar el uso de soportes extraíbles, cifrar información sensible y mantener registros de altas/bajas.  
   - Implementar controles de transferencia y copias de seguridad.  
2. **Eliminación de soportes**:  
   - Desarrollar procesos seguros para eliminar datos sensibles de dispositivos obsoletos.  
   - Controlar y registrar la eliminación segura, incluso si la realiza una empresa externa.  
3. **Traslado de soportes físicos**:  
   - Proteger la información durante traslados, registrando la salida, controlando transportistas, cifrando datos y asegurando embalajes adecuados.  

Este enfoque integral asegura una gestión adecuada de los activos y minimiza riesgos para la información sensible.

## Gestión de Activos

### Objetivo

Preservar los activos de información como soporte del negocio, aplicando controles para su identificación, clasificación y protección frente a riesgos.

### Principales Áreas

#### 1. **Responsabilidad de los Activos**

- **Inventario de activos**:  
  Identificar y registrar todos los activos de información, asignando un propietario y nivel de clasificación.  
- **Propiedad de los activos**:  
  Asignar responsables que gestionen los activos durante su ciclo de vida, asegurando su correcta protección, clasificación y eliminación.  
- **Uso aceptable de los activos**:  
  Definir y documentar reglas para el uso adecuado de los activos y las instalaciones de procesamiento.  
- **Devolución de activos**:  
  Garantizar la devolución de activos al finalizar contratos o empleos, eliminando información en medios propios si es necesario.  

#### 2. **Clasificación de la Información**

- **Clasificación**:  
  Evaluar la criticidad, valor y sensibilidad de la información para asignarle el nivel de protección adecuado, en alineación con los controles de acceso.  
- **Etiquetado**:  
  Implementar procedimientos claros para etiquetar la información según su clasificación, tanto en papel como en formato digital.  
- **Manejo**:  
  Aplicar medidas de gestión según el nivel de clasificación, como restringir accesos, registrar receptores autorizados y proteger copias de la información.  

#### 3. **Manejo de los Soportes de Almacenamiento**

- **Gestión de soportes extraíbles**:  
  - Limitar el uso y requerir autorización.  
  - Cifrar datos sensibles y mantener registros de altas y bajas.  
  - Renovar dispositivos periódicamente para evitar degradación de datos.  
  - Asegurar copias de seguridad en soportes independientes.  
- **Eliminación de soportes**:  
  - Establecer procesos para eliminar datos de forma segura e irrecuperable.  
  - Registrar los dispositivos eliminados y controlar a terceros que realicen esta tarea.  
- **Traslado de soportes físicos**:  
  - Implementar controles en los traslados, como registro de salida, identificación de transportistas, cifrado de datos y embalajes adecuados.  

Este enfoque asegura la protección integral de los activos de información y su correcta gestión durante todo el ciclo de vida.

## Gestión de la Identidad y el Control de Acceso

### Objetivo general

Administrar cuentas de usuario, derechos de acceso y recursos de red para garantizar la seguridad y eficiencia en el acceso a sistemas, datos y aplicaciones.

---

### Conceptos Clave

#### 1. **Gestión de Accesos**

- Configura niveles de acceso para usuarios y grupos, asegurando auditorías y ajustes periódicos para adaptarse a cambios organizativos.

#### 2. **Principios de Seguridad**

- **Necesidad de saber**: Acceso limitado únicamente a lo necesario para realizar las tareas laborales.  
- **Menor privilegio**: Otorgar solo los privilegios mínimos indispensables para cada usuario.  

#### 3. **Aprovisionamiento y Desaprovisionamiento**

- **Aprovisionamiento**: Crear identidades y asignar permisos.  
- **Desaprovisionamiento**: Eliminar accesos y permisos al finalizar relaciones laborales o modificar roles.  

#### 4. **Identificación, Autenticación y Autorización**

- **Identificación**: Declarar la identidad del usuario.  
- **Autenticación**: Verificar que la identidad es legítima mediante contraseñas, biometría, o dispositivos.  
- **Autorización**: Validar qué recursos puede acceder según los permisos asignados.  

#### 5. **Factores de Autenticación (2FA/MFA)**

- **Algo que sabes**: Contraseña o PIN.  
- **Algo que eres**: Biometría (huella digital, iris, voz).  
- **Algo que tienes**: Smartcard, token o dispositivo móvil.  
- Recomendado: Usar múltiples factores (2FA/MFA) en sistemas críticos.  

#### 6. **Acceso Basado en Roles (RBAC)**

- **Modelo RBAC**: Permisos se asignan según roles laborales.  
- **Modelos alternativos**:  
  - **DAC**: Control discrecional basado en permisos personalizados.  
  - **MAC**: Control obligatorio basado en etiquetas y políticas predefinidas, usado en entornos gubernamentales.  

#### 7. **Flujos de Trabajo para Altas, Bajas y Modificaciones**

- Definir procesos claros para aprovisionar, desaprovisionar o modificar accesos, especificando roles, aprobaciones y auditorías.

#### 8. **Inicio de Sesión Único (SSO)**

- Permite que el usuario se autentique una vez y acceda a múltiples sistemas sin volver a ingresar credenciales durante un periodo definido.

#### 9. **Contraseña de un Solo Uso (OTP)**

- Contraseñas válidas únicamente una vez, generadas aleatoriamente o enviadas al dispositivo del usuario, por ejemplo, vía SMS o aplicación.

---

### Beneficios

- Mayor seguridad al limitar accesos no autorizados.  
- Simplificación en la gestión de usuarios.  
- Reducción del riesgo mediante autenticación robusta y controles basados en roles.

## Requisitos del Negocio para el Control de Accesos

### 1. **Política de Control de Acceso**

Una política de control de acceso debe ser establecida, documentada y revisada regularmente para reflejar los requisitos de seguridad del negocio y la información. Esta política debe garantizar que las reglas, derechos y restricciones de acceso estén alineadas con los riesgos de seguridad asociados. La política debe abordar tanto controles **físicos** como **lógicos** y considerar los siguientes aspectos:

#### Elementos Clave

- **Requisitos de seguridad para aplicaciones de negocio:** Determinar controles específicos para cada sistema y aplicación según su criticidad.
- **Políticas de diseminación de información:** Incluir los principios de "necesidad de saber" y clasificación de la información.
- **Consistencia en derechos de acceso:** Asegurar que los permisos coincidan con las políticas de clasificación de información en sistemas y redes.
- **Cumplimiento normativo:** Incluir requisitos legales y contractuales para la limitación de acceso a datos y servicios.
- **Gestión de acceso en entornos distribuidos:** Considerar conexiones y usuarios en redes complejas.
- **Segregación de roles:** Separar claramente las funciones de solicitud, autorización y administración de accesos.
- **Autorización formal:** Definir requisitos para aprobar solicitudes de acceso.
- **Revisión periódica:** Establecer procesos para revisar regularmente los derechos de acceso asignados.
- **Eliminación de derechos:** Asegurar la eliminación inmediata de accesos al finalizar la relación laboral o cambiar roles.
- **Archivado de registros:** Mantener un registro detallado de eventos relacionados con el uso, gestión y autenticación de identidades de usuario.
- **Gestión de roles privilegiados:** Implementar controles estrictos para usuarios con accesos avanzados.

---

### 2. **Acceso a Redes y Servicios de Red**

Específicamente, la gestión del acceso a redes debe garantizar que solo usuarios autorizados accedan a los recursos de red y servicios asociados. Para ello, se requiere una política específica que contemple:

#### Contenido de la Política

- **Redes y servicios accesibles:** Identificar a qué redes y servicios tienen acceso los usuarios.
- **Procedimientos de autorización:** Definir cómo se solicita, evalúa y otorga el acceso.
- **Controles asociados:** Implementar medidas para garantizar que las autorizaciones son seguras y consistentes con las políticas.
- **Medios de acceso permitidos:** Especificar métodos aceptados como VPN y prohibir accesos no autorizados (e.g., redes WiFi no seguras).
- **Requisitos de autenticación:** Establecer requisitos mínimos de autenticación, como el uso de MFA para redes críticas.
- **Supervisión del uso:** Implementar sistemas de monitoreo para registrar, analizar y controlar el acceso a los servicios de red.

---

### Beneficios de Implementar una Política Robusta

1. **Seguridad Mejorada:** Reducción de riesgos mediante controles estrictos y revisiones periódicas.  
2. **Cumplimiento Normativo:** Cumple con obligaciones legales y contractuales.  
3. **Eficiencia Operativa:** Proporciona claridad en roles y responsabilidades.  
4. **Trazabilidad y Auditoría:** Archivos detallados para eventos relacionados con accesos y autentificaciones.  

## Gestión del Acceso de Usuarios

## 1. **Registro de Usuarios y Cancelación del Registro**

Es fundamental establecer controles claros para el alta y baja de usuarios, incluyendo:

### Altas

- **Registro de IDs o cuentas de usuario:** Cada usuario debe tener un identificador único que no sea reutilizable.
- **Vinculación del ID al usuario:** Asociar el ID con el individuo para facilitar el seguimiento y auditoría.

### Bajas

- **Desactivación inmediata:** Los IDs deben desactivarse automáticamente o de forma inmediata cuando el usuario abandone la organización.
- **Eliminación periódica de usuarios redundantes:** Los IDs no utilizados deben eliminarse regularmente.
- **Gestión de permisos:** Revocar tanto el ID como los permisos asociados al finalizar el contrato.

---

## 2. **Gestión de Accesos de Usuarios**

Debe implementarse un proceso formal para asignar, modificar y revocar accesos. Este proceso incluye:

- **Aprobación formal:** La autorización debe ser aprobada por el propietario del sistema o servicio.
- **Cumplimiento de políticas:** Verificar que el acceso solicitado cumple con las políticas de la organización.
- **Ejecución tras autorización:** No otorgar accesos hasta completar todo el proceso de aprobación.
- **Registro de accesos:** Mantener un registro detallado de los permisos otorgados.
- **Revisión periódica:** Evaluar periódicamente los accesos concedidos y realizar ajustes necesarios.

---

## 3. **Gestión de Derechos de Acceso Privilegiados**

El acceso privilegiado requiere controles independientes y estrictos:

- **Políticas definidas:** Seguir las políticas específicas para accesos privilegiados.
- **Identificación clara:** Identificar usuarios con accesos privilegiados en cada sistema.
- **Principio de menor privilegio:** Asignar solo los permisos necesarios para realizar sus funciones.
- **Caducidad:** Establecer fechas límite para permisos especiales.
- **Cuentas separadas:** Utilizar cuentas específicas para tareas privilegiadas, distintas de las normales.
- **Prevención de usos no autorizados:** Implementar procedimientos para evitar el abuso de privilegios.
- **Mantenimiento de contraseñas:** Forzar cambios de contraseñas cuando un usuario privilegiado abandone o cambie de puesto.

---

## 4. **Gestión de la Información de Autenticación Secreta**

La confidencialidad de los mecanismos de autenticación (contraseñas, tokens, etc.) debe garantizarse mediante:

- **Contratos y políticas:** Incluir cláusulas que obliguen al mantenimiento del secreto.
- **Cambios iniciales:** Obligar a los usuarios a cambiar las contraseñas entregadas inicialmente.
- **Validación de identidad:** Confirmar la identidad antes de entregar contraseñas y registrar su entrega.
- **Contraseñas seguras:** Prohibir contraseñas compartidas o débiles y fomentar el uso de métodos seguros de comunicación.
- **Actualización tras finalización:** Cambiar contraseñas utilizadas por personal externo tras la finalización de sus actividades.

---

## 5. **Revisión de Derechos de Acceso de Usuarios**

Establecer controles para revisar periódicamente los accesos de los usuarios:

- **Accesos tras cambios organizativos:** Ajustar accesos por cambios de puesto, promociones o finalización de empleo.
- **Accesos con privilegios:** Revisar regularmente cuentas privilegiadas y documentar los cambios realizados.
- **Eliminación de accesos no necesarios:** Garantizar la eliminación o modificación oportuna de accesos según corresponda.

---

## 6. **Responsabilidades del Usuario**

Establecer normas claras sobre el uso adecuado de contraseñas y otros datos de autenticación:

### Políticas de Contraseñas

- **Confidencialidad:** Prohibir la divulgación de contraseñas y el almacenamiento inseguro (en papel, archivos, etc.).
- **Amenazas:** Definir políticas para cambiar contraseñas ante riesgos de seguridad.
- **Requisitos de calidad:**
  - **Longitud:** Mínimo de 10 caracteres.
  - **Complejidad:** Incluir al menos tres de los cuatro grupos: minúsculas, mayúsculas, números y caracteres especiales.
  - **Validez temporal:** Cambiar contraseñas regularmente.
- **Evitar uso compartido:** Una contraseña no debe usarse para múltiples aplicaciones o usuarios.

### Obligaciones del Usuario

- **Cambios iniciales:** Forzar cambios en las contraseñas iniciales asignadas.
- **Manejo responsable:** Prohibir compartir contraseñas y promover su gestión segura.

## Control de Acceso a Sistemas y Aplicaciones

## 1. **Restricción de Acceso a la Información**

Para garantizar el acceso controlado a las funciones y datos de sistemas y aplicaciones, se debe implementar:

- **Uso de menús:** Restringir el acceso a funciones según el rol del usuario.
- **Ocultación de funciones administrativas:** Limitar estas opciones únicamente a usuarios autorizados.
- **Restricción de acceso a datos:** Determinar qué información es accesible para cada usuario o ID.
- **Control de permisos:** Restringir acciones específicas como lectura, escritura, eliminación o ejecución según el rol.
- **Limitación de información de salida:** Controlar qué datos se pueden visualizar o exportar.
- **Accesos físicos o lógicos adicionales:** Considerar protecciones adicionales para sistemas e información clasificada.

---

## 2. **Procedimientos de Conexión Seguros (Log-on)**

El acceso a sistemas debe estar protegido mediante procedimientos seguros que consideren:

- **Nivel de autenticación:** Adoptar métodos como usuario y contraseña, autenticación de doble factor (ej., smartcard o aplicación móvil), según la criticidad del sistema.
- **Información mínima:** Mostrar solo la información necesaria para evitar ofrecer detalles útiles a atacantes no autorizados.
- **Buenas prácticas:**
  - No mostrar IDs hasta completar la autenticación.
  - Evitar mensajes de ayuda que revelen detalles técnicos.
  - Validar datos únicamente cuando estén completos.
  - Proteger contra ataques de fuerza bruta (p.ej., límites en intentos de acceso).
  - Registrar intentos de acceso exitosos y fallidos.
  - Cifrar contraseñas y evitar que se muestren en texto claro.

---

## 3. **Sistema de Gestión de Contraseñas**

Los sistemas deben incluir un gestor de contraseñas para automatizar la seguridad y calidad de las mismas. Este sistema debe:

- **Reforzar el uso de IDs individuales:** Asegurar la responsabilidad individual (accountability).
- **Permitir cambios de contraseñas:** Habilitar a los usuarios para crear y modificar sus contraseñas, con confirmación para corregir errores.
- **Garantizar contraseñas seguras:** Filtrar contraseñas que no cumplan requisitos de calidad.
- **Forzar cambios iniciales:** Obligar al cambio de contraseñas entregadas por primera vez.
- **Cambios periódicos:** Implementar rotación regular o ante sospechas de intrusión.
- **Evitar el reuso:** Registrar las contraseñas utilizadas y evitar su reutilización.
- **Protección en pantalla:** No mostrar contraseñas mientras se introducen.
- **Almacenamiento y transmisión seguras:** Cifrar contraseñas tanto en almacenamiento como en tránsito.

---

## 4. **Uso de Programas de Utilidad Privilegiados**

Los programas con capacidades especiales deben gestionarse de manera estricta:

- **Autenticación separada:** Exigir autenticación adicional para su uso.
- **Segregación:** Mantener los programas de utilidad separados de las aplicaciones regulares.
- **Registro de actividades:** Documentar todas las acciones realizadas con estos programas.
- **Control de funciones:** Limitar privilegios al mínimo necesario para cumplir con las tareas asignadas.

---

## 5. **Control de Acceso al Código Fuente de Programas**

El acceso al código fuente debe estar controlado rigurosamente para evitar cambios no autorizados o el uso indebido:

- **Restricción de accesos:** Limitar acceso a usuarios específicamente autorizados.
- **Seguridad de documentación asociada:** Proteger también los diseños, diagramas y especificaciones relacionadas.
- **Registro de accesos y cambios:** Documentar todas las acciones sobre el código fuente.

---

## 6. **Protección de Datos**

La protección de datos requiere salvaguardar la información en todas las fases de su ciclo de vida:

### Ciclo de Vida de los Datos

![alt text](<Captura de pantalla 2024-12-02 a las 16.48.07.png>)

1. **Creación y Recolección:** La información es generada o modificada.
2. **Almacenamiento:** La información se guarda en reposo hasta ser utilizada.
3. **Uso:** La información es accedida sin ser modificada.
4. **Compartición:** La información se pone a disposición de otros (email, permisos, etc.).
5. **Archivado:** Se almacena a largo plazo para cumplir requisitos legales o internos.
6. **Eliminación:** Se destruye de forma definitiva cuando ya no es necesaria.

### Consideraciones Clave

- Proteger la información en reposo y en tránsito mediante cifrado.
- Controlar el acceso en cada fase del ciclo.
- Registrar las acciones relacionadas con los datos para garantizar trazabilidad.

## Copias de Seguridad y Protección de Datos

## 1. **Copias de Seguridad**

Las copias de seguridad son fundamentales para la recuperación de datos en caso de fallos. Su implementación debe considerar los siguientes aspectos:

### Factores para Determinar la Frecuencia

1. **Volumen de datos:** Cantidad de datos generados o modificados.
2. **Impacto en el negocio:** Consecuencias de la pérdida de datos por hora, día, semana, etc.
3. **Costes:** Almacenamiento necesario para soportar las copias.
4. **Obligaciones legales y normativas:** Período mínimo de conservación exigido.

### Período de Retención

Decidir cuánto tiempo conservar las copias de seguridad según requisitos legales y necesidades operativas.

### Tipos de Copias de Seguridad

1. **Completa:** Duplica todos los datos; lenta pero con restauración más rápida y sencilla.
2. **Incremental:** Copia solo los cambios desde la última copia (menor almacenamiento, restauración más lenta).
3. **Diferencial:** Copia cambios desde la copia completa inicial (balance entre velocidad y espacio).
4. **Espejo (mirroring):** Replica datos en tiempo real. Ideal para sistemas con alta disponibilidad, aunque elimina datos si se borran en la fuente.

### Procedimientos y Pruebas

- Probar regularmente los procedimientos de recuperación.
- Verificar la integridad de las copias.
- Implementar controles para evitar errores críticos en la restauración.

### Ubicación y Seguridad

- Proteger las copias con los mismos controles que los datos originales.
- Mantener copias en ubicaciones geográficamente alejadas o en la nube para protegerse ante desastres.

---

## 2. **Data Loss Prevention (DLP)**

El DLP previene fugas de información mediante la monitorización y bloqueo de datos sensibles. Sus funciones principales son:

- **Monitoreo de datos:**
  - En uso: Acciones realizadas en dispositivos.
  - En tránsito: Tráfico de red.
  - En reposo: Archivos almacenados.
- **Etiquetado:** Usa metadatos para definir permisos (lectura, edición, bloqueo de copias o envíos externos).
- **Cifrado en dispositivos externos:** Protege datos en discos USB para su uso solo en equipos autorizados.
- **Políticas personalizadas:** Define restricciones por roles, departamentos o contenido específico.

---

## 3. **Information Rights Management (IRM)**

El IRM protege documentos con cifrado persistente. Sus principales características incluyen:

- **Control granular:** Define permisos como lectura, edición, compartición o impresión.
- **Revisión en tiempo real:** Cambia o revoca permisos en cualquier momento.
- **Protección en reposo:** Cifra documentos y controla accesos según roles o usuarios.
- **Acceso remoto seguro:** Requiere cliente que valida permisos con el servidor cada vez que se accede al archivo.

---

## 4. **Seguridad en el Correo Electrónico**

El correo es clave para la compartición de información y es uno de los vectores de ataque más comunes. Las soluciones de seguridad deben ofrecer:

1. **Detección de phishing:** Identificación de correos maliciosos mediante IA y análisis de origen.
2. **Validación de enlaces y adjuntos:** Uso de sandboxes para analizar comportamiento.
3. **Análisis de malware:** Evaluación de adjuntos mediante firmas y patrones de IA.
4. **Control de información saliente:** Prevención de fugas mediante análisis y cifrado.
5. **Detección de fraudes:** Identificación de actividades sospechosas como el fraude del CEO.
6. **Protección contra exploits:** Detección de intentos de explotación de sistemas.

---

## 5. **Monitorización de Actividad en BBDD y Almacenes de Datos**

La monitorización de bases de datos y repositorios es esencial para identificar comportamientos anómalos. Una solución eficaz debe incluir:

- **Descubrimiento y clasificación:** Automatizar la identificación de datos sensibles.
- **Monitoreo en tiempo real:** Analizar y auditar actividades de usuarios en sistemas SQL, no-SQL, y data warehouses.
- **Cumplimiento legal:** Plantillas predefinidas para cumplir normas como RGPD, HIPAA, PCI-DSS, etc.
- **Bloqueo y enmascaramiento:** Actuar como un cortafuegos de aplicación para bloquear acciones específicas o enmascarar datos sensibles.

## Seguridad en Punto Final

La protección del punto final se enfoca en salvaguardar la infraestructura final (sistemas, servidores y aplicaciones desplegadas) para prevenir accesos no autorizados a la información o el uso como punto de salto a otros sistemas críticos.

### Protección contra Malware

Los antivirus han evolucionado para detectar y eliminar diversos tipos de malware (virus, spyware, troyanos, etc.). Los métodos antiguos basados en firmas ya no son efectivos debido a su vulnerabilidad a modificaciones. Los sistemas modernos utilizan aprendizaje automático y profundo para identificar características comunes de malware, incluso aquellos no detectados previamente. Otros enfoques se centran en evitar que el malware tome control del sistema o realice acciones como cifrado de discos.

### Detección de Intrusiones (EDR)

Las soluciones EDR supervisan en tiempo real los sistemas y aplicaciones para detectar posibles ataques. Ofrecen visibilidad completa de la actividad de los endpoints, con capacidades para detectar amenazas nuevas, realizar análisis forenses y responder a incidentes rápidamente. EDR permite un enfoque preventivo, detectivo y reactivo en la protección de sistemas.

### Configuración Segura (Hardening)

El hardening es el proceso de configurar sistemas y aplicaciones para reducir su superficie de ataque, mejorando la seguridad general. Implica acciones como cerrar puertos innecesarios, asegurar contraseñas, habilitar cifrado y establecer políticas de acceso seguras.

### Gestión de Vulnerabilidades

La gestión de vulnerabilidades es el proceso de identificar, evaluar y corregir debilidades en sistemas y aplicaciones. Involucra el uso de escáneres, pruebas de penetración y categorización de activos para priorizar las vulnerabilidades críticas. La corrección debe realizarse con procedimientos adecuados para evitar impactos negativos en los sistemas.

### Otras Tecnologías de Protección de Punto Final

- **FIM (File Integrity Monitoring):** Detecta modificaciones no autorizadas en archivos críticos del sistema mediante algoritmos de hash y comparaciones con una base de referencia.
  
- **Protección de Móviles:** Protege smartphones con antivirus, cifrado de datos, correo seguro y gestión remota en caso de pérdida o robo.
  
- **Tecnología de Señuelos:** Crea sistemas falsos en la red para detectar ataques y analizar los comportamientos del adversario.
  
- **Seguridad en la Virtualización:** Protege tanto las máquinas físicas como las virtuales o basadas en contenedores, considerando aspectos específicos de las redes virtuales.
  
- **RASP (Runtime Application Self-Protection):** Protege aplicaciones web en tiempo de ejecución, defendiendo sus puntos vulnerables de ataques dirigidos a estos puntos.

## Seguridad en Red

Los sistemas, aplicaciones e información de las organizaciones están desplegados en redes informáticas, lo que facilita la comunicación interna y externa, pero también expone a nuevas amenazas que deben ser controladas.

### Segmentación y Microsegmentación de la Red

La segmentación de la red divide una gran red (como intranet o extranet) en segmentos más pequeños, filtrando las entradas y salidas de tráfico en cada uno de ellos para minimizar la exposición de los sistemas internos. Esta segmentación puede ser física o lógica, mediante VLANs o redes virtuales. La microsegmentación lleva este concepto más lejos, segmentando a nivel de servicios dentro de una misma aplicación, utilizando tecnologías como Red Definida por Software (SDN) para una gestión dinámica y escalable.

### IDS/IPS (Detección/Prevención de Intrusiones en Red)

Un sistema IDS detecta accesos no autorizados analizando el tráfico de red y comparándolo con firmas de ataques conocidos o comportamientos sospechosos. Un IPS añade capacidad para bloquear el tráfico según reglas predefinidas. El análisis de tráfico cifrado puede realizarse utilizando técnicas de "man in the middle" o cargando certificados de cifrado. Los sistemas modernos utilizan aprendizaje automático y profundo para detectar anomalías y características comunes de ataques.

### Cortafuegos (FWs)

Un cortafuegos (FW) bloquea el acceso no autorizado y permite comunicaciones autorizadas basadas en reglas definidas. Los tipos de cortafuegos incluyen:

- **A nivel de paquetes:** Reglas basadas en paquetes de datos
- **Con estado:** Controla sesiones y permite solo los paquetes que pertenecen a una sesión establecida.
- **A nivel de aplicación:** Actúa sobre protocolos específicos como FTP, DNS o HTTP, y protege aplicaciones web de ataques como SQL Injection o XSS.
- **Con inspección profunda de paquetes (DPI):** Combina un IDS/IPS y un cortafuegos para detectar ataques más complejos.
- **Software Defined Firewall (SDFW):** Cortafuegos definidos a través de reglas de control del SDN.

### SIEM (Security Information and Events Management)

El SIEM recoge eventos de seguridad desde diversas fuentes (SO, FWs, IDS, AV, etc.) en un formato unificado para correlacionarlos y analizar posibles intrusiones. Sus principales funcionalidades incluyen:

- **Reglas de alerta y correlación:** Para notificar posibles intrusiones y asegurar el cumplimiento normativo
- **Análisis avanzado:** Permite a los gestores de incidentes investigar eventos de seguridad.
- **Análisis de comportamiento de usuarios y entidades (UEBA):** Establece patrones de comportamiento para alertar sobre anomalías.
- **Análisis de comportamiento de la red (NBA):** Detecta anomalías en el tráfico de la red.
- **Orquestación y automatización (SOAR):** Permite la automatización de respuestas ante incidentes, como el aislamiento de sistemas o el bloqueo en un cortafuegos.

## Cumplimiento y Ciberseguridad

### Cumplimiento Legal (Legal Compliance)

El cumplimiento legal, o **legal compliance**, se refiere a la implementación de los requisitos y normas necesarias para garantizar que una organización cumple con el marco normativo aplicable. Aunque este concepto tiene más de cincuenta años, inicialmente centrado en el ámbito financiero, con el tiempo se ha expandido a todos los sectores empresariales y gubernamentales.

En España, el cumplimiento legal ha ganado importancia recientemente, especialmente en empresas con matrices internacionales. Aunque inicialmente más relevante para grandes empresas, las normativas cada vez más estrictas también afectan a medianas y pequeñas empresas.

A medida que los ataques cibernéticos aumentan, los marcos legales se han endurecido, con leyes cada vez más exigentes en materia de ciberseguridad. Esto ha llevado a que la protección de la seguridad de la información y otras infraestructuras críticas se convierta en una obligación para diversos sectores.

### Normativas y Leyes Clave en Ciberseguridad

#### 1. **Esquema Nacional de Seguridad (ENS)**

   En España, el ENS establece la política de seguridad para la utilización de medios electrónicos en la Administración Pública. Incluye principios básicos y requisitos mínimos para proteger la información y afecta a organizaciones privadas que prestan servicios a las administraciones públicas.

#### 2. **Ley de Protección de Infraestructuras Críticas (Ley PIC)**

   Esta ley define las infraestructuras críticas como aquellas cuya perturbación o destrucción tendría un grave impacto sobre los servicios esenciales de la sociedad, como la administración, energía, salud, transporte, entre otros. Su objetivo es identificar estas infraestructuras y diseñar medidas de prevención y protección tanto en el ámbito físico como en el de las tecnologías de la información.

#### 3. **Reglamento General de Protección de Datos (GDPR)**

   El GDPR es un reglamento europeo que regula la protección de datos personales y su libre circulación. Todas las empresas en la UE o aquellas que operen con datos personales de ciudadanos europeos deben cumplir con sus disposiciones. Incluye la necesidad de realizar un Análisis de Impacto de la Privacidad (PIA) para identificar y mitigar riesgos relacionados con la privacidad de los datos.

#### 4. **Directiva NIS**

   La Directiva NIS de la UE busca mejorar la seguridad de las redes y sistemas de información en su territorio. Establece, entre otras medidas, la creación de Equipos de Respuesta a Incidentes de Seguridad Informática (CSIRT) para mejorar la cooperación y respuesta ante incidentes de seguridad.

#### 5. **Sarbanes-Oxley (SOX)**

   Promulgada en Estados Unidos, esta ley busca prevenir fraudes y proteger a los inversores, especialmente en empresas cotizadas en bolsa. En el ámbito de la ciberseguridad, exige que la información financiera esté sujeta a controles adecuados y que estos sean auditados, introduciendo los principios de **cuidado debido** y **diligencia debida**

#### 6. **PCI DSS (Payment Card Industry Data Security Standard)**

   Aunque no es una ley, la norma PCI DSS fue creada por el consorcio de empresas de tarjetas de crédito (como VISA y Mastercard) para asegurar que las organizaciones que procesan, almacenan o transmiten datos de tarjetas de crédito mantengan medidas de seguridad adecuadas. Afecta a bancos, comercios electrónicos y procesadores de pagos, entre otros.

### Conclusión

El cumplimiento de las leyes y normativas de ciberseguridad es esencial para proteger tanto a las organizaciones como a sus clientes frente a riesgos cada vez mayores. Las leyes, como el GDPR o el Esquema Nacional de Seguridad, requieren que las organizaciones implementen medidas proactivas y de protección rigurosas para asegurar la confidencialidad, integridad y disponibilidad de los datos y sistemas críticos.

## PCI-DSS: Normativa de Seguridad para Datos de Tarjetas de Pago

PCI DSS (Payment Card Industry Data Security Standard) es la normativa internacional de seguridad aplicable a todas las entidades que almacenan, procesan o transmiten datos de titulares de tarjeta o datos sensibles de autentificación. Su objetivo es establecer un nivel básico de protección para los consumidores y reducir el fraude y las filtraciones de datos dentro del ecosistema de pagos. La normativa se aplica a cualquier organización que acepte o procese tarjetas de pago.

### Aspectos Clave del Cumplimiento PCI

El cumplimiento de PCI DSS implica tres aspectos fundamentales:

1. **Manejo de los datos de tarjetas de crédito** de los consumidores de manera segura, incluyendo su recolección y transmisión.
2. **Almacenamiento seguro de los datos**, cumpliendo con los 12 dominios de seguridad de PCI, como el cifrado y la vigilancia continua.
3. **Validación anual de los controles de seguridad**, que incluye cuestionarios, escaneos de vulnerabilidades y auditorías de terceros.

Más información sobre la norma en español: [PCI DSS v3.2.1 en Español](https://es.pcisecuritystandards.org/_onelink_/pcisecurity/en2es/minisite/en/docs/PCI_DSS_v3-2-1-ES-LA.PDF)

### Medidas de Seguridad Requeridas

#### 1. Desarrollar y Mantener una Red Segura

- **Cortafuegos:** Instalar y mantener una configuración de cortafuegos para proteger los datos de los titulares de tarjetas, con procesos documentados y revisión cada 6 meses.
- **Contraseñas Predeterminadas:** Evitar el uso de contraseñas del sistema y otros parámetros de seguridad proporcionados por los proveedores.

#### 2. Proteger los Datos de los Titulares de Tarjetas

- **Protección de Datos Almacenados:** Establecer procedimientos para almacenar el menor número posible de datos de tarjetas y aplicar medidas de protección (como enmascarado del PAN, cifrado y hash).
- **Cifrado de Datos:** Utilizar criptografía sólida para proteger los datos confidenciales durante su transmisión a través de redes públicas abiertas.

#### 3. Mantener un Programa de Gestión de Vulnerabilidades

- **Antivirus:** Implementar software antivirus en todos los sistemas del entorno CDE, asegurando que esté actualizado y documentado.
- **Desarrollo Seguro:** Implementar un proceso de desarrollo seguro, incluyendo revisiones de código y pruebas antes de la implementación en producción.

#### 4. Implementar Medidas de Control de Acceso

- **Acceso Restringido:** Controlar el acceso a los datos mediante políticas basadas en la necesidad de conocer y el principio de menor privilegio.
- **Identificación Única:** Asignar una identificación única a cada persona que tenga acceso al sistema, con contraseñas fuertes y políticas de acceso remoto utilizando MFA.
- **Acceso Físico:** Implementar controles físicos adecuados para restringir y supervisar el acceso a los sistemas que procesan datos de tarjetas.

#### 5. Monitorizar y Probar Regularmente las Redes

- **Auditoría:** Generar y mantener registros de acceso a los datos de tarjetas, con identificación de usuarios, tipo de evento y detalles asociados.
- **Pruebas de Seguridad:** Realizar escaneos de vulnerabilidades trimestrales y un test de penetración anual para identificar debilidades en los sistemas.

#### 6. Mantener una Política de Seguridad de la Información

- **Política de Seguridad:** Tener una política de seguridad publicada y revisada regularmente que defina las responsabilidades de todo el personal y contemple un programa formal de concienciación sobre seguridad.

### Resumiendo

Cumplir con PCI DSS es esencial para asegurar la protección de los datos de tarjetas de pago y mantener la confianza de los consumidores, reduciendo riesgos de fraude y filtraciones de información sensible. La implementación de medidas de seguridad rigurosas es clave para proteger tanto los datos de los titulares como la infraestructura de la organización.

## Dudas

- Qué es un XSS?
- a
