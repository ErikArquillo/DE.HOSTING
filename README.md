# 🌐 DE.HOSTING

## 📝 Descripción del Proyecto
**DE.HOSTING** es un entorno de hosting multicliente diseñado con un enfoque estricto en el aislamiento y la seguridad de los servicios. Desplegado íntegramente sobre la infraestructura de virtualización **IsardVDI**, el proyecto simula un proveedor de alojamiento web profesional separando el tráfico público, los servicios de los clientes y la gestión interna en diferentes subredes.

## 🏗️ Arquitectura de Red
La topología se divide en tres máquinas virtuales principales corriendo sobre **Ubuntu Server 26.04**:

*   **MV1 - Router / Firewall (WAN):** Actúa como puerta de enlace perimetral. Se encarga del enrutamiento del núcleo (IP Forwarding) y el enmascaramiento de IPs (NAT mediante iptables). Alojará el servidor DNS autoritativo (BIND9) para el dominio interno `dehosting.local` y actuará como barrera de seguridad.
*   **MV DMZ - Servidor de Clientes (`192.168.20.10/24`):** Zona desmilitarizada aislada de la gestión interna. Centraliza el motor de contenedores y el proxy inverso para exponer de forma segura las páginas web o aplicaciones de los clientes del hosting hacia el exterior.
*   **MV LAN - Administración (`192.168.30.10/24`):** Red interna, privada y securizada. Diseñada para alojar los servicios críticos de administración del sistema, el stack de monitorización global del clúster y la gestión de copias de seguridad.

## 🛠️ Stack Tecnológico
*   **Sistema Operativo:** Ubuntu Server 26.04 LTS
*   **Redes y Seguridad:** Netplan, iptables (Netfilter Persistent), sysctl (Kernel IP Routing)
*   **Servicios Base:** BIND9 (DNS), isc-dhcp-server
*   **Alojamiento Web:** Docker, Docker Compose, Nginx
*   **Monitorización (Proyectado):** Prometheus, Grafana, Loki

## 🚀 Estado de Implementación (Roadmap)
- [x] Diseño de topología y planificación del proyecto (Kanban/Gantt).
- [x] Despliegue del hardware virtual e interfaces aisladas en IsardVDI.
- [x] Activación de IP Forwarding y NAT persistente en el Router perimetral.
- [x] Segmentación de red y configuración de Netplan con IPs estáticas para DMZ y LAN.
- [ ] Configuración del servidor DNS (BIND9) para la resolución del dominio `dehosting.local`.
- [ ] Implementación de Firewall avanzado (políticas restrictivas y port forwarding hacia DMZ).
- [ ] Despliegue de los primeros servicios web de clientes mediante contenedores Docker.
- [ ] Instalación y configuración del stack de monitorización y logs en la zona LAN.

---
*Proyecto desarrollado en el marco del ciclo de Administració de Sistemes Informàtics en Xarxa (ASIX).*
