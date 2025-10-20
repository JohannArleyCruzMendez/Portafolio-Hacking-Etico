# Laboratorio 01: Reconocimiento y OSINT

Este repositorio documenta las prácticas realizadas durante el **Módulo 3: Reconocimiento** como parte de mi formación en el curso de **Hacking Ético de Cisco**. El objetivo principal de este laboratorio fue aplicar técnicas de recolección de información pasiva (OSINT) para descubrir la huella digital de un objetivo, utilizando herramientas estándar de la industria.

---

## 🎯 Objetivo del Laboratorio

El propósito de esta práctica fue simular la primera fase de una prueba de penetración, donde el objetivo es recopilar la mayor cantidad de información posible sobre un objetivo de forma sigilosa, sin interactuar directamente con sus sistemas. El fin último es analizar estos datos para identificar posibles vectores de ataque y priorizar los esfuerzos para las siguientes fases del pentesting.

---

## 🧠 Conceptos Clave Aprendidos

- **Reconocimiento Pasivo vs. Activo:** La diferencia fundamental entre observar desde lejos y "tocar" los sistemas del objetivo.
- **Inteligencia de Fuentes Abiertas (OSINT):** El arte de encontrar y correlacionar información disponible públicamente en internet.
- **Huella Digital (Digital Footprint):** Mapeo de la infraestructura visible de una organización (dominios, subdominios, IPs, etc.).
- **Análisis y Priorización:** La habilidad crítica de transformar datos brutos en inteligencia accionable, identificando los objetivos más prometedores.

---

## 🛠️ Herramientas Utilizadas

- **SpiderFoot:** Escáner OSINT automatizado con interfaz gráfica para un reconocimiento amplio.
- **Recon-ng:** Framework modular en línea de comandos para un reconocimiento controlado y preciso.
- **Recursos OSINT:** `OSINT Framework`, `WhatsMyName` y otros para búsquedas manuales.
- **Kali Linux:** El entorno de laboratorio donde se ejecutaron todas las herramientas.

---

## 🔬 Metodología y Comandos Ejecutados

El laboratorio se dividió en dos fases principales, utilizando las herramientas más adecuadas para cada enfoque.

### Fase 1: Reconocimiento Automatizado con SpiderFoot

En esta fase, se utilizó SpiderFoot para obtener una visión general y rápida del objetivo `h4cker.org`.

1.  **Lanzamiento del Servicio:** Se inició el servidor web de SpiderFoot desde la terminal.
    ```bash
    spiderfoot -l 127.0.0.1:5001
    ```
2.  **Configuración del Escaneo:** A través de la interfaz web, se configuró un nuevo escaneo con el objetivo **`h4cker.org`** y se seleccionó el perfil de **"Footprint"**, que se enfoca en métodos pasivos.
3.  **Análisis de Resultados:** El escaneo reveló una gran cantidad de información, destacando la **enumeración de múltiples subdominios** descubiertos principalmente a través del análisis de registros de **Certificados SSL Públicos**.
4.  **Priorización de Objetivo:** Dentro de los resultados, el subdominio **`backdoor.h4cker.org`** fue identificado como un objetivo de alta prioridad, demostrando el proceso de análisis de inteligencia.



### Fase 2: Reconocimiento Controlado con Recon-ng

En esta fase, se utilizó Recon-ng para realizar búsquedas específicas y controladas, demostrando el flujo de trabajo en un entorno de línea de comandos.

1.  **Gestión de Workspaces:** Se creó un espacio de trabajo aislado para la investigación, una práctica fundamental para la organización.
    ```bash
    # Dentro de recon-ng
    workspaces create hackxor_scan
    ```
2.  **Instalación de Módulos:** Se exploró el **Marketplace** para buscar e instalar los módulos necesarios.
    ```bash
    # Buscar un módulo
    marketplace search hackertarget

    # Instalar el módulo
    marketplace install recon/domains-hosts/hackertarget
    ```
3.  **Ejecución de un Módulo (Ejemplo `hackertarget`):** Se siguió el ciclo completo de carga, configuración y ejecución.
    ```bash
    # Cargar el módulo en el workspace
    modules load hackertarget

    # Ver las opciones del módulo
    info

    # Establecer el dominio objetivo
    options set SOURCE hackxor.net

    # Ejecutar el módulo
    run

    # Revisar el resumen de resultados
    dashboard
    ```
4.  **Comparación de Fuentes:** Se repitió el proceso con otros módulos como `bing_domain_web` para comparar y consolidar resultados, demostrando el principio clave de que **múltiples fuentes de datos producen una imagen más completa**.

---

## 🏁 Conclusión y Reflexión

Este laboratorio práctico fue fundamental para entender que la fase de reconocimiento es uno de los pilares de una prueba de penetración exitosa. El éxito no radica simplemente en la cantidad de datos recopilados, sino en la **capacidad de analizar, correlacionar y priorizar esa información** para construir un plan de ataque informado y eficiente.

---

### ⚠️ Disclaimer

Todas las actividades documentadas en este repositorio se realizaron en un entorno de laboratorio controlado y se dirigieron exclusivamente a dominios (`h4cker.org`, `hackxor.net`) designados para la práctica y el aprendizaje de la ciberseguridad. Ninguna actividad se dirigió a sistemas productivos o sin permiso explícito.
