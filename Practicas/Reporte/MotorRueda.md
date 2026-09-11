## Reporte de Práctica: Control de Motor CC con L293D y Arduino UNO R4 WiFi

**Objetivo**
Implementar y validar físicamente el control bidireccional y de velocidad de un motor de corriente continua (CC) utilizando la tarjeta Arduino UNO R4 WiFi y el driver L293D (Puente H), interactuando en tiempo real mediante comandos de texto enviados desde el Monitor Serie de la computadora.

**Materiales Utilizados**
* Tarjeta de desarrollo Arduino UNO R4 WiFi
* Circuito integrado L293D (Puente H)
* Motor de corriente continua (CC)
* Fuente de poder de CC (Alimentación externa de potencia)
* Protoboard y cables de conexión (Jumpers)

**Justificación del Uso de Fuente de Poder**
Se descartó el uso de una pila estándar de 9V debido a su baja capacidad de suministro de corriente. Los motores de CC generan picos de consumo (especialmente en el arranque o cambios de velocidad) que provocan caídas severas de tensión en pilas comunes. Al utilizar una fuente de poder dedicada para el pin 8 (VCC2) del L293D, se garantizó un suministro de energía constante y estable, evitando la pérdida de torque en el motor y protegiendo la lógica del circuito de fluctuaciones eléctricas.

**Esquema de Conexiones Físicas**

| Componente / Origen | Pin de Origen | Pin en L293D | Función |
| :--- | :--- | :--- | :--- |
| **Arduino UNO R4 WiFi** | Pin 3 (PWM) | Pin 9 (EN3/4) | Control de velocidad vía PWM |
| **Arduino UNO R4 WiFi** | Pin 4 | Pin 10 (IN3) | Dirección de giro 1 |
| **Arduino UNO R4 WiFi** | Pin 5 | Pin 15 (IN4) | Dirección de giro 2 |
| **Arduino UNO R4 WiFi** | 5V | Pin 16 (VCC1) | Alimentación lógica del chip |
| **Arduino / Fuente** | GND / Negativo (-) | Pines 4, 5, 12, 13 | Tierra común del circuito |
| **Fuente de poder** | Positivo (+) | Pin 8 (VCC2) | Fuente externa para el motor |
| **Motor CC** | Terminales 1 y 2 | Pines 11 y 14 | Salidas de potencia (OUT3 y OUT4) |

**Lógica de Control y Comandos Serie**
El código grabado en el Arduino UNO R4 WiFi establece la comunicación serie a **115200 baudios** para asegurar una lectura rápida de los datos. El microcontrolador ejecuta las siguientes acciones al recibir cada comando:

* **`ADELANTE`**: Activa `IN3` en ALTO y `IN4` en BAJO para hacer girar el motor en sentido horario.
* **`RETROCEDE`**: Invierte las señales (`IN3` en BAJO e `IN4` en ALTO) para girar en sentido antihorario.
* **`PARA`**: Coloca ambas salidas en BAJO y apaga el pin de habilitación (`ENB` a 0) para detener el motor.
* **`LENTO`**: Modula la señal PWM del Pin 3 a un valor de **100** (~40% de potencia).
* **`MEDIO`**: Modula la señal PWM del Pin 3 a un valor de **180** (~70% de potencia).
* **`RAPIDO`**: Aplica el ciclo de trabajo máximo en PWM con valor de **255** (100% de potencia).

**Resultados y Conclusiones**
El montaje físico funcionó correctamente. La tarjeta Arduino UNO R4 WiFi respondió de forma inmediata a la lectura de comandos en el Monitor Serie a 115200 baudios. La integración de la fuente de poder permitió sostener los cambios bruscos de giro y los distintos niveles de velocidad (PWM) sin experimentar caídas de voltaje. Finalmente, organizar el L293D sobre la canaleta central de la protoboard unificando las tierras aseguró una señal de control limpia y un rendimiento óptimo del puente H.
