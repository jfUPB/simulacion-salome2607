**Explica brevemente cómo configuraste p5.sound y qué datos de audio estás extrayendo.**

- cargue la canción utilizando loadSound(), que permite cargar y controlar la reproducción de un archivo de audio.
```song = loadSound('Interstellar Main Theme - Hans Zimmer (1).mp3');```

- La función fft.analyze() realiza un análisis de la canción, obteniendo los espectros de frecuencia y la energía de diferentes bandas de frecuencia (bajos, medios, agudos).
- utilice amplitude.getLevel() para extraer el nivel de amplitud del audio, lo que representa la intensidad general del sonido en cada momento.

```js
// Análisis de frecuencias y amplitud
let spectrum = fft.analyze();  // Devuelve un array de 256 valores de frecuencia
let bass = fft.getEnergy("bass");  // Energía en las frecuencias bajas
let treble = fft.getEnergy("treble");  // Energía en las frecuencias altas
let level = amplitude.getLevel();  // Nivel de amplitud general del sonido
```

Estos datos extraídos del audio se utilizan para ajustar los parámetros del algoritmo generativo. Por ejemplo, la amplitud del sonido (level) controla la opacidad de las partículas, y las energías de las frecuencias (bass, treble, mid) afectan el color y el tamaño de las partículas.

**Muestra fragmentos de código clave donde los datos del audio modulan tu algoritmo generativo y donde el algoritmo controla los outputs visuales.**

- El color de las partículas cambia dependiendo de la energía de los bajos (bass), los agudos (treble) o los medios (mid).
- El tamaño de las partículas y su opacidad también dependen de la amplitud del sonido (amplitude.getLevel()).

```js
let hue, sat, bri;
if (bass > treble && bass > mid) {
  // Graves: tonos azules y morados
  hue = map(bass, 0, 255, 200, 280);
  sat = 100;
  bri = map(ampLevel, 0, 1, 40, 70);
} else if (treble > bass && treble > mid) {
  // Agudos: tonos blancos y dorados
  hue = map(treble, 0, 255, 40, 70);
  sat = map(treble, 0, 255, 60, 100);
  bri = map(ampLevel, 0, 1, 90, 100);
} else {
  // Medios: naranjas y rojos
  hue = map(mid, 0, 255, 0, 50);
  sat = 100;
  bri = map(ampLevel, 0, 1, 60, 90);
}
```

El número de partículas generadas depende del scroll, el scroll controla si salen mas o menos particulas

```js
function mouseWheel(event) {
  // Controlar el número de partículas nuevas por frame
  particleSpawnRate += event.delta > 0 ? -1 : 1;
  particleSpawnRate = constrain(particleSpawnRate, 0, 50); // limitarlo para no saturar
}
```

se generan destellos de particulas amarillas cuando se oprime el clic, se generan donde esta el mouse

```js
function mousePressed() {
  for (let i = 0; i < 20; i++) {
    particles.push(new Particle(true, createVector(mouseX, mouseY)));
  }
}
```

cuando se oprime la barra espaceadora hay un "momento de calma" y se baja el volumen de la cancion y la opacidad de las particulas

```js
function keyPressed() {
  if (key === ' ') {
    fadeMode = !fadeMode;
    song.setVolume(fadeMode ? 0.2 : 1.0); // Cambié 'volume' a 'setVolume'
  }
}
```

se ajusta la resolucion del flow field segun la amplitud

´´´js
// Ajustamos la complejidad del campo según la amplitud de los bajos y agudos
  let fieldResolution = map(bass + treble, 0, 510, 0.05, 0.2); // mayor volumen = campo más complejo
```


**Incluye el código completo de tu sketch final (o enlace a repositorio).**

```js
let song;
let fft, amplitude;
let particles = [];
let flowField = [];
let cols, rows;
let inc = 0.1;
let zoff = 0;
let scl = 20;
let particleLifespan = 20 * 60; // 20 segundos en frames
let muted = false;
let fadeMode = false;
let particleSpawnRate = 2; // controlado por scroll

function preload() {
  song = loadSound('Interstellar Main Theme - Hans Zimmer (1).mp3');
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  fft = new p5.FFT();
  amplitude = new p5.Amplitude();
  cols = floor(width / scl);
  rows = floor(height / scl);
  song.loop();
}

