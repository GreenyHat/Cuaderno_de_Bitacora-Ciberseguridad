![[siete-capas-de-modelo-osi-1623136242-cywGEI9RkM.webp]]


El administrador de una red configura STP con el fin de evitar la formación de bucles de capa 2. Sin embargo, quiere asegurarse que el switch que ha elegido como root siga siéndolo independientemente de las prioridades de los switches que se agreguen posteriormente. ¿Qué comando ha de configurar en el switch? Switch(config)# spanning-tree portfast bpduguard default Switch(config-if)# spanning-tree portfast Switch(config-if)# spanning-tree bpduguard enable Switch(config)# spanning-tree guard root

###### ChatGPT Plus

ChatGPT

El comando adecuado para garantizar que un switch específico siga siendo el root en un entorno de STP, independientemente de los switches que se agreguen posteriormente, no está en las opciones mencionadas. En este caso, lo que debe configurarse es la **prioridad del switch** para que tenga la prioridad más baja y, por ende, sea seleccionado como root switch en el protocolo Spanning Tree.

El comando es:

bash

Copiar código

`Switch(config)# spanning-tree vlan [vlan-id] priority 0`

O también puedes usar el comando global para todas las VLANs (si el switch soporta MST o PVST+):

bash

Copiar código

`Switch(config)# spanning-tree priority 0`

### Explicación:

- **STP Root Switch**: En STP, el switch con la prioridad más baja es seleccionado como root. Si la prioridad no se configura manualmente, el valor predeterminado es **32768**. Configurar un valor más bajo (0 es el más bajo posible) asegura que el switch sea el root switch.

Ninguno de los comandos en las opciones garantiza explícitamente que el switch seguirá siendo el root en el futuro. Sin embargo, el comando **`spanning-tree guard root`** (última opción) puede usarse para proteger puertos de acceso y evitar que un switch no autorizado pueda convertirse en root al enviar BPDUs superiores, pero **no configura al switch como root directamente**.

### Qué hacen los comandos mencionados:

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

4o