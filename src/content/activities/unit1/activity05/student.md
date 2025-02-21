**Código del ejemplo**

```
let xValues = [];

function setup() {
  createCanvas(640, 360);
  background(255);
  noStroke();
}

function draw() {
  // Generar un número aleatorio con distribución normal
  let x = randomGaussian(); 

  // Media y desviación estándar
  let mean = width / 2; // Media en el centro del canvas
  let sd = 60; // Desviación estándar
  
  // Ajustar el número a la distribución
  x = x * sd + mean;

  // Almacenar los valores generados
  xValues.push(x);

  // Visualización de la distribución usando elipses
  background(255); // Limpiar la pantalla cada vez
  fill(0, 150, 255, 50);

  // Dibujar círculos en las posiciones generadas
  for (let i = 0; i < xValues.length; i++) {
    ellipse(xValues[i], height / 2, 16, 16); 
  }
}
```

**Captura de pantalla**

![Captura de Pantalla 2025-01-23 a la(s) 6 39 59 p  m](https://github.com/user-attachments/assets/c7629ff9-0bd2-414f-ac4e-37d2ca1d404a)


**Una breve explicación de cómo se refleja la distribución normal en la visualización.**

use randomGaussian() que genera números aleatorios siguiendo una distribución normal (gaussiana) con una media de 0 y una desviación estándar de 1.

Dibujamos círculos en la posición x, que representa los números generados por la distribución normal. La visualización refleja la distribución normal con una mayor concentración de círculos cerca del centro del canvas, donde se encuentra la media. Esto significa que la mayoría de los valores generados por randomGaussian() se agrupan alrededor de la media, y hay menos valores más alejados, siguiendo la forma de la campana de Gauss.