function draw() {
  background(0, 0, 0, 4); // Fondo con transparencia para dejar rastro
  
  // Generar partículas nuevas según el spawn rate
  for (let i = 0; i < particleSpawnRate; i++) {
    particles.push(new Particle());
  }

  let spectrum = fft.analyze();
  let bass = fft.getEnergy("bass");
  let treble = fft.getEnergy("treble");
  let mid = fft.getEnergy("mid");
  let level = amplitude.getLevel();
  let ampLevel = pow(level, 1.5) * 10;

  updateFlowField(bass, treble, mid);

  for (let p of particles) {
    p.follow(flowField);
    p.update();
    p.edges();
    p.show(bass, treble, ampLevel);
  }

  // Eliminar partículas muertas
  particles = particles.filter(p => !p.isDead());
}

function updateFlowField(bass, treble, mid) {
  flowField = [];
  let yoff = 0;
  
  // Ajustamos la complejidad del campo según la amplitud de los bajos y agudos
  let fieldResolution = map(bass + treble, 0, 510, 0.05, 0.2); // mayor volumen = campo más complejo
  
  for (let y = 0; y < rows; y++) {
    let xoff = 0;
    for (let x = 0; x < cols; x++) {
      let angle = noise(xoff, yoff, zoff) * TWO_PI * 4;
      let v = p5.Vector.fromAngle(angle);
      v.setMag(0.5);
      flowField[x + y * cols] = v;
      xoff += fieldResolution; // control de resolución
    }
    yoff += fieldResolution;
  }
  zoff += 0.01;
}

function mousePressed() {
  for (let i = 0; i < 20; i++) {
    particles.push(new Particle(true, createVector(mouseX, mouseY)));
  }
}

function keyPressed() {
  if (key === ' ') {
    fadeMode = !fadeMode;
    song.setVolume(fadeMode ? 0.2 : 1.0); // Cambié 'volume' a 'setVolume'
  }
}

function mouseWheel(event) {
  // Controlar el número de partículas nuevas por frame
  particleSpawnRate += event.delta > 0 ? -1 : 1;
  particleSpawnRate = constrain(particleSpawnRate, 0, 50); // limitarlo para no saturar
}

class Particle {
  constructor(golden = false, position = null) {
    this.pos = position || createVector(width / 2 + random(-10, 10), height / 2 + random(-10, 10));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxSpeed = 2;
    this.golden = golden;
    this.age = 0;
    this.lifespan = particleLifespan;
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.age++;
  }

  applyForce(force) {
    this.acc.add(force);
  }

  follow(vectors) {
    let x = floor(this.pos.x / scl);
    let y = floor(this.pos.y / scl);
    let index = x + y * cols;
    if (vectors[index]) {
      this.applyForce(vectors[index]);
    }
  }

  show(bass, treble, ampLevel) {
    noStroke();

    let mid = fft.getEnergy("mid");

    // Ajustar la opacidad según el nivel de amplitud
    let alpha = map(ampLevel, 0, 1, 20, 255); // Mayor volumen = mayor opacidad

    let hue, sat, bri;
    if (bass > treble && bass > mid) {
      // Graves: tonos azules y morados
      hue = map(bass, 0, 255, 200, 280);
      sat = 100;
      bri = map(ampLevel, 0, 1, 40, 70);
    } else if (treble > bass && treble > mid) {
      // Agudos: blancos y dorados
      hue = map(treble, 0, 255, 40, 70);
      sat = map(treble, 0, 255, 60, 100);
      bri = map(ampLevel, 0, 1, 90, 100);
    } else {
      // Medios: naranjas y rojos
      hue = map(mid, 0, 255, 0, 50);
      sat = 100;
      bri = map(ampLevel, 0, 1, 60, 90);
    }

    let sizePulse = map(bass + treble, 0, 510, 3, 12);
    if (this.golden) {
      fill(50, 100, 100, alpha); // Dorado
    } else {
      fill(hue, sat, bri, alpha);
    }

    ellipse(this.pos.x, this.pos.y, sizePulse, sizePulse);
  }

  edges() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }

  isDead() {
    return this.age > this.lifespan;
  }
}
```

**Inserta un VIDEO DEMO (si esta vez si, pero trata de optimizar el archivo) (¡esencial!) de tu simulación en acción, CON la música sonando. Debe mostrar la respuesta al audio y la naturaleza generativa.**

https://upbeduco-my.sharepoint.com/:v:/g/personal/salome_perezp_upb_edu_co/EQPATUvS8hRMl44l1VSzGLkBv85-cNKd2jPB2z9wqT-DLA?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=WJzsG5
