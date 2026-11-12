#  DE.HOSTING

##  Descripción del Proyecto
**DE.HOSTING** es un entorno de hosting multicliente diseñado con un enfoque estricto en el aislamiento y la seguridad de los servicios. Desplegado íntegramente sobre la infraestructura de virtualización **IsardVDI**, el proyecto simula un proveedor de alojamiento web profesional separando el tráfico público, los servicios de los clientes y la gestión interna en diferentes subredes.

##  Arquitectura de Red
La topología se divide en tres máquinas virtuales principales corriendo sobre **Ubuntu Server 26.04**:

*   **MV1 - Router / Firewall (WAN):** Actúa como puerta de enlace perimetral. Se encarga del enrutamiento del núcleo (IP Forwarding) y el enmascaramiento de IPs (NAT mediante iptables). Aloja el servidor DNS autoritativo (BIND9) para el dominio interno `dehosting.local` y actúa como barrera de seguridad.
*   **MV DMZ - Servidor de Clientes (`192.168.20.10/24`):** Zona desmilitarizada aislada de la gestión interna. Centraliza el motor de contenedores y el proxy inverso para exponer de forma segura las páginas web o aplicaciones de los clientes del hosting hacia el exterior.
*   **MV LAN - Administración (`192.168.30.10/24`):** Red interna, privada y securizada. Diseñada para alojar los servicios críticos de administración del sistema, el stack de monitorización global del clúster y la gestión de copias de seguridad.

##  Stack Tecnológico
*   **Sistema Operativo:** Ubuntu Server 26.04 LTS
*   **Redes y Seguridad:** Netplan, iptables (Netfilter Persistent), sysctl (Kernel IP Routing)
*   **Servicios Base:** BIND9 (DNS), isc-dhcp-server
*   **Alojamiento Web:** Docker, Docker Compose, Nginx
*   **Monitorización:** Prometheus, Grafana, Loki

##  Requisitos Previos
Para replicar este entorno de laboratorio es necesario disponer de:
- Hypervisor o entorno virtualizado (ej. IsardVDI, Proxmox, VMware).
- 3 redes virtuales segmentadas (WAN con salida a Internet, switch virtual DMZ, switch virtual LAN).
- 3 máquinas virtuales con Ubuntu Server (mínimo 2GB RAM por máquina recomendados para despliegues con Docker).

##  Flujo de Despliegue Básico
1. **Infraestructura Base:** Despliegue de las MVs y asignación de las tarjetas de red a sus respectivos switches virtuales.
2. **Capa de Red:** Activación del enrutamiento estático en la MV1 y asignación de IPs mediante `netplan` en las zonas aisladas.
3. **Seguridad:** Aplicación de reglas restrictivas y NAT (MASQUERADE) en el firewall perimetral.
4. **Resolución de Nombres:** Despliegue del servidor DNS maestro para orquestar las peticiones internas hacia la DMZ.
5. **Contenedores:** Instalación del motor Docker y levantamiento de los servicios de cara al cliente final.

---
*Proyecto desarrollado en el marco del ciclo de Administració de Sistemes Informàtics en Xarxa (ASIX).*
