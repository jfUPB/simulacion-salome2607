**Describe el experimento que vas a realizar.**

quiero explorar cómo se ve afectado el comportamiento del movimiento al modificar la magnitud del desplazamiento en cada paso, es decir, aumentar el tamaño del movimiento por paso.

**¿Qué pregunta quieres responder con este experimento?**

¿Qué pasaría si aumentara el tamaño del desplazamiento en cada paso de la caminata aleatoria?

**¿Qué resultados esperas obtener?**

Espero lograr que me funcione. Si aumento el tamaño del desplazamiento en cada paso, espero que la caminata se expanda más rápido en el espacio.

**¿Qué resultados obtuviste?**

- Añadí una variable stepSize en el constructor de la clase Walker, que controla el tamaño del paso. Inicialmente, esta variable está configurada con un valor de 1.
- En la función step(), en lugar de incrementar o decrementar las coordenadas x e y por 1 unidad, ahora se incrementan o decrementan por el valor de this.stepSize. Luego, al final de cada iteración, el tamaño del paso (this.stepSize) se incrementa en 1.

Logre que se incrementara el tamaño del desplazamiento entonces el dibujo se vuelve mas ampplio rapidamente

![Captura de Pantalla 2025-01-23 a la(s) 6 22 55 p  m](https://github.com/user-attachments/assets/4b966d85-5d21-4e6c-b19e-e5c213aec79f)

```
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.stepSize = 1; // Inicialmente, el tamaño del paso es 1
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4)); // Escoge una dirección aleatoria

    if (choice == 0) {
      this.x += this.stepSize; // Mueve hacia la derecha
    } else if (choice == 1) {
      this.x -= this.stepSize; // Mueve hacia la izquierda
    } else if (choice == 2) {
      this.y += this.stepSize; // Mueve hacia abajo
    } else {
      this.y -= this.stepSize; // Mueve hacia arriba
    }

    // Aumenta el tamaño del paso en 5 después de cada iteración
    this.stepSize += 1;
  }
}
```

**¿Qué aprendiste de este experimento?**

En este experimento aprendí cómo el tamaño del paso en una caminata aleatoria afecta mucho el movimiento visual. Al aumentar el paso en 1 unidad en cada iteración, pude ver cómo el "walker" se desplaza más rápido y cubre mayor distancia con el tiempo. Este cambio muestra cómo pequeños ajustes en el código pueden generar grandes diferencias en la animación, dándome más control sobre el comportamiento y la dinámica del movimiento en proyectos futuros.
