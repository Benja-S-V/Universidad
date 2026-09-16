**Texto en Markdown para copiar manualmente (si prefieres pegarlo directamente en el editor):**

```markdown
# Práctica: Control de Servomotor MG996R por Interfaz Web

## 1. Descripción del Proyecto
Este proyecto implementa un sistema de control de posición angular en tiempo real para un servomotor **TowerPro MG996R**[cite: 1, 2]. El sistema utiliza un microcontrolador **Arduino UNO R4 WiFi** que hospeda un servidor web HTTP embebido (puerto 80), permitiendo modificar la posición del motor dentro de un rango de 0° a 180° mediante peticiones HTTP asíncronas enviadas desde cualquier navegador en la red local[cite: 1, 2, 3].

---

## 2. Objetivos del Proyecto

### Objetivo General
Implementar y validar un sistema de control remoto para posicionar un servomotor en el rango de 0° a 180° en tiempo real a través de una interfaz web alojada directamente en un servidor HTTP incrustado en el microcontrolador[cite: 1, 2].

### Objetivos Específicos
* Configurar la conectividad WiFi del microcontrolador Arduino UNO R4 mediante la librería `WiFiS3`[cite: 1, 2].
* Desarrollar una interfaz web HTML5 responsiva con botones de acceso rápido (20°, 60°, 120°, 180°) y un selector de ángulo tipo slider[cite: 1, 2, 3].
* Garantizar el correcto aislamiento eléctrico del circuito utilizando una **fuente de alimentación externa** y configurando la **Tierra Común (Common GND)** para proteger el microcontrolador contra picos de corriente[cite: 1, 2, 3].

---

## 3. Estructura de Carpetas del Proyecto

```text
PracticaServoM/
├── Codigo/        # Firmware principal (.ino) y respaldo en texto (.txt)
├── Diagrama/      # Esquema de conexiones y fotos de evidencia (image1.jpeg, image2.jpeg)
├── Reporte/       # Reporte técnico detallado en formatos Markdown (.md), TXT y PDF
├── Resultados/    # Evaluación de rendimiento, tabla de parámetros y PDF de resultados
├── Terminal/      # Registros de log del Monitor Serie y comandos de prueba
└── Video/         # Enlace a la demostración en video del proyecto
