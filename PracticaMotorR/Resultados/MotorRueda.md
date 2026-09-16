# Reporte de Resultados y Evidencias Experimentales

Este documento registra la validación técnica, las pruebas de operación en tiempo real y el banco de evidencias físicas del sistema de control para un motor de corriente continua (CC), utilizando el microcontrolador **Arduino UNO R4 WiFi**, un driver de potencia de puente en H y alimentación regulada externa.

---

## 1. Resumen de Validación Operativa

* **Microcontrolador:** Arduino UNO R4 WiFi (Renesas RA4M1 @ 48 MHz)
* **Driver de Potencia:** Módulo de potencia H-Bridge (L293D / L298N)
* **Alimentación de Potencia:** Fuente ATX regulada (Línea de 12V DC - Cable Amarillo)
* **Tasa de Transmisión UART:** 115200 bps
* **Ubicación en Repositorio:** `Practicas/Resultados/`

---

## 2. Matriz Metodológica de Pruebas Serie (115200 Baudios)

Se evaluó la respuesta dinámica del actuador mediante el envío de comandos de texto desde el Monitor Serie. Al trabajar a **115200 bps**, el tiempo de transmisión por paquete se redujo a menos de 0.7 ms, eliminando latencias perceptibles durante las transiciones de velocidad y sentido de giro.

| Comando Serie | Estado Lógico de Pines | Valor PWM (Pin 3) | Ciclo de Trabajo (%) | Comportamiento Dinámico en Rueda | Estado |
| :--- | :--- | :---: | :---: | :--- | :---: |
| **`ADELANTE`** | `IN3` = HIGH, `IN4` = LOW | 255 | 100% | Giro continuo horario con torque máximo | **PASÓ** |
| **`RETROCEDE`** | `IN3` = LOW, `IN4` = HIGH | 255 | 100% | Inversión inmediata a sentido antihorario | **PASÓ** |
| **`LENTO`** | Según dirección activa | 100 | ~39.2% | Rotación suave y constante a bajas RPM | **PASÓ** |
| **`MEDIO`** | Según dirección activa | 180 | ~70.5% | Incremento progresivo de velocidad y par | **PASÓ** |
| **`RAPIDO`** | Según dirección activa | 255 | 100% | Régimen máximo alimentado por la línea de 12V | **PASÓ** |
| **`PARA`** | `IN3` = LOW, `IN4` = LOW | 0 | 0% | Freno eléctrico inmediato por cortocircuito bajo | **PASÓ** |

---

## 3. Análisis de Alimentación y Aislamiento Electrónico

1. **Inmunidad ante *Brown-Outs* (12V ATX):**  
   Las baterías comerciales de 9V experimentan caídas drásticas de tensión por debajo de 6.5V durante los picos de corriente al arrancar el motor, lo que provocaba reinicios indeseados (*brown-outs*) en la etapa lógica del Arduino UNO R4 WiFi. La implementación de la fuente ATX con la línea de **12V DC (cable amarillo)** garantizó una reserva de corriente superior a 1.5A, manteniendo constante la alimentación de 5V en la placa principal.

2. **Unificación de Referencias (GND):**  
   Se enlazaron los terminales neutros de la fuente ATX (cables negros) con el pin `GND` del microcontrolador para establecer una referencia de voltaje común, evitando bucles de masa y garantizando señales PWM limpias hacia el driver.

---

## 4. Banco de Evidencias Gráficas

### A. Esquema Técnico de Conexiones
El diseño esquemático completo y la distribución de pines se encuentran vinculados mediante la ruta relativa correspondiente:

![Esquema Técnico Digital](../Diagrama/MotorRuedaImagen2.jpeg)
*Figura 1: Diagrama de conexiones e interconexión lógica/potencia (`MotorRuedaImagen2.jpeg`).*

---

### B. Ensamble Físico Real de Banco
Demostración del circuito armado y energizado en el entorno de laboratorio:

![Montaje Físico Real del Circuito](Imagen3.jpeg)
*Figura 2: Banco de pruebas en operación. Se aprecia la fuente ATX alimentando el módulo driver mediante la línea de 12V (cable amarillo) y masa unificada (cable negro), la placa Arduino UNO R4 WiFi conectada por USB y el motorreductor con rueda amarilla de pruebas (`Imagen3.jpeg`).*

---

## 5. Demostración en Video (Prueba en Tiempo Real)

Haz clic en la imagen a continuación para reproducir la demostración en **YouTube Shorts** de los cambios de velocidad por PWM y sentido de giro:

[![Ver Demostración en YouTube Shorts](https://img.youtube.com/vi/whfj_7Ri47Q/hqdefault.jpg)](https://youtube.com/shorts/whfj_7Ri47Q)

> 🔗 **Enlace directo al video:** [Ver demostración en YouTube Shorts](https://youtube.com/shorts/whfj_7Ri47Q)

---

## 6. Conclusiones Técnicas

1. **Aislamiento de Potencia:** La separación de la etapa lógica (5V USB/Arduino) y la etapa de potencia (12V ATX) es indispensable para proteger el procesador Renesas RA4M1 del Arduino UNO R4 WiFi ante picos inductivos.
2. **Validación de Código:** El control por señales PWM permite modular la velocidad del motor de forma uniforme sin perder estabilidad ni generar sobrecalentamiento en el driver.
3. **Documentación Modular:** La estructura de rutas relativas de GitHub garantiza que las imágenes (`../Diagrama/` e `Imagen3.jpeg`) y el video integrado sean accesibles desde cualquier plataforma web o móvil.
