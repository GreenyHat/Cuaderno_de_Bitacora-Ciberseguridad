
## Vulnerabilidades:
### **1. Jenkins Exploitation (Groovy Script Console)**

Jenkins es una popular herramienta de integración continua que, si no está configurada correctamente, puede ser explotada por atacantes a través de su consola de scripts Groovy.

#### **Qué es Groovy Script Console:**

- Es una herramienta interna en Jenkins que permite a los administradores ejecutar scripts Groovy directamente en el servidor.
- Se utiliza para tareas administrativas avanzadas, como la configuración de trabajos o la gestión de usuarios.

#### **Vulnerabilidades Comunes:**

- **Autenticación débil o inexistente:** Si la consola Groovy está accesible sin autenticación, los atacantes pueden ejecutar comandos arbitrarios en el servidor.
- **Falta de control de acceso:** Usuarios con privilegios insuficientes pueden acceder y abusar de esta funcionalidad.

#### **Explotación:**

1. Acceso a la consola Groovy (`http://<jenkins-url>/script` o `/scriptConsole`)
2. Ejecución de comandos maliciosos, como: 
    
    groovy
    
    Copiar código
    
    `def proc = "cmd.exe /c whoami".execute() println proc.text`
    
3. **Escalada de privilegios:** Dependiendo de los permisos con los que Jenkins se ejecute, se pueden obtener privilegios de sistema.

#### **Mitigación:**

- Configurar controles de acceso estrictos.
- Deshabilitar la consola Groovy si no es necesaria.
- Mantener Jenkins y sus plugins actualizados.

---

### **2. RottenPotato (SeImpersonatePrivilege)**

Esta técnica aprovecha el privilegio **SeImpersonatePrivilege** en sistemas Windows para escalar privilegios localmente.

#### **Concepto Clave:**

- **SeImpersonatePrivilege** permite a un proceso hacerse pasar por otro proceso en el sistema.
- Esto puede ser explotado en servicios que aceptan tokens de autenticación de otros usuarios, como servidores COM/DCOM.

#### **Explotación:**

1. **Condiciones previas:**
    - El atacante necesita acceso local al sistema.
    - El proceso debe ejecutarse con **SeImpersonatePrivilege**.
2. **RottenPotato:** Usa una vulnerabilidad en cómo Windows maneja el servicio COM/DCOM para interceptar tokens NTLM.
3. **Escalada de privilegios:** Una vez que el token de autenticación es interceptado, se utiliza para obtener una sesión SYSTEM.

#### **Mitigación:**

- Restringir el acceso a cuentas de usuario con privilegios de **SeImpersonatePrivilege**.
- Aplicar parches de seguridad recientes para mitigar ataques basados en DCOM.

---

### **3. PassTheHash (Psexec)**

Es una técnica de post-explotación que permite a un atacante usar un **hash NTLM** en lugar de una contraseña para autenticarse en sistemas remotos.

#### **Qué es el PassTheHash:**

- Se basa en cómo Windows maneja la autenticación NTLM.
- Si un atacante obtiene el hash de una contraseña (sin necesidad de la contraseña original), puede usarlo para acceder a otros sistemas.

#### **Psexec:**

- Es una herramienta de Sysinternals que permite ejecutar comandos en sistemas remotos usando credenciales de red.
- En combinación con PassTheHash, permite mover lateralmente por la red.

#### **Explotación:**

1. **Obtener el hash:** Mediante herramientas como Mimikatz.
2. **Ejecutar Psexec:**
    
    bash
    
    Copiar código
    
    `psexec.py DOMAIN/USER@TARGET -hashes LMHASH:NTHASH`
    
3. **Resultado:** Acceso remoto con los privilegios del usuario cuya cuenta fue comprometida.

#### **Mitigación:**

- Usar autenticación Kerberos en lugar de NTLM.
- Implementar medidas de seguridad para proteger credenciales (LSA Protection).
- Monitorizar intentos de autenticación sospechosos.

---

### **4. Breaking KeePass**

KeePass es un popular gestor de contraseñas que, aunque muy seguro, puede ser vulnerado en ciertos escenarios.

#### **Vectores de Ataque:**

1. **Acceso físico a la base de datos (.kdbx):**
    
    - Si el archivo está almacenado sin cifrado fuerte o la contraseña maestra es débil, se puede forzar mediante técnicas de fuerza bruta.
    - Herramientas como **KeeCracker** o **Hashcat** pueden ser usadas para descifrar el archivo.
2. **Ataque en memoria:**
    
    - Si KeePass está en ejecución, las contraseñas descifradas pueden ser extraídas directamente de la memoria del sistema usando herramientas como **Mimikatz**.
3. **Plugins maliciosos:**
    
    - Plugins de KeePass no oficiales pueden contener código malicioso que robe contraseñas o la clave maestra.

#### **Mitigación:**

- Usar contraseñas maestras fuertes y combinarlas con un archivo de clave adicional.
- Asegurarse de descargar KeePass y sus plugins solo de fuentes confiables.
- Proteger el entorno donde se ejecuta KeePass, especialmente contra accesos no autorizados.

---

### **5. Alternate Data Streams (ADS)**

Es una característica del sistema de archivos NTFS que permite asociar metadatos ocultos a archivos, lo que puede ser explotado por atacantes para ocultar archivos maliciosos.

#### **Qué son los ADS:**

- NTFS permite añadir flujos adicionales de datos a un archivo sin afectar el contenido visible del archivo principal.
- Ejemplo: `archivo.txt:stream_malicioso`.

#### **Explotación**

1. **Ocultar malware:**
    - Un atacante puede almacenar ejecutables o scripts maliciosos en un ADS.
    - Por ejemplo:
`echo MaliciousCode > file.txt:malicious_stream`

2. **Ejecución del ADS:**
    - Los flujos ADS pueden ejecutarse directamente en sistemas Windows:

        `start file.txt:malicious_stream`

#### **Detección y Mitigación:**

- Usar herramientas como **streams.exe** de Sysinternals para identificar ADS en un sistema.
- Configurar políticas para evitar la creación de ADS en sistemas críticos.
- Monitorizar la actividad del sistema de archivos para detectar comportamientos sospechosos.

---