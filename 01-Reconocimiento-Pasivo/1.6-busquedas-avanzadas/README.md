# Práctica de Laboratorio: Búsquedas Avanzadas (Google Dorking)

Este laboratorio es una piedra angular del **Módulo de Reconocimiento Pasivo**. El objetivo es aprender a utilizar motores de búsqueda (como Google) y archivos públicos (como la GHDB y la Wayback Machine) como potentes herramientas de OSINT (Inteligencia de Fuentes Abiertas).

El objetivo no es "atacar" nada, sino **encontrar información sensible que ha sido indexada y expuesta accidentalmente** al público.

---

## 🛠️ Operadores y Herramientas Clave

| Herramienta / Operador | Propósito | Ejemplo de Uso |
| :--- | :--- | :--- |
| **`site:`** | Restringe la búsqueda a un solo dominio. | `site:pearson.com` |
| **`filetype:`** | Filtra los resultados por tipo de archivo. | `filetype:pdf` |
| **`intitle:`** | Busca la palabra clave en el título de la página. | `intitle:certification` |
| **`inurl:`** | Busca la palabra clave en la URL de la página. | `inurl:admin` |
| **GHDB** | Base de datos de "dorks" pre-hechos. | `https.www.exploit-db.com/google-hacking-database/` |
| **Wayback Machine** | Archivo histórico de sitios web ("arqueología digital"). | `https://web.archive.org` |

---

## 📝 Resumen del Proceso y Hallazgos

El laboratorio se dividió en tres partes principales, cada una construyendo sobre la anterior.

### Parte 1: Búsquedas Avanzadas de Google (Dorking)

En esta parte, aprendimos a combinar operadores para crear filtros de búsqueda muy precisos.

* **Comando:** `ethical hacker site:pearson.com`
    * **Hallazgo:** Filtra con éxito todos los resultados para mostrar solo páginas del dominio `pearson.com`.
* **Comando:** `ethical hacker site:pearson.com filetype:pdf`
    * **Hallazgo:** Refina aún más la búsqueda para mostrar únicamente archivos PDF de `pearson.com` que contengan las palabras clave.

#### 📍 Hallazgo Clave (OSINT de Personal):

El *dork* más poderoso de esta sección fue el enfocado en el reconocimiento de personal:

* **Comando:** `site:linkedin.com intitle:"Cisco"`
* **Valor para un Atacante:** Este *dork* es una mina de oro. Proporciona una lista casi completa de empleados de Cisco en LinkedIn. Un atacante usa esto para:
    1.  **Perfilar la Tecnología:** Leer los perfiles para ver qué tecnologías usa la empresa (ej: "Experto en AWS", "Administrador de Salesforce").
    2.  **Crear Listas de Objetivos:** Identificar empleados (especialmente nuevos o en roles no técnicos) para ataques de *phishing* e ingeniería social.

### Parte 2: La Base de Datos de Piratería de Google (GHDB)

Aquí dejamos de inventar nuestros propios *dorks* y usamos el "libro de recetas" de la comunidad de seguridad.

* **Herramienta:** Google Hacking Database (GHDB) en `Exploit-DB`.
* **Hallazgo Clave (Búsqueda de Servicios Vulnerables):**
    * **Dork:** `allinurl:tsweb/default.htm`
    * **Resultado:** Este *dork* encontró páginas de inicio de sesión de **Windows Terminal Services**.
    * **Valor para un Atacante:** Esto no solo encuentra un portal de inicio de sesión, sino que implica fuertemente que el servidor es un **Windows 2000** o **Server 2003**. Estos son sistemas operativos "fin de vida", extremadamente antiguos y vulnerables a miles de *exploits* conocidos.

### Parte 3: La Máquina Wayback (Wayback Machine)

En esta parte, realizamos "arqueología digital" para encontrar información que ya ha sido eliminada del sitio web actual.

* **Herramienta:** `web.archive.org`
* **Proceso:** Se buscó `cisco.com` y se navegó a una "instantánea" de 1999.
* **Hallazgo Clave (Ingeniería Social y Phishing):**
    * **Valor para un Atacante:** Esta es una ventaja táctica inmensa. Un atacante puede:
        1.  **Copiar Sitios para Phishing:** Descargar el HTML y los logotipos de una versión antigua del sitio para crear una página de *phishing* perfectamente creíble.
        2.  **Encontrar Información Antigua:** Revisar páginas "Sobre Nosotros" o "Contacto" de hace una década para encontrar nombres de gerentes y empleados que pueden ser usados en correos de ingeniería social.
        3.  **Encontrar Archivos Olvidados:** Usar el filtro de URL para buscar archivos (`.pdf`, `.zip`, `.bak`) que contenían información sensible y que fueron eliminados del sitio actual, pero no del archivo.

---

## 💡 Conclusión y Reflexión

Este laboratorio demuestra por qué el **reconocimiento pasivo** es la fase más crítica de un *pentest*.

Un atacante puede, sin hacer un solo "ping" al servidor del objetivo (y por lo tanto, de forma **100% invisible**), lograr lo siguiente:
1.  **Mapear la Infraestructura** (Dominios, Servidores IP).
2.  **Identificar el Personal** (Objetivos de Phishing).
3.  **Descubrir la Tecnología Interna** (AWS, Google, Windows 2000).
4.  **Encontrar Vulnerabilidades Directas** (Portales de `tsweb`, contraseñas en GHDB).

Esta información es la base sobre la cual se planifica y ejecuta todo el resto del ataque.
