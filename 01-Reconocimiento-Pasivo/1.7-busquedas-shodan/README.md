
# Práctica de Laboratorio: Reconocimiento Pasivo con Shodan 🛰️ 🎯

Este laboratorio es una pieza fundamental del **Módulo de Reconocimiento Pasivo**. El objetivo es aprender a utilizar el motor de búsqueda de infraestructura **Shodan** para identificar dispositivos de IoT, servicios expuestos y posibles vulnerabilidades sin interactuar directamente con el objetivo.

---

## 🛠️ Objetivos de la Práctica
* Crear y configurar una cuenta de usuario en Shodan y obtener una clave de API.
* Utilizar la interfaz web de Shodan para localizar dispositivos de IoT vulnerables.
* Emplear la interfaz de línea de comandos (CLI) de Shodan en Kali Linux para realizar búsquedas y obtener estadísticas.
* Analizar la superficie de ataque y los riesgos asociados a servicios expuestos.

---

## ⚙️ Recursos y Configuración
* **Sistema Operativo**: Kali Linux VM.
* **Herramientas**: Shodan (Web y CLI).
* **Clave de API**:
* **Inicialización**: Se vinculó la cuenta mediante el comando `shodan init`.

---

## 🚀 Metodología Ejecutada

### 1. Interacción con la CLI (Kali Linux)
* **Diagnóstico de Cuenta**: Mediante `shodan info` se verificó la disponibilidad de créditos (0 créditos de consulta/escaneo en cuenta gratuita).
* **Identificación de Origen**: Uso de `shodan myip` para determinar la IP pública de salida .
* **Búsqueda General**: Ejecución de `shodan search webcam` para visualizar resultados en texto plano.

### 2. Búsqueda Avanzada (Interfaz Web)
Se utilizaron filtros específicos para refinar los resultados en Colombia:
* **Consulta**: `port:21 country:CO 230`.
* **Lógica del Dork**: Filtrar por puerto FTP (21) en una ubicación específica buscando el código de respuesta **230** (User login complete).

---

## 🔍 Hallazgos Críticos (Caso de Estudio)

### Análisis de la IP: 190.253.242.37 (Bogotá)
* **FTP Anónimo**: El banner confirmó acceso exitoso sin contraseña (`230 User login complete`).
* **Superficie de Ataque**: Se detectaron múltiples puertos abiertos (23, 7547, 37215) asociados históricamente a vulnerabilidades críticas y botnets.

---

## 💡 Reflexión Final
Para un administrador de TI, Shodan es una herramienta de **auditoría externa** vital para detectar dispositivos conectados sin autorización (**Shadow IT**) y validar que las políticas de firewall estén bloqueando correctamente los servicios de administración.
