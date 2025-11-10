# Práctica de Laboratorio: Conociendo más sobre la organización

Este laboratorio es parte del módulo de Reconocimiento Pasivo y se centra en el uso de herramientas OSINT (Inteligencia de Fuentes Abiertas) para recopilar información sobre objetivos, específicamente a través de brechas de correo electrónico y metadatos de archivos.

---

## 🛠️ Herramientas y Comandos Clave

| Herramienta | Propósito | Comando de Ejemplo |
| :--- | :--- | :--- |
| **Have I Been Pwned** | Verificar si un email está en una brecha de datos. | `https://haveibeenpwned.com` |
| **EmailHarvester** | Intentar encontrar correos de un dominio. | `emailharvester -d [dominio] -e [motor]` |
| **Spiderfoot** | Automatización OSINT avanzada. | `spiderfoot -l 127.0.0.1:5001` |
| **Google Dorks** | Búsqueda avanzada para encontrar archivos. | `"google confidential" filetype:pdf` |
| **ExifTool** | Instalar la herramienta de metadatos. | `sudo apt install -y libimage-exiftool-perl` |
| **ExifTool** | Leer metadatos de un archivo. | `exiftool [archivo.pdf]` |
| **ExifTool** | Exportar metadatos a CSV. | `exiftool -csv . > reporte.csv` |

---

## 📝 Resumen del Proceso y Hallazgos

### Parte 1: Encuentre información sobre violaciones de correo electrónico

El objetivo de esta parte fue encontrar correos electrónicos asociados a un objetivo y determinar si han sido comprometidos en brechas de datos anteriores.

**Paso 1: Investigar con 'Have I Been Pwned' (HIBP)**

* Se utilizó el sitio `haveibeenpwned.com` para verificar una dirección de correo electrónico personal.
* **Hallazgo:** El correo electrónico **sí** formaba parte de múltiples brechas de datos, incluyendo **Deezer**, **Edmodo** e **Internet Archive**. Esto confirma que los atacantes tienen acceso a hashes de contraseñas antiguas y datos personales (nombres de usuario, fechas de nacimiento, etc.) asociados con ese correo.

**Paso 2: Usar `emailharvester`**

* Se instaló la herramienta `emailharvester` en Kali.
* Se intentó usarla para encontrar correos en dominios de práctica (`h4cker.org`, `scanme.nmap.org`).
* **Hallazgo (Problemática):** La herramienta falló repetidamente.
    * Al usar el motor `ask`, se produjo un error de Python (`TypeError`).
    * Al usar los motores `google` y `bing`, la herramienta fue bloqueada o no encontró resultados ("No emails found"), incluso en dominios grandes como `mit.edu`.
* **Lección:** Esto demuestra que las herramientas de *scraping* simples son cada vez menos fiables contra las defensas modernas de los motores de búsqueda.

**Paso 3: Usar `spiderfoot`**

* Se inició `spiderfoot` como un servicio web local (`spiderfoot -l 127.0.0.1:5001`).
* Se exploró la interfaz y el sistema de módulos, que es mucho más potente porque se basa en claves API.
* **Hallazgo (Problemática):** Se identificó que módulos clave (como el de `Have I Been Pwned`) ahora requieren una **suscripción de pago** para obtener una clave API, lo cual no era el caso antes.
* Se ejecutó un escaneo básico sin APIs de pago sobre una dirección de correo. La herramienta confirmó la correlación de **"Alto Riesgo"**, verificando que el correo estaba en múltiples brechas, validando los hallazgos de HIBP.

### Parte 2: Ver metadatos del archivo

El objetivo de esta parte fue extraer información oculta (metadatos) de archivos públicos para perfilar la tecnología y el personal de una organización.

**Paso 1: Instalar `ExifTool`**

* Se instaló la biblioteca de Perl en Kali usando `sudo apt install -y libimage-exiftool-perl`.
* Se verificaron los tipos de archivo compatibles con `exiftool -listf`, confirmando que puede leer casi todo (PDF, DOC, PNG, MPEG, etc.).

**Paso 2: Usar `ExifTool`**

* Se utilizó un **Google Dork** (`"google confidential" filetype:pdf`) para encontrar un documento PDF expuesto públicamente.
* Se descargó el archivo (`hoa-analytics-basico.pdf`) y se analizó con `exiftool`.
* **Hallazgo Clave:** El metadato más importante encontrado fue:
    ```
    Creator: Google
    ```
* **Valor para un Atacante:** Este simple dato sugiere fuertemente que la organización utiliza **Google Workspace** (Docs, Slides, etc.) en lugar de Microsoft Office. Esta información es crucial para crear un ataque de *phishing* (ej. una página de inicio de sesión falsa de Google) mucho más creíble y dirigido.
* Finalmente, los metadatos de todos los archivos en el directorio se exportaron exitosamente a un archivo CSV para su posterior análisis (`exiftool -csv . > reporte_metadatos.csv`).

---

## 💡 Conclusión y Reflexión

Este laboratorio demostró cómo el reconocimiento pasivo no se trata de una sola herramienta, sino de **conectar puntos** de múltiples fuentes de datos públicos.

Un atacante ahora sabe que un empleado (cuyo correo se encontró en la brecha de Deezer) trabaja en una empresa que usa Google Workspace (por el `Creator` del PDF). Esta combinación de información permite la creación de un ataque de **ingeniería social** altamente personalizado y convincente.
