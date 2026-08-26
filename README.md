# Laboratorio Corporativo de Soporte IT & Active Directory

##  Descripción del Proyecto
Implementación de un entorno corporativo virtualizado para simular la infraestructura de red, gestión de identidades, políticas de seguridad y mesa de ayuda de una empresa.



##  Tecnologías Utilizadas
* **Hipervisor:** Oracle VirtualBox 7.x
* **Servidor de Directorio:** Windows Server 2022 (Standard / Desktop Experience)
* **Cliente:** Windows 10 
* **Servicios de Red:** Active Directory Domain Services (AD DS), DNS local, DHCP/IP estática



##  Arquitectura de Red y Direccionamiento

* **Nombre de Red Interna (VirtualBox):** `lab-red`
* **Dominio Corporativo:** `lab.local`
* **NetBIOS:** `LAB`

| Equipo | Rol | Adaptador de Red | Dirección IP | Máscara | DNS Preferido |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **SRV-DC01** | Controlador de Dominio (AD/DNS) | 1. NAT<br>2. Red Interna (`lab-red`) | DHCP (Internet)<br>`192.168.10.1` | —<br>`255.255.255.0` | —<br>`127.0.0.1` |
| **WIN-CLI01** | Estación de Trabajo Cliente | 1. Red Interna (`lab-red`) | `192.168.10.20` *(por configurar)* | `255.255.255.0` | `192.168.10.1` |

---

## Fases Implementadas

### Fase 1: Aislamiento de Red e Instalación Base
1. Creación del switch virtual aislado (`lab-red`) en VirtualBox para garantizar tráfico seguro entre hosts sin interferir con la red física doméstica.
2. Despliegue de la máquina virtual **Windows Server 2022** con doble interfaz de red:
   * Adaptador 1 (NAT) para descarga de actualizaciones.
   * Adaptador 2 (Red Interna) para servicios del dominio corporativo.

### Fase 2: Configuración de Red e Instalación de AD DS & DNS
1. Asignación de direccionamiento IP estático en la interfaz LAN (`192.168.10.1/24`) con loopback DNS (`127.0.0.1`).
2. Instalación de los roles de servidor:
   * **Active Directory Domain Services (AD DS)**
   * **Servidor DNS**
3. Promoción del servidor a primer Controlador de Dominio raíz de un nuevo bosque (`lab.local`).

---

## Troubleshooting

* **Comportamiento "Sin acceso a Internet" en placa LAN:**
  * *Observación:* La interfaz interna muestra conectividad limitada / sin internet.
  * *Causa:* Al tratarse de una subred aislada sin puerta de enlace (Default Gateway) hacia el exterior, Windows clasifica la red como no identificada. El tráfico local opera correctamente.
* **Advertencia de Delegación DNS durante la promoción:**
  * *Observación:* El asistente indica que no se pudo crear una delegación para el servidor DNS autoritativo primario.
  * *Resolución:* Según lo averiguado es el comportamiento estándar esperado al crear el primer controlador de dominio raíz de un bosque nuevo sin infraestructura DNS superior previa.

---
