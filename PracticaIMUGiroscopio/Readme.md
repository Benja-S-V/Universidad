# Inclinómetro con control de motorreductor

Práctica 3.2.3 de Sistemas Programables (Instituto Tecnológico de Mazatlán).

Sistema embebido que mide la inclinación frontal (*pitch*) de un sensor MPU-6050 y la usa para controlar el sentido y la velocidad de un motorreductor mediante un puente H L298N. Corre en un **Arduino UNO R4 WiFi**.

## Características

- Lectura del MPU-6050 por I2C accediendo directamente a sus registros, **sin librerías externas** para el sensor.
- Ángulo calculado con un **filtro complementario** (acelerómetro + giroscopio).
- Control de sentido y velocidad con PWM, con **zona muerta**, **velocidad mínima útil** y **rampa de aceleración**.
- **Paro de seguridad** inmediato si falla el sensor, con recuperación automática.
- Indicador de inclinación en la **matriz de LEDs** de la placa y estado en el **Monitor Serie**.
- Cuatro tareas periódicas con `millis()`/`micros()`, **sin `delay()`** en el ciclo principal.

## Material

| Componente | Notas |
|---|---|
| Arduino UNO R4 WiFi | Incluye matriz de LEDs de 12×8 |
| Módulo GY-521 (MPU-6050) | Acelerómetro y giroscopio, I2C |
| Módulo puente H L298N | Con el jumper de ENA retirado |
| Motorreductor DC con llanta | |
| Fuente externa para el motor | Conectada a +12V del L298N |
| Resistencia de 10 kΩ | Entre ENA y GND (recomendada) |
| Cables y cable USB-C | |

## Conexiones

| Señal | Arduino | Destino |
|---|---|---|
| ENA (PWM) | D9 | ENA del L298N |
| IN1 | D8 | IN1 del L298N |
| IN2 | D7 | IN2 del L298N |
| SDA | A4 | SDA del GY-521 |
| SCL | A5 | SCL del GY-521 |
| VCC / GND | 5V / GND | VCC / GND del GY-521 |
| AD0 | — | GND (fija la dirección I2C en 0x68) |

Notas importantes:

- Retirar el jumper de **ENA** del módulo L298N para poder controlar la velocidad con PWM.
- Alimentar el motor desde la **fuente externa**, nunca desde el pin de 5V del Arduino.
- Unir las tierras de la fuente, del L298N y del Arduino (**tierra común**).
- La resistencia de 10 kΩ entre ENA y GND mantiene el motor deshabilitado mientras el Arduino arranca o se reinicia.

## Uso

1. Armar el circuito según la tabla de conexiones.
2. Abrir el programa en el **Arduino IDE** y seleccionar la placa *Arduino UNO R4 WiFi*.
3. Cargar el programa.
4. Abrir el **Monitor Serie** a **115200 baudios**.
5. Mantener el sensor **quieto durante unos 5 segundos** mientras se calibra el giroscopio (500 muestras).
6. Cuando aparezca "Sistema listo", inclinar el sensor hacia adelante y hacia atrás.

Librerías utilizadas (incluidas con el entorno de la placa): `Wire` y `Arduino_LED_Matrix`.

## Cómo funciona

El motor responde al ángulo de inclinación así:

| Inclinación \|θ\| | Comportamiento |
|---|---|
| Menor a 5° | Motor detenido (zona muerta) |
| De 5° a 45° | PWM crece linealmente de 90 a 255 |
| 45° o más | PWM máximo (255) |

- El **signo** del ángulo define el sentido: positivo es adelante, negativo es reversa.
- Se usa un PWM mínimo de 90 porque con valores menores el motor no vence la fricción de sus engranes.
- La **rampa** cambia el PWM en pasos de 9 cada 20 ms (de 0 a 255 en unos 0.58 s). Al invertir el giro, el motor baja primero hasta cero y luego acelera en el otro sentido.

### Filtro complementario

```
ángulo = α · (ángulo + giro · dt) + (1 − α) · ángulo_acelerómetro
```

Con α = τ / (τ + dt) y τ = 0.5 s, que a 100 muestras por segundo da α ≈ 0.98. El giroscopio aporta la respuesta rápida y el acelerómetro corrige su deriva.

### Tareas periódicas

| Tarea | Periodo |
|---|---|
| Lectura del sensor y filtro | 10 ms |
| Rampa y control del motor | 20 ms |
| Matriz de LEDs | 50 ms |
| Monitor Serie | 500 ms |

### Estados del sistema

| Estado | Descripción |
|---|---|
| `BLOQUEADO` | El sensor no responde o falla la calibración al arrancar. Motor apagado; hay que revisar conexiones y pulsar RESET |
| `ACTIVO` | Operación normal |
| `PARO` | Falló una lectura o el intervalo entre lecturas superó 0.1 s. Motor detenido al instante; reintenta cada 250 ms |
| `RECUPERANDO` | El sensor volvió a responder; espera 100 ms y retoma el control conservando la calibración |

## Indicadores

**Matriz de LEDs**

- Marco completo: inclinación dentro de ±2°.
- Punto de 2×2 LEDs que sube o baja con el ángulo (llega al borde a 45°).
- X: sistema en paro o con falla inicial.

**Monitor Serie** (solo imprime cuando algo cambia). Ejemplo:

```
Inclinacion: adelante | 10 grados | leve | Motor: adelante | PWM: 44 %
Inclinacion: atras | -59 grados | fuerte | Motor: reversa | PWM: 74 %
Inclinacion: atras | -60 grados | fuerte | Motor: reversa | PWM: 100 %
```

La intensidad se clasifica como *leve* (menos de 15°), *moderada* (menos de 30°) o *fuerte*.

## Parámetros configurables

Están al inicio del programa como constantes:

| Constante | Valor | Descripción |
|---|---|---|
| `ZONA_MUERTA` | 5° | Rango donde el motor no se mueve |
| `ANGULO_MAXIMO` | 45° | Ángulo desde el que el PWM es máximo |
| `PWM_MINIMO` / `PWM_MAXIMO` | 90 / 255 | Velocidad mínima y máxima |
| `PASO_RAMPA` | 9 | Cambio de PWM cada 20 ms |
| `TAU_FILTRO` | 0.5 s | Constante de tiempo del filtro |
| `SENTIDO_PITCH` | 1.0 | Cambiar a `-1.0f` si el sentido queda invertido |

## Solución de problemas

- **"ERROR INICIAL: MPU-6050 no responde":** revisar SDA/SCL, la alimentación del sensor y que AD0 esté a GND. Pulsar RESET.
- **El motor gira al revés:** cambiar `SENTIDO_PITCH` a `-1.0f` o intercambiar los cables del motor en OUT1 y OUT2.
- **El motor no gira con inclinaciones pequeñas:** es normal dentro de la zona muerta (menos de 5°).
- **El motor se mueve solo al arrancar:** verificar la resistencia de 10 kΩ entre ENA y GND y la tierra común.
- **Aparece "PARO DE SEGURIDAD":** hay una falla de lectura del sensor, normalmente un cable suelto. El sistema se recupera solo cuando el sensor vuelve a responder.

## Aplicaciones

Vehículos autobalanceados, estabilizadores de cámara (gimbals) y controles por inclinación.
