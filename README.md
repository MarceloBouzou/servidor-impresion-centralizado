# Caso de Estudio: Servidor de Impresión Centralizado

## (Resumen del Proyecto)

Este repositorio documenta una solución de software (un servicio desatendido en Python) diseñada para **superar una limitación de hardware y transformar una impresora USB local en un recurso de red compartido**. El sistema permite a múltiples usuarios enviar trabajos de impresión desde sus propios escritorios a una impresora térmica especializada (Zebra) que carece de conectividad de red nativa.

---

## 🎯 El Desafío: Un Cuello de Botella Físico en un Proceso Crítico

La operación de la empresa dependía de una única impresora térmica para la generación de etiquetas de código de barras, un activo crucial para la gestión de stock. Sin embargo, este equipo presentaba serias ineficiencias:

*   **Activo Crítico Subutilizado:** La impresora solo podía ser utilizada desde la PC a la que estaba conectada físicamente.
*   **Pérdida de Productividad Masiva:** Múltiples empleados debían **interrumpir sus tareas, abandonar sus puestos de trabajo y desplazarse físicamente** hasta la PC anfitriona para imprimir una sola etiqueta.
*   **Fragmentación del Flujo de Trabajo:** Este proceso de "peregrinación" para imprimir rompía la concentración y generaba micro-interrupciones constantes, reduciendo la eficiencia general del equipo.
*   **Falta de Trazabilidad:** No existía un registro centralizado de qué se imprimía, cuándo y desde dónde, dificultando cualquier auditoría.

---

## 🛠️ Mi Solución: Una Infraestructura de Impresión Virtual

Para resolver esta limitación de hardware con una solución de software, diseñé y desarrollé una arquitectura cliente-servidor a medida:

### Arquitectura y Flujo de Trabajo:

1.  **Servidor de Impresión Desatendido (Daemon):** Creé un servicio en Python que se ejecuta de forma continua en la PC anfitriona. Este servicio **monitorea en tiempo real un directorio de red compartido** utilizando la librería `watchdog`.
2.  **Cola de Impresión Virtual:** Los empleados ahora imprimen desde sus propias PCs a un archivo (`.prn`) y simplemente lo depositan en la carpeta de red compartida, que actúa como una **cola de impresión virtual centralizada**. No necesitan moverse de sus escritorios.
3.  **Procesamiento y Despacho Automático:** Al detectar un nuevo archivo en la carpeta, mi servicio lo envía inmediatamente a la cola de impresión de la impresora Zebra local, logrando una **impresión instantánea y desatendida**.
4.  **Sistema de Archivo y Logging para Auditoría:** Una vez impreso con éxito, el servicio **mueve el archivo `.prn` a un directorio de archivo histórico** y **registra la transacción** (nombre del archivo, fecha, hora) en un archivo `impresos.log`, creando un sistema de trazabilidad completo desde cero.

---

## 🚀 Impacto Cuantificable en el Negocio

*   **Recuperación de Más de 20 Horas de Trabajo Productivo al Mes:** Al eliminar los desplazamientos físicos, se recuperó un tiempo valiosísimo que el personal de logística ahora puede dedicar a tareas de mayor valor.
*   **Maximización del Retorno de Inversión (ROI) del Hardware:** Se transformó un activo de hardware de uso individual en un **recurso de red disponible para todos los puestos autorizados**, maximizando su utilidad sin ninguna inversión adicional.
*   **Flujo de Trabajo Ininterrumpido:** Se eliminaron las interrupciones y los cambios de contexto, permitiendo que el equipo mantenga una concentración total en sus tareas críticas.
*   **Implementación de Trazabilidad y Control:** El sistema de logging proporciona, por primera vez, un **registro auditable de toda la actividad de impresión**, permitiendo verificar trabajos y resolver incidencias rápidamente.

---

## 💻 Stack Tecnológico

*   **Lenguaje:** Python
*   **Librerías Clave:** `watchdog` (para monitoreo de sistema de archivos), `os`, `shutil`, `subprocess`
*   **Arquitectura:** Servicio Desatendido (Daemon)
*   **Protocolo:** Monitoreo de Carpeta Compartida en Red (SMB/CIFS)
