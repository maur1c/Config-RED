---
title: Configuracion de la RED
author: Mauricio Mamani Flores
date: 2022-02-04
category: Jekyll
layout: post
---


# Documentación de Configuración de Red

Esta documentación proporciona la configuración básica para 9 switches de acceso, 2 switches de capa 3 (Cisco 3650) y un router, basada en la topología especificada. Se describen las configuraciones necesarias para habilitar la conectividad entre diferentes VLANs y la conexión a redes externas.

## Asunciones Previas

1. **Switches de Acceso**: Conectados en cada bloque (A, B, C, D, E, ADMIN).
2. **Switches de Capa 3 (Cisco 3650)**: Configurados para enrutamiento inter-VLAN.
3. **Router**: Conectado al switch de capa 3 principal para la salida hacia redes externas (posiblemente hacia Internet).
4. **VLANs**:
   - **Bloque A**: VLAN 10 (10.1.0.0/16) 
   - **Bloque B**: VLAN 20 (10.2.0.0/16)
   - **Bloque C**: VLAN 30 (10.3.0.0/16)
   - **Bloque D**: VLAN 40 (10.4.0.0/16)
   - **Bloque E**: VLAN 50 (10.5.0.0/16)
   - **Administración**: VLAN 70 (10.7.0.0/16)

## Configuración para los Switches de Acceso (Switches 1-9)

Cada switch de acceso tendrá configuradas las VLANs correspondientes y los puertos asignados en modo access para conectar dispositivos en su respectiva VLAN.
```bash
enable
configure terminal

# Crear VLANs
vlan 10
 name BLOQUE_A

vlan 70
 name ADMINISTRACION

# Configurar puertos en modo access
interface range FastEthernet0/1 - 24
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast

# Configurar el puerto de enlace troncal hacia el switch de capa 3
interface GigabitEthernet0/1/0
 switchport trunk encapsulation dot1q
 switchport mode trunk

end
write memory


```
Nota: Repite esta configuración en cada switch de acceso en función de la VLAN de cada bloque (VLAN 10 para Bloque A, VLAN 20 para Bloque B, y así sucesivamente). Cambia el número de VLAN y el nombre en cada switch según el bloque correspondiente.

### Ejemplo de Configuración para un Switch de Acceso (Switch en Bloque A)

```bash
enable
configure terminal

# Crear VLANs
vlan 10
 name BLOQUE_A

vlan 70
 name ADMINISTRACION

# Configurar puertos en modo access
interface range FastEthernet0/1 - 24
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast

# Configurar el puerto de enlace troncal hacia el switch de capa 3 
interface GigabitEthernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk

end
write memory
```

### Configuración para los Switches de Capa 3 (Cisco 3650)
Los switches de capa 3 realizarán el enrutamiento entre VLANs. Configura una interfaz VLAN para cada VLAN para que cada red tenga su Gateway en estos switches.
```bash
enable
configure terminal

# Crear VLANs
vlan 10
 name BLOQUE_A

vlan 20
 name BLOQUE_B

vlan 30
 name BLOQUE_C

vlan 40
 name BLOQUE_D

vlan 50
 name BLOQUE_E

vlan 70
 name ADMINISTRACION

# Configurar las interfaces VLAN con direcciones IP (Gateways)
interface vlan 10
 ip address 10.1.0.1 255.255.0.0

interface vlan 20
 ip address 10.2.0.1 255.255.0.0

interface vlan 30
 ip address 10.3.0.1 255.255.0.0

interface vlan 40
 ip address 10.4.0.1 255.255.0.0

interface vlan 50
 ip address 10.5.0.1 255.255.0.0

interface vlan 70
 ip address 10.7.0.1 255.255.0.0  # Gateway para la VLAN de Administración

# Configurar enrutamiento entre VLANs
ip routing

# Configurar puertos troncales hacia switches de acceso
interface range GigabitEthernet1/0/1 - 2
 switchport trunk encapsulation dot1q
 switchport mode trunk

# Configurar el puerto troncal hacia el router
interface GigabitEthernet1/0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk

end
write memory
```

### Configuración del Router
Conecta el router al switch principal a través de una interfaz GigabitEthernet. Esta configuración permite al router manejar el tráfico hacia una red externa o como puerta de enlace principal.
```bash
enable
configure terminal
hostname LAN
# Configurar la interfaz hacia el switch de capa 3
interface GigabitEthernet0/0
 ip address 10.7.0.2 255.255.0.0  # Mismo rango que la VLAN de Administración
 no shutdown

```

### Introducción al Subneteo
El subneteo que realizaremos es el proceso de dividir una red en subredes más pequeñas para mejorar la gestión y la eficiencia del uso de direcciones IP.

### Beneficios del Subneteo
- **Mejoras en la Seguridad**: Limita el acceso entre subredes.
- **Eficiencia en la Gestión**: Facilita la administración de direcciones IP.
- **Reducción de Tráfico**: Disminuye la congestión en la red.
### IP principal
tenemos una red con el siguiente rango de direcciones:

Dirección de Red: 10.0.0.0/16
Mascara         : 255.255.0.0
Clase           :B

### Tabla de Subredes

| Subred         | Dirección de Red  | Máscara de Subred  | Dirección de Broadcast | Direcciones Utilizables         |
|----------------|-------------------|---------------------|------------------------|----------------------------------|
| VLAN 10        | 10.1.0.0         | 255.255.0.0         | 10.1.255.255          | 10.1.0.1 - 10.1.255.254         |
| VLAN 20        | 10.2.0.0         | 255.255.0.0         | 10.2.255.255          | 10.2.0.1 - 10.2.255.254         |
| VLAN 30        | 10.3.0.0         | 255.255.0.0         | 10.3.255.255          | 10.3.0.1 - 10.3.255.254         |
| VLAN 40        | 10.4.0.0         | 255.255.0.0         | 10.4.255.255          | 10.4.0.1 - 10.4.255.254         |
| VLAN 50        | 10.5.0.0         | 255.255.0.0         | 10.5.255.255          | 10.5.0.1 - 10.5.255.254         |
| VLAN 70        | 10.7.0.0         | 255.255.0.0         | 10.7.255.255          | 10.7.0.1 - 10.7.255.254         |

### Descripción de la Tabla:
- **Subred**: Indica el nombre de la VLAN correspondiente.
- **Dirección de Red**: La dirección base de la subred.
- **Máscara de Subred**: Indica cuántos bits están reservados para la red.
- **Dirección de Broadcast**: La dirección utilizada para enviar paquetes a todos los hosts en la subred.
- **Direcciones Utilizables**: Rango de direcciones IP que pueden asignarse a los dispositivos.
### Conclusión
El subneteo es una técnica fundamental en la configuración de redes, ya que permite dividir
una red en subredes más pequeñas, lo que mejora la seguridad, la gestión y la eficiencia del uso de direcciones IP.

El subneteo es una técnica fundamental en la administración de redes. Al segmentar una red, se mejora la seguridad y la eficiencia, facilitando así la gestión del tráfico y de las direcciones IP.
