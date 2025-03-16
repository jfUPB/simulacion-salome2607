**diseña e implementa una obra de arte generativa algorítmica interactiva en tiempo real en p5.js que cumpla con los siguientes requisitos:**

- **Selecciona uno de los conceptos con los que experimentaste en la fase de investigación y propón la obra alrededor de este.**
- **La obra debe ser interactiva en tiempo real. Puedes usar teclado, mouse, música, el micrófono, video, sensor o cualquier otro dispositivo de entrada.**
- **Documenta el proceso de creación, incluyendo la idea inicial, bocetos, experimentación con el código y el resultado final.**

---------------------
quiero aplicar el conceptos de ondas sinuidales y de rotacion con angulos. 

Mi idea es hacer una simulacion de ondas y poder cambiar los parametros con las teclas y el mouse.

Primero lo hice que fueran muchas ondas y que si muevo el mouse se cambia la frecuencia y que si hundo unas teclas se incrementa o disminuye la amplitud. 

Pero luego queria tambien hacer que rotaran y que se puedieran agregar mas ondas. entonces hice que apareciera solo una unda, que si doy clic sale otra con otros parametros diferentes random, que si presiono las teclas de felchas de los lados se rotan las ondas, y que si presiono las flechas de arriba y abajo se incrementa y disminuye la amplitud de todas las ondas. 


**Enlace a tu obra en el editor de p5.js.**

https://editor.p5js.org/salome2607/sketches/BDrscfvF5

**El código.**

```js
let waves = [];
let angle = 0; // Ángulo de rotación para las ondas

function setup() {
  createCanvas(windowWidth, windowHeight);
  // Crear la primera onda al inicio
  waves.push(new Wave(height / 10, 100, 0.05, 0, color(random(255), random(255), random(255)))); 
}

function draw() {
  background(0);

  // Dibujar y actualizar cada onda
  push();
  translate(width / 2, height / 2); // Centramos las ondas en el canvas
  rotate(angle); // Aplicamos la rotación

  for (let i = 0; i < waves.length; i++) {
    waves[i].show();
    waves[i].update();
  }

  pop();
}

// Controlar la amplitud y rotación con las teclas de flecha
function keyPressed() {
  if (keyCode === UP_ARROW) {
    // Incrementar la amplitud de todas las ondas
    for (let i = 0; i < waves.length; i++) {
      waves[i].amplitude += 10;
    }
  }
  if (keyCode === DOWN_ARROW) {
    // Disminuir la amplitud de todas las ondas
    for (let i = 0; i < waves.length; i++) {
      if (waves[i].amplitude > 10) {
        waves[i].amplitude -= 10;
      }
    }
  }
  if (keyCode === LEFT_ARROW) {
    // Rotar hacia la izquierda
    angle -= PI / 16;
  }
  if (keyCode === RIGHT_ARROW) {
    // Rotar hacia la derecha
    angle += PI / 16;
  }
}

// Crear una nueva onda al hacer clic
function mousePressed() {
  let newWaveY = random(-height / 4, height / 4); // Posición vertical aleatoria
  let newWaveColor = color(random(255), random(255), random(255)); // Color aleatorio
  waves.push(new Wave(newWaveY, random(50, 150), random(0.01, 0.05), random(TWO_PI), newWaveColor));
}

// Clase para las ondas
class Wave {
  constructor(amplitude, wavelength, frequency, phase, waveColor) {
    this.amplitude = amplitude;
    this.wavelength = wavelength;
    this.frequency = frequency;
    this.phase = phase;
    this.waveColor = waveColor; // Color de la onda
  }

  update() {
    this.phase += this.frequency; // La onda se mueve
  }

  show() {
    noFill();
    stroke(this.waveColor); // Color personalizado
    beginShape();
    for (let x = -width / 2; x < width / 2; x++) { // Cambiamos para ajustar a la traslación
      let y = this.amplitude * sin((TWO_PI / this.wavelength) * x + this.phase); // Función seno para la onda
      vertex(x, y); // Centramos la onda en el medio
    }
    endShape();
  }
}
```

**Una captura de pantalla con una imagen de tu obra.**

![Foto](../../../../assets/uni4/act11.gif)
