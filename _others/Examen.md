---
title: Examne final de REDES II
author: Mauricio Mamani Flores
date: 2022-02-04
category: Jekyll
layout: post
---
### Cantidad de bits
ip : 10.0.0.0
mascara:  255.255.0.0
bit: de 2^4
     16-2 = 14  

### Tabla de Subredes


| Subred           | Dirección de Red  | Máscara de Subred   |
|------------------|-------------------|---------------------|
| 10.16.0.0        | 10.16.0.0         | 255.255.0.0         |
| 10.32.0.0        | 10.32.0.0         | 255.255.0.0         |
| 10.64.0.0        | 10.64.0.0         | 255.255.0.0         |
| 10.80.0.0        | 10.80.0.0         | 255.255.0.0         |
| 10.96.0.0        | 10.96.0.0         | 255.255.0.0         |
| 10.112.0.0       | 10.112.0.0        | 255.255.0.0         |

### Tabla de las Vlan

| Subred         | Dirección de Red  | Máscara de Subred  | Dirección de Broadcast | Direcciones Utilizables         |
|----------------|-------------------|---------------------|------------------------|----------------------------------|
| VLAN 10        | 10.96.0.0         | 255.255.0.0         | 10.1.255.255          | 10.96.0.1 - 10.96.255.254         |
| VLAN 20        | 10.112.0.0         | 255.255.0.0         | 10.2.255.255          | 10.112.0.1 - 10.112.255.254         |


### Configuracion de los router
```bash
enable
configure terminal
hostname LPZ
interface Se0/0
ip address 10.32.0.1 255.255.0.0
no shutdown
```
repetir para cada router mismos pasos
```bash

# Crear las rutas dinamicas
enable 
conf t
router rip
network 10.32.0.0
```
repetir para cada ruta dinamica a los router
```bash
# Configurar Vlan
vlan 10
interface vlan 10
ip address 10.96.0.1 255.255.0.0

vlan 20
interface vlan 20
ip address 10.112.0.1 255.255.0.0

end
write memory

```