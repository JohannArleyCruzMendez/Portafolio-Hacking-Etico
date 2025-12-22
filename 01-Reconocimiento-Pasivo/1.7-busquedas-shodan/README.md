Práctica de Laboratorio: Reconocimiento Pasivo con Shodan 🛰️
🎯 Objetivos de la Práctica
Crear y configurar una cuenta de usuario en Shodan y obtener una clave de API.

Utilizar la interfaz web de Shodan para localizar dispositivos de IoT vulnerables.

Emplear la interfaz de línea de comandos (CLI) de Shodan en Kali Linux para realizar búsquedas y obtener estadísticas.

Analizar la superficie de ataque y los riesgos asociados a servicios expuestos.

🛠️ Recursos y Configuración
Sistema Operativo: Kali Linux VM.

Herramienta: Shodan (Web y CLI).

Clave de API: 

Inicialización: Se vinculó la cuenta mediante el comando shodan init obteniendo la confirmación "Successfully initialized".

🚀 Metodología Ejecutada
1. Interacción con la CLI (Kali Linux)
Diagnóstico de Cuenta: Mediante shodan info se verificó que la cuenta dispone de 0 créditos de consulta y escaneo, lo que limita las búsquedas avanzadas desde la terminal.

Identificación de Origen: Se utilizó shodan myip para determinar la IP pública de salida().

Búsqueda General: Se ejecutó shodan search webcam para visualizar resultados en texto plano directamente en el prompt.

2. Búsqueda Avanzada (Interfaz Web)
Se utilizaron filtros de tipo filtro:valor para refinar los resultados en Bogotá, Colombia:

Consulta: port:21 city:"Bogota" 230.

Lógica del Dork: Filtrar por el puerto estándar de FTP (21) en una ubicación específica, buscando el código de respuesta 230, que indica un inicio de sesión exitoso (frecuentemente asociado a acceso anónimo).

🔍 Análisis de Hallazgos Críticos
Caso de Estudio: IP 190.253.242.37 (Bogotá)
Se identificó un host perteneciente a Colombia Telecomunicaciones S.A. ESP BIC con las siguientes brechas de seguridad:

FTP Anónimo: El banner del puerto 21 confirmó: 230 User login complete, lo que permite el acceso a archivos sin autenticación privada.

Servicios Críticos Expuestos:

Puerto 23 (Telnet): Protocolo de administración inseguro que transmite datos en texto plano.

Puerto 161 (SNMP): Protocolo de gestión que puede revelar topología y configuración interna.

Superficie de Ataque Masiva: En otros dispositivos analizados, se detectaron hasta 18 puertos abiertos, incluyendo el 37215 y 7547, conocidos por ser objetivos de botnets como Mirai.

🛡️ Recomendaciones de Seguridad (Hardening)
Cierre de Puertos: Deshabilitar servicios no esenciales como Telnet (23) y puertos de gestión de ISP (7547, 37215) para reducir la superficie de ataque.

Cifrado de Administración: Reemplazar Telnet por SSH (puerto 22) para asegurar que la administración sea cifrada.

Control de FTP: Desactivar el acceso anónimo y configurar el servidor para requerir credenciales robustas.

Ofuscación: Configurar los servicios para que no anuncien versiones específicas o nombres de fabricantes en sus banners, dificultando el reconocimiento pasivo.

💡 Reflexión Final
Shodan es una herramienta fundamental para los administradores de TI, ya que permite realizar auditorías de exposición externa, detectar dispositivos conectados sin autorización (Shadow IT) y verificar proactivamente vulnerabilidades antes de que sean explotadas.

❓ Respuestas al Cuestionario del Laboratorio
¿Unidad fundamental de datos?: El Banner (metadatos del servicio).

¿Información general disponible?: IP, nombre de host, país, ciudad, organización e ISP.

¿Información de puertos abiertos?: Se visualizan las respuestas crudas de los servicios, permitiendo identificar versiones de software y configuraciones de acceso.
