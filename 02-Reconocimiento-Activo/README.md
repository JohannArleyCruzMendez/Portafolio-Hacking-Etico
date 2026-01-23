# Módulo 02: Reconocimiento Activo

Este directorio contiene las prácticas de laboratorio relacionadas con la fase de **Reconocimiento Activo** del curso de Hacking Ético. El objetivo es interactuar directamente con los sistemas del objetivo para identificar puertos, servicios, versiones y vulnerabilidades, analizando las respuestas del tráfico de red.

---

## 🔬 Laboratorios Realizados

A continuación se listan las prácticas detalladas realizadas en este módulo. Cada una cuenta con su propia documentación explicando el proceso, comandos y hallazgos.

* 📄 **[2.1: Enumeración con Nmap](./2.1-Enumeracion-Nmap.md)**
    * *Escaneo de hosts, detección de SO, versiones de servicios y descubrimiento de vulnerabilidades en FTP y SMB.*
* 📄 **[2.2: Elaboración de paquetes con Scapy](./2.2-Elaboracion-Paquetes-Scapy.md)**
     * *Creación manual de paquetes ICMP y TCP, inyección de payload y análisis de flags (SYN/ACK) para determinar el estado de puertos.*
* 📄 **[2.4: Rastreo de redes con Wireshark](./2.3-Rastreo-Wireshark.md)**
    * *Captura de tráfico con tcpdump, análisis forense de DNS, extracción de credenciales HTTP en texto plano y robo de cookies de sesión (Session Hijacking).*


---

### 🛠️ Herramientas Utilizadas
* **Nmap:** Escaneo de puertos y scripts NSE.
* **Wireshark:** Análisis de protocolos y tráfico de red.
* **Scapy:** Manipulación y creación de paquetes TCP/IP en Python.
