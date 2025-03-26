**Mira bien el código. ¿Dónde está el marco motion 101?**

el marco motion 101 esta en donde se usan y actualizan los vectores y a la velocidad se le suma la aceleracion y a la posicion se le suma la velocidad.

**En la unidad anterior tu definías la aceleración mediante algún algoritmo. ¿Cuáles eran? Muestra ejemplos de código de la unidad anterior (solo la parte donde se define la aceleración).**

en la anterior la definimos como aceleracion constante, aceleracion aleatoria, y hacia el mouse.

- constante:
```this.acceleration = createVector(0, 0);```

- aleatoria:
```
this.acceleration = p5.Vector.random2D();
  this.acceleration.mult(0.5);
```

- hacia el mouse:
```
let dir = p5.Vector.sub(mouse, this.position);
dir.normalize();
dir.mult(0.2);
this.acceleration = dir;
```


**En esta unidad tu vas a calcular la aceleración. ¿Qué tiene que ver esto con las leyes de movimiento de Newton?**

El cálculo de la aceleración está directamente relacionado con la segunda ley de Newton: F = ma. pero como vamos a tomar la masa como 1, entonces la aceleracion es igual a la fuerza. por eso esta relacionada

