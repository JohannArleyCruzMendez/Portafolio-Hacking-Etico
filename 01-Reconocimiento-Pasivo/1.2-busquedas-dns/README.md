# Laboratorio 1.2: Búsquedas de DNS y Whois

## 🎯 Objetivo
Este laboratorio se centra en el uso de herramientas de línea de comandos fundamentales (`nslookup`, `dig`, `host`, `whois`) para realizar un reconocimiento pasivo. El objetivo es interrogar directamente al Sistema de Nombres de Dominio (DNS) y a los registros de `whois` para obtener inteligencia accionable sobre la infraestructura y propiedad de un dominio.

---
## 🛠️ Herramientas Utilizadas
* `nslookup`
* `dig`
* `host`
* `whois`


---
## 🔬 Metodología y Comandos

### 1. Análisis de `nslookup`

Se comenzó con `nslookup` para entender las consultas DNS básicas. El análisis de su manual (`man nslookup`) reveló dos modos de operación y comandos clave.

* **Modo No Interactivo (Búsqueda Rápida):**
    ```bash
    # Buscar la IP (Registro A) de un dominio
    nslookup netacad.com
    ```
* **Modo Interactivo (Sesión de Investigación):** Permite realizar múltiples consultas de forma eficiente.
    ```bash
    # Iniciar sesión interactiva
    nslookup

    # Comando del manual: Cambiar el tipo de consulta a MX (servidores de correo)
    > set type=mx
    > cisco.com

    # Comando del manual: Cambiar el servidor DNS para la sesión
    > server 8.8.8.8
    > netacad.com
    ```
* **Búsqueda Inversa (IP a Nombre):**
    ```bash
    nslookup 72.163.5.201
    ```


Análisis y Conceptos Clave Obtenidos:

La lectura del manual reveló que nslookup opera en dos modos distintos:

🌐 Modo No Interactivo: Ideal para búsquedas rápidas y únicas. Se ejecuta el comando seguido del dominio, devuelve la información y se cierra.

🗣️ Modo Interactivo: Se activa al ejecutar nslookup sin argumentos. Proporciona un prompt (>) y permite realizar una "sesión" de investigación. Dentro de este modo, el comando set actúa como un "panel de control" para modificar las consultas.

Opciones del Comando set:

set type=<valor>: El comando más importante. Permite cambiar el tipo de registro DNS a consultar.

a: Direcciones IPv4 (por defecto).

mx: Servidores de correo.

ns: Servidores de nombres.

txt: Registros de texto (para verificaciones de seguridad).

any: Cualquiera de los registros disponibles.

set all: Muestra un resumen de la configuración actual de la sesión (servidor, tipo de consulta, etc.).

set port=<valor>: Cambia el puerto de la consulta DNS (el estándar es el 53). Es una opción avanzada.

set recurse | norecurse: Controla si el servidor DNS puede consultar a otros servidores si no conoce la respuesta.

set debug: Muestra información de depuración detallada sobre los paquetes de la consulta.

Conclusión de este paso: Para un reconocimiento detallado, el modo interactivo de nslookup es la opción preferida, gracias a la flexibilidad que ofrece el comando set para ajustar las consultas sobre la marcha.


### 2. Investigación de Propiedad con `whois`

Se utilizó `whois` para obtener información de registro, tanto para dominios como para direcciones IP.

* **Consulta de Dominio:** Se compararon los registros de `cisco.com` y `netacad.com` para confirmar la propiedad.
    ```bash
    whois cisco.com
    ```
* **Consulta de Dirección IP:** Se usó `whois` sobre una IP conocida de Cisco para descubrir el rango de red completo (`NetRange`) que posee la organización, expandiendo masivamente la superficie de ataque.
    ```bash
    whois 72.163.5.201
    ```

### 3. Consultas Avanzadas con `dig`

Se utilizó `dig` como la herramienta profesional para obtener respuestas detalladas y estructuradas.

* **Consulta Específica:** Se consultó por un registro `AAAA` (IPv6).
    ```bash
    dig cisco.com AAAA
    ```
* **Consulta a un Servidor Específico:** Se usó la sintaxis `@` para interrogar directamente a un servidor DNS externo.
    ```bash
    dig cisco.com @8.8.8.8 ns
    ```
* **Búsqueda Inversa (IP a Nombre):** Se utilizó la opción `-x` para encontrar el nombre de host asociado a una IP.
    ```bash
    dig -x 72.163.1.1
    ```
### 4. Consultas Simples con `host`

Se exploró el comando `host` como una alternativa rápida y de fácil lectura para consultas simples.

* **Búsqueda Directa e Inversa:**
    ```bash
    # De nombre a IP
    host cisco.com

    # De IP a nombre
    host 72.163.1.1
    ```

---
## 📊 Análisis de Hallazgos Clave

* **Infraestructura de Red:** Se identificó que dominios como `cisco.com` utilizan **balanceo de carga** (múltiples registros A) y servicios de CDN como **Akamai** para mejorar el rendimiento y la seguridad.
* **Proveedores de Servicios:** Se descubrió que `netacad.com` utilizan **Google Workspace** para su correo electrónico (a través de registros MX).
* **Mapeo de la Superficie de Ataque:** Mediante `whois` a una IP, se identificó un **`NetRange`** completo (`72.163.0.0/16`) propiedad de Cisco, revelando miles de IPs como objetivos potenciales.
* **Descubrimiento de Dispositivos:** Una búsqueda inversa con `dig -x` reveló un nombre de host (`hsrp-72-163-1-1.cisco.com`), identificando un **router de borde** que utiliza el protocolo HSRP de Cisco, un hallazgo de infraestructura de alto valor.

**Conclusión:** Este laboratorio demostró que con un puñado de comandos básicos es posible desentrañar una cantidad significativa de información sobre la arquitectura, los proveedores y las políticas de seguridad de una organización.

