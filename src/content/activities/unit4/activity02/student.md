**PRIMERA SIMULACION**


**¿Qué está pasando en esta simulación? ¿Cuál es la interacción?**

en la simulacion hay una linea con dos circulos a los lados. la interaccion es que la linea rota si se presiona alguna tecla. o sea si se presiona la tecla el angulo aumenta en 0.1

**Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?**

se hace para que el eje de rotacion sea en el centro de la pantalla, esto permite que los objetos giren alrededor del centro de la pantalla en lugar de hacerlo desde la esquina superior izquierda (que es el origen predeterminado del sistema de coordenadas). 

**Cuál es la relación entre el sistema de coordenadas y la función rotate().**

La función rotate() aplica una rotación a lo que esta en el sistema de coordenadas actual. Cuando se traslada el origen al centro de la pantalla, la función rotate() hace que los objetos roten alrededor de ese nuevo origen. La relación entre el sistema de coordenadas y rotate() es que el punto de rotación depende del origen del sistema de coordenadas; al trasladar el origen, se cambia el punto alrededor del cual los objetos rotan.

**Observa que al dibujar los elementos gráficos parece que se está dibujando en la posición (0, 0) del sistema de coordenadas. ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?**

Los elementos parecen estar dibujándose en la posición (0, 0) del sistema de coordenadas porque, después de trasladar el origen del sistema al centro de la pantalla, todos los objetos se dibujan con coordenadas relativas a ese nuevo origen. 

Los elementos gráficos parecen rotar porque la función rotate() se está aplicando antes de dibujar cada frame. Esta función rota el sistema de coordenadas alrededor del origen. Aunque las coordenadas de los objetos no cambian lo que está rotando es el sistema de coordenadas entero debido a la aplicación de rotate(). Cada vez que rotate() incrementa el ángulo de rotación, el sistema de coordenadas se gira, lo que hace que los elementos gráficos parezcan girar alrededor del centro, aunque en realidad sus posiciones relativas al origen del sistema no cambian.

**SEGUNDA SIMULACION**

**Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?**

En este caso se aplica el motion 101, usando los vectores de velocidad y posicion para simular el movimiento del objeto. ademas se usan para calcular la orientacion del movimiento par aque siga al mouse.

**¿Qué hace la función heading()?**

Esta función calcula el ángulo de dirección (en radianes) del vector de velocidad con respecto al eje X. heading() dice hacia qué dirección (en ángulos) apunta un vector

Esto es útil para orientar objetos gráficos en la dirección del movimiento, ya que devuelve el ángulo que forma el vector con el eje horizontal, lo que permite usarlo en la función rotate() para hacer que el objeto apunte en esa dirección.

**¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.**

- push() guarda el estado actual del sistema de coordenadas (incluyendo traslaciones, rotaciones y escalas).
- pop() restaura el estado del sistema de coordenadas al momento en que se llamó push().

Esto permite hacer transformaciones (como traslaciones y rotaciones) de forma aislada para un objeto específico sin afectar a otros objetos que se dibujan en la pantalla. En este caso, push() y pop() aseguran que las rotaciones y traslaciones solo afecten al objeto actual y no al resto de la escena.

**¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.**

La función rectMode(CENTER) cambia el modo en el que se dibuja un rectángulo. De forma predeterminada, los rectángulos se dibujan desde la esquina superior izquierda usando las coordenadas (x, y) como referencia. Con rectMode(CENTER), el rectángulo se dibuja usando su centro como el punto de referencia para (x, y).

**¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.**

El ángulo de rotación está directamente relacionado con la dirección del vector de velocidad. El vector de velocidad indica hacia dónde se mueve el objeto, y heading() calcula el ángulo que forma ese vector con respecto al eje X. Luego, rotate(angle) usa ese ángulo para rotar el objeto, de manera que apunte hacia la dirección del movimiento.
