**En qué consiste el concepto de Lévy flight y en qué caso sería interesante usarlo.**

Un Lévy flight es un tipo de movimiento aleatorio en el que los pasos que da un objeto no son de tamaño constante. En lugar de moverse una distancia fija en cada paso, los saltos en un Lévy flight siguen una distribución de probabilidad con una cola pesada, lo que significa que los saltos más largos son mucho más probables que en un movimiento aleatorio convencional. 

Un Lévy flight es interesante de usar cuando queremos simular comportamientos que no siguen un patrón regular, sino que implican un movimiento errático con grandes saltos esporádicos. Es útil para modelar situaciones donde un objeto o entidad se mueve de manera impredecible pero aún dentro de un marco general de búsqueda o exploración 

**Código de la simulación.**

```
let x, y;

function setup() {
  createCanvas(640, 480);
  x = width / 2;
  y = height / 2;
  background(255);
  frameRate(30);
}

function draw() {
  let stepLength = randomLevyStep(); // Generamos un salto de Lévy
  let angle = random(TWO_PI); // Ángulo aleatorio para la dirección del paso
  
  // Calculamos las nuevas coordenadas
  x += stepLength * cos(angle);
  y += stepLength * sin(angle);

  // Asegurarnos de que el "walker" se quede dentro del canvas
  x = constrain(x, 0, width);
  y = constrain(y, 0, height);

  // Dibujar el punto en la nueva posición
  stroke(0);
  point(x, y);
}

function randomLevyStep() {
  let exponent = 1.5; // Exponente para la distribución de Lévy
  let step = pow(random(), -1 / exponent); // Generamos el paso según la distribución Lévy
  return step * 10; // Escalar el paso para hacerlo visible
}
```


**Captura de pantalla.**

![Captura de Pantalla 2025-01-23 a la(s) 6 40 09 p  m](https://github.com/user-attachments/assets/877f65b6-1d1c-4262-9f7f-b65ff376ac56)
