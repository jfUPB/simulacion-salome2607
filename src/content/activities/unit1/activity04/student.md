**Explicación de la diferencia entre distribuciones uniformes y no uniformes.**

> Values from a normal distribution cluster around a central value called the mean

En la distribucion uniforme cada resultado tiene la misma probabilidad de ocurrir. Mientras que en la no uniforme algunos resultados tienen más probabilidad de ocurrir que otros. 

**Código modificado y captura de pantalla de la caminata con distribución no uniforme.**

puedo aumentar la probabilidad de que camine hacia la derecha

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
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = random(1); // Genera un número aleatorio entre 0 y 1

    // Distribución no uniforme
    if (choice < 0.5) { // 50% de probabilidad de moverse hacia la derecha
      this.x++;
    } else if (choice < 0.75) { // 25% de probabilidad de moverse hacia la izquierda
      this.x--;
    } else if (choice < 0.875) { // 12.5% de probabilidad de moverse hacia abajo
      this.y++;
    } else { // 12.5% de probabilidad de moverse hacia arriba
      this.y--;
    }
  }
}
```
![Captura de Pantalla 2025-01-23 a la(s) 6 31 28 p  m](https://github.com/user-attachments/assets/57f5ea6e-731f-407f-938a-68ec6dd2160f)

se puede ver como camina hacia la derecha mucho mas.
