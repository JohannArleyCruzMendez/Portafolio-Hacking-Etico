# Laboratorio: Búsqueda de Información a partir de Certificados SSL

Este laboratorio forma parte del Módulo de Reconocimiento Pasivo. El objetivo es aprender a extraer información valiosa (como subdominios, tecnologías y debilidades) analizando certificados SSL/TLS y sus implementaciones.

---

## 🛠️ Herramientas y Comandos Utilizados

| Herramienta | Propósito | Comando de Ejemplo |
| :--- | :--- | :--- |
| **Navegador Web** | Inspección básica de certificados. | `(Clic en el 🔒 junto a la URL)` |
| **`crt.sh`** | Búsqueda en logs de Transparencia de Certificados. | `https://crt.sh/?q=netacad.com` |
| **`sslscan`** | Escaneo activo de protocolos y cifrados SSL/TLS. | `sslscan netacad.com` |
| **`aha`** | Utilidad para convertir salida de terminal a HTML. | `sudo apt install -y aha` |
| **Pipe (`|`) y Redirect (`>`)** | Combinar comandos y guardar la salida. | `... | aha > reporte.html` |

---

## 🔬 Hallazgos Clave

### Parte 1: Inspección Manual (Navegador)

* **Objetivo:** `netacad.com`
* **Certificado emitido para:** `www.netacad.com`
* **Algoritmo de Firma:** `SHA-256 ECDSA` (Fuerte)
* **CA Raíz (Ejemplos):** `Microsoft`, `SSL.com`, `Starfield` (GoDaddy).
* **Costo de Certificados:** Varía desde **Gratis** (`Let's Encrypt`) hasta **$70+** (`SSL.com`).

### Parte 2: Reconocimiento Pasivo (crt.sh)

Esta herramienta fue la más útil para **expandir la superficie de ataque** sin tocar la red del objetivo.

* **Hallazgo Principal:** Descubrimiento de numerosos subdominios que no son de uso público, revelando infraestructura interna.
* **Subdominios Críticos Encontrados:**
    * `nsmail.netacad.com` (Servidor de correo)
    * `devselfservice.netacad.com` (Entorno de Desarrollo)
    * `qaselfservice.netacad.com` (Entorno de Control de Calidad - QA)
* **Dominios Relacionados:** Se descubrió que `skillsforall.com` está directamente asociado con `netacad.com`, implicando una infraestructura de TI compartida y estandarizada.

### Parte 3 y 4: Reconocimiento Activo (sslscan)

Esta herramienta fue la más útil para **auditar vulnerabilidades** en un objetivo conocido.

* **Hallazgo Principal:** Se identificó una **implementación criptográfica débil** en el servidor de `netacad.com`.
* **Vulnerabilidad Específica:** El servidor tiene **habilitados los protocolos `TLSv1.0` y `TLSv1.1`**. Estos son protocolos obsoletos con vulnerabilidades conocidas (ej. POODLE, BEAST) y deben estar deshabilitados.
* **Comando de Reporte:** Se generó un informe HTML con los hallazgos usando `aha`:
    ```bash
    sslscan netacad.com | aha > sfa_cert.html
    ```

---

## 💡 Conclusión del Laboratorio

Este laboratorio demostró la diferencia clave entre dos tipos de reconocimiento:

1.  **Reconocimiento Pasivo (`crt.sh`):** Nos ayuda a **descubrir objetivos** (mapa de la red).
2.  **Reconocimiento Activo (`sslscan`):** Nos ayuda a **encontrar vulnerabilidades** en esos objetivos (puntos débiles).
