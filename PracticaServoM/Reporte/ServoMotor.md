Reporte Técnico: Control de Servomotor por Interfaz Web

Plataforma: Arduino UNO R4 WiFi | Protocolo: HTTP Server | Documento: Portafolio de Evidencias

1. Objetivo de la Práctica

Implementar y validar un sistema de control remoto para posicionar un servomotor en el rango de 0° a 180° en tiempo real a través de una interfaz web alojada directamente en un servidor HTTP incrustado en el microcontrolador.
• Servidor web incrustado: Alojamiento y ejecución de la interfaz web desde la memoria del microcontrolador.
• Control en tiempo real: Permite modificar la posición del servomotor en tiempo real desde la web.

2. Materiales Utilizados

• Tarjeta de desarrollo: Arduino UNO R4 WiFi
• Actuador: Servomotor (TowerPro MG996R)
• Conectividad: Cables jumper de conexión

3. Esquema de Conexiones Físicas

--------------------------------------------------------------------------------
LÍNEA / PIN SERVO     | PIN EN ARDUINO | FUNCIÓN
--------------------------------------------------------------------------------
Señal (Naranja)       | PIN 9 (PWM)    | Control de posición vía PWM
GND (Marrón / Negro)  | GND            | Tierra común del circuito
ENERGÍA (Rojo)        | 5V             | Alimentación de potencia
--------------------------------------------------------------------------------

4. Análisis de Resultados

El sistema funciona completamente. Las conexiones con jumpers entre el Arduino y el servomotor están correctamente establecidas y garantizan la transmisión estable de la señal. El Monitor Serie de Arduino muestra exitosamente la dirección IP asignada a la página web alojada en el microcontrolador (Arduino UNO R4 WiFi). La página web responde correctamente y refleja en tiempo real los cambios de ángulo del servomotor.

5. Conclusiones Individuales

La arquitectura del microcontrolador Arduino UNO R4 WiFi simplifica significativamente la creación de servidores locales sin requerir de plataformas o servicios de terceros.
