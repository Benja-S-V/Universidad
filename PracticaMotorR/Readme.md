# Práctica: Control de Motor CC con Driver L293D y Arduino UNO R4 WiFi

## 1. Descripción del Proyecto
Este proyecto implementa el sistema de control bidireccional y de velocidad para un motor de corriente continua (CC) de 12V utilizando un integrado **L293D (Puente H)** y una tarjeta **Arduino UNO R4 WiFi**. La interacción con el sistema se realiza en tiempo real procesando comandos de texto enviados desde el Monitor Serie a 115200 baudios. 

El circuito incluye una etapa de potencia aislada mediante una fuente regulada externa de 12V para garantizar el torque óptimo del motor sin comprometer la estabilidad lógica ni provocar fluctuaciones de voltaje en el microcontrolador.

---

## 2. Objetivos del Proyecto

### Objetivo General
Implementar y validar físicamente el control bidireccional y de velocidad PWM de un motor de CC utilizando la tarjeta Arduino UNO R4 WiFi y el driver L293D, interactuando en tiempo real mediante comandos de texto por el Monitor Serie.

### Objetivos Específicos
* Configurar las salidas digitales y la señal PWM (Pin 3) en el Arduino UNO R4 WiFi para controlar la velocidad y sentido de giro del Puente H L293D.
* Implementar una etapa de potencia aislada alimentada con una fuente regulada de 12V compartiendo una masa común (Common GND).
* Establecer comunicación serie a 115200 baudios para la interpretación e inserción instantánea de comandos lógicos (`ADELANTE`, `RETROCEDE`, `PARA`, `LENTO`, `MEDIO`, `RAPIDO`).

---

## 3. Estructura de Carpetas del Proyecto

```text
PracticaMotorR/
├── Codigo/        # Firmware principal (.ino) y respaldo en texto (.txt)
├── Diagrama/      # Esquemas de conexión e imágenes de evidencia (image.png, image1.jpeg, image2.jpeg)
├── Reporte/       # Reporte técnico en Markdown (.md), TXT y PDF
├── Resultados/    # Evaluación de parámetros de prueba y PDFs
├── Terminal/      # Registros de log de comunicación serie (115200 baudios)
└── Video/         # Demostración en video del motor en funcionamiento
