

# Práctica: Control de Servomotor MG996R por Interfaz Web

## 1. Descripción del Proyecto
Este proyecto implementa un sistema de control de posición angular en tiempo real para un servomotor **TowerPro MG996R**. El sistema utiliza un microcontrolador **Arduino UNO R4 WiFi** que hospeda un servidor web HTTP embebido, permitiendo modificar la posición del motor dentro de un rango de 0° a 180° mediante peticiones HTTP asíncronas enviadas desde cualquier navegador en la red local[cite: 1, 3].

---

## 2. Objetivos del Proyecto

### Objetivo General
Implementar y validar un sistema de control remoto para posicionar un servomotor en el rango de 0° a 180° en tiempo real a través de una interfaz web alojada directamente en un servidor HTTP incrustado en el microcontrolador.

### Objetivos Específicos
* Configurar la conectividad WiFi y el servidor HTTP embebido en la tarjeta Arduino UNO R4 WiFi[cite: 1].
* Desarrollar una interfaz web responsiva para el envío de comandos de posicionamiento angular (como 20°, 60°, 120° y 180°)[cite: 1, 3].
* Validar la transmisión estable de señales PWM y el comportamiento físico del servomotor sin picos de inestabilidad[cite: 1].

---

## 3. Estructura de Carpetas del Proyecto

```text
PracticaServoM/
├── Codigo/        # Firmware principal (.ino) y respaldo en texto (.txt)
├── Diagrama/      # Esquema de conexiones e imágenes de evidencia (image.png, image1.jpeg, image2.jpeg)
├── Reporte/       # Reporte técnico detallado en formatos Markdown (.md), TXT y PDF
├── Resultados/    # Evaluación de rendimiento, tabla de parámetros y PDF de resultados
├── Terminal/      # Registros de log del Monitor Serie y comandos de prueba
└── Video/         # Enlace a la demostración en video del proyecto
