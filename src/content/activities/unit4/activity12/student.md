**¿Qué concepto de oscilación utilizaste como base para tu obra? Describe cómo lo implementaste.**

El concepto de oscilación que utilice como base para esta obra es la oscilación armónica simple, representada a través de ondas sinusoidales.

Implementé este concepto utilizando la función sin() de p5.js para calcular la posición vertical de cada punto a lo largo de la onda, mientras que los parámetros de amplitud, frecuencia, y fase se utilizan para controlar su forma y comportamiento.

**¿Cómo funciona la interacción en tu obra? Explica cómo el usuario puede modificar la obra en tiempo real.**

La interacción en la obra es con el uso del mouse y las teclas de flecha. Con esto se puede:

- Agregar ondas: Cada vez que se hace clic con el mouse, se genera una nueva onda con un color, amplitud y posición diferentes.
- Rotar las ondas: Usando las flechas izquierda y derecha, se puede rotar todas las ondas alrededor del centro de la pantall.
- Modificar la amplitud: Con las flechas arriba y abajo, se puede aumentar o disminuir la amplitud de todas las ondas en tiempo real.

**¿Qué desafíos encontraste durante el proceso de creación? ¿Cómo los superaste?**

Uno de los principales desafíos fue lograr que cada nueva onda no se superpusiera directamente sobre las anteriores. Para resolver esto, agregué una variación en la posición vertical de cada nueva onda.

Otro desafío fue gestionar los cambios en la amplitud de múltiples ondas simultáneamente. Esto se solucionó asegurando que cada onda tuviera su propia variable de amplitud, pero controlando todas con un solo valor de entrada para las flechas arriba/abajo.

**¿Qué aprendiste sobre las oscilaciones y su aplicación en el arte generativo?**

Aprendí cómo los parametros de las oscilaciones, como la frecuencia, amplitud y fase, pueden traducirse en herramientas creativas para generar obras visuales. Las oscilaciones no solo describen el comportamiento físico de las ondas, sino que también pueden usatse para crear patrones visuales que interactúan entre sí de formas interesantes.
