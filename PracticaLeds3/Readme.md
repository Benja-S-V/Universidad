# Paradigmas de Ejecución: delay() vs millis()

## Descripción
Esta práctica compara dos paradigmas de ejecución en Arduino para el control simultáneo 
de múltiples LEDs mediante el Arduino UNO R4 WiFi.

La Parte 1 implementa el parpadeo de tres LEDs usando `delay()`, evidenciando el 
comportamiento bloqueante de este enfoque. La Parte 2 reimplementa la misma lógica 
usando `millis()`, logrando que cada LED parpadee de forma verdaderamente independiente 
y simultánea.

## Objetivos

* Identificar el comportamiento bloqueante de `delay()` en sistemas con múltiples tareas.
* Implementar temporización no bloqueante con `millis()` para tres LEDs con distintos intervalos.
* Comparar ambos enfoques en términos de concurrencia, escalabilidad y respuesta del sistema.

## Herramientas y material utilizado

* Arduino UNO R4 WiFi.
* Arduino IDE.
* 3 LEDs de distintos colores.
* 3 resistencias de 220Ω.
* Protoboard.
* Cables de conexión (jumpers).

## Diagrama
El diagrama muestra las conexiones utilizadas para implementar el control de los tres LEDs.

![Diagrama de conexiones](Diagrama/Diagrama%20Leds.png)

### Evidencia del circuito físico

![Circuito con delay](Diagrama/Leds%20Delay.jpeg)

![Circuito con millis](Diagrama/Leds%20Milic.png)

## Código
La Parte 1 implementa el parpadeo con `delay()`, produciendo una ejecución secuencial 
y bloqueante. La Parte 2 usa `millis()` para lograr una temporización no bloqueante 
donde cada LED maneja su propio intervalo de forma independiente.

[Ver código](https://github.com/Benja-S-V/Universidad/blob/main/PracticaLeds3/Codigo)

## Reporte
El reporte contiene la comparación entre ambos paradigmas, la metodología utilizada, 
el análisis de los resultados y las conclusiones obtenidas durante la práctica.

[Ver Reporte](https://github.com/Benja-S-V/Universidad/blob/main/PracticaLeds3/Reporte)

## Resultados
En la Parte 1, los tres LEDs se encendieron y apagaron de forma secuencial debido al 
comportamiento bloqueante de `delay()`. El ciclo completo acumuló un total de 6000 ms, 
haciendo que LED1 no parpadeara cada 500 ms como se pretendía, sino hasta que los otros 
dos LEDs terminaran su propio ciclo.

En la Parte 2, los tres LEDs parpadearon de forma simultánea e independiente, cada uno 
respetando su propio intervalo. Además, se imprimió "Hola Mundo" en el Monitor Serial 
cada 3000 ms como cuarta tarea concurrente, sin afectar el parpadeo de ningún LED.

[Ver carpeta Resultados](https://github.com/Benja-S-V/Universidad/blob/main/PracticaLeds3/Resultados)

## Video

Parte 1 — delay():
[Ver video](https://youtube.com/shorts/LCgG4536u_I)

Parte 2 — millis():
[Ver video](https://youtube.com/shorts/VZnIhfNHM9s)

[Ver carpeta Video](https://github.com/Benja-S-V/Universidad/blob/main/PracticaLeds3/Video)

## Conclusiones
La práctica permitió comprobar de forma directa la diferencia entre el paradigma bloqueante 
de `delay()` y el no bloqueante de `millis()`. Mientras que `delay()` impide que el 
microcontrolador realice cualquier otra tarea durante la espera, `millis()` permite que 
múltiples tareas se ejecuten de forma concurrente sin interferirse entre sí.

El uso de `millis()` resultó indispensable para lograr el comportamiento originalmente 
buscado, y representa una solución escalable aplicable en cualquier proyecto que requiera 
manejar más de una temporización al mismo tiempo.
