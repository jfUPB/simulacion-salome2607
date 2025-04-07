**Elige uno de los algoritmos estudiados (Flow Fields o Flocking) y úsalo como base para una pieza de arte generativo interactiva. El desafío principal es aplicar el algoritmo a un contexto visual o conceptual inesperado, diferente a las típicas simulaciones de fluidos o criaturas. ¡Piensa fuera de la caja!**

> Elige un algoritmo: decide si trabajarás con Flow Fields o Flocking.
> Conceptualiza la aplicación inesperada: brainstorming: ¿Cómo puedes usar la lógica de ese algoritmo para representar algo diferente? ¿Qué fenómeno visual, natural o abstracto podrías simular de forma no convencional? Define tu concepto.
> Diseña la implementación:
> ¿Cómo adaptarás el algoritmo base a tu nuevo concepto? ¿Qué representarán los “agentes”? ¿Qué significará el “campo” o las “reglas de enjambre” en tu contexto?
> ¿Qué aspecto visual tendrán tus agentes o el resultado del proceso?
> ¿Qué tipo de interacción incluirás? (ej: el mouse influye en el flow field, un clic añade/quita agentes, teclas cambian parámetros, micrófono, etc).
> Implementa tu sketch: escribe el código en p5.js. Empieza adaptando el código base del algoritmo elegido e introduce gradualmente los cambios para tu concepto, los nuevos visuales y la interacción.
> Prueba y refina: ejecuta, prueba la interacción y refina los parámetros y el aspecto visual hasta que logres una pieza coherente con tu concepto inesperado.

**Indica claramente qué algoritmo elegiste (Flow Fields o Flocking).**

voy a escoger el de flocking

**Describe tu concepto de aplicación inesperada: ¿Qué estás representando o simulando de forma no convencional y por qué?**

quisiera hacer como que el flocking y lo boids representen pensamientos o ideas dentro de la mente. Cada uno tiene un color según su emoción. Las reglas del Flocking representan cómo se agrupan los pensamientos similares. Esto representa cómo nuestras ideas se agrupan, cómo interactúan entre sí y cómo algunas se vuelven más conscientes (cuando el usuario las toca con el cursor), mientras otras simplemente desaparecen con el tiempo. Ademas de cuando tenemos momentos de claridad y las ideas se dispersan. 

**Explica brevemente cómo adaptaste la lógica del algoritmo a tu concepto.** 

Adapté el algoritmo de flocking para que los boids solo interactúen con otros que comparten la misma emoción, creando pequeñas comunidades mentales que reflejan cómo los pensamientos similares tienden a agruparse. Además, implementé un sistema de lifespan, de modo que cada boid tiene una vida útil limitada y eventualmente desaparece, como ocurre con pensamientos pasajeros. 

**Describe la interacción que implementaste.**

- Al arrastrar el mouse, genera nuevos pensamientos (boids) de la emoción actual.
- Puede cambiar la emoción de los boids generados presionando las teclas del 1 al 5.
- Al pasar el cursor sobre un boid, este se vuelve más grande, simbolizando que ese pensamiento se ha vuelto consciente.
- Al presionar la barra espaciadora, todos los boids cercanos se dispersan en una explosión, representando un momento de claridad o sobrecarga mental.

**Incluye el código completo de tu sketch p5.js y el enlace al editor de p5.js.**

```js
let flock;
let previousMouse;
let explosion = false;
let currentEmotionIndex = 0;

let emotions = [
  { name: "alegría", color: [255, 204, 0] },
  { name: "tristeza", color: [0, 102, 204] },
  { name: "ira", color: [204, 0, 0] },
  { name: "calma", color: [102, 204, 153] },
  { name: "miedo", color: [153, 51, 255] }
];

function setup() {
  createCanvas(640, 240);
  flock = new Flock();
  previousMouse = createVector(mouseX, mouseY);

  for (let i = 0; i < 120; i++) {
    let boid = new Boid(width / 2, height / 2, random(emotions));
    flock.addBoid(boid);
  }

  // Instrucciones debajo del canvas
  createP(
    "💡 Arrastra el mouse para generar pensamientos.<br>" +
    "🌟 Pasa el cursor sobre uno para hacerlo consciente.<br>" +
    "💥 Presiona espacio para crear una explosión de claridad.<br>" +
    "🎨 Presiona 1–5 para cambiar la emoción que generas:<br>" +
    "1: alegría, 2: tristeza, 3: ira, 4: calma, 5: miedo."
  );
}

function draw() {
  background(255, 255, 255, 25);
  flock.run();

  if (explosion) {
    flock.explode(mouseX, mouseY);
    explosion = false;
  }
}

function mouseDragged() {
  let mouseDir = createVector(mouseX - previousMouse.x, mouseY - previousMouse.y);
  let emotion = emotions[currentEmotionIndex];
  let newBoid = new Boid(mouseX, mouseY, emotion);
  newBoid.velocity = mouseDir.setMag(random(1, 3));
  flock.addBoid(newBoid);
  previousMouse.set(mouseX, mouseY);
}

function keyPressed() {
  if (key === ' ') {
    explosion = true;
  }

  if (key >= '1' && key <= '5') {
    currentEmotionIndex = parseInt(key) - 1;
  }
}

class Boid {
  constructor(x, y, emotion) {
    this.acceleration = createVector(0, 0);
    this.velocity = p5.Vector.random2D();
    this.position = createVector(x, y);
    this.r = 3.0;
    this.maxspeed = 3;
    this.maxforce = 0.05;
    this.lifespan = 500;
    this.emotion = emotion;
  }

  run(boids) {
    this.flock(boids);
    this.update();
    this.borders();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  flock(boids) {
    let sameEmotion = boids.filter(b => b.emotion.name === this.emotion.name);
    let sep = this.separate(sameEmotion);
    let ali = this.align(sameEmotion);
    let coh = this.cohere(sameEmotion);
    sep.mult(1.5);
    ali.mult(1.0);
    coh.mult(1.0);
    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 1;
  }

  show() {
    let angle = this.velocity.heading();
    let d = dist(mouseX, mouseY, this.position.x, this.position.y);
    let isHovered = d < 30;
    let sizeMultiplier = isHovered ? 2.5 : 1;

    let col = this.emotion.color;
    let alpha = map(this.lifespan, 0, 255, 0, 255);
    fill(col[0], col[1], col[2], alpha);
    stroke(1000, alpha);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    beginShape();
    vertex(this.r * 2 * sizeMultiplier, 0);
    vertex(-this.r * 2 * sizeMultiplier, -this.r * sizeMultiplier);
    vertex(-this.r * 2 * sizeMultiplier, this.r * sizeMultiplier);
    endShape(CLOSE);
    pop();
  }

  borders() {
    if (this.position.x < -this.r) this.position.x = width + this.r;
    if (this.position.y < -this.r) this.position.y = height + this.r;
    if (this.position.x > width + this.r) this.position.x = -this.r;
    if (this.position.y > height + this.r) this.position.y = -this.r;
  }

  seek(target) {
    let desired = p5.Vector.sub(target, this.position);
    desired.normalize();
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    return steer;
  }

  separate(boids) {
    let desiredSeparation = 25;
    let steer = createVector(0, 0);
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.position, other.position);
      if (d > 0 && d < desiredSeparation) {
        let diff = p5.Vector.sub(this.position, other.position);
        diff.normalize();
        diff.div(d);
        steer.add(diff);
        count++;
      }
    }
    if (count > 0) {
      steer.div(count);
    }
    if (steer.mag() > 0) {
      steer.normalize();
      steer.mult(this.maxspeed);
      steer.sub(this.velocity);
      steer.limit(this.maxforce);
    }
    return steer;
  }

  align(boids) {
    let neighborDist = 50;
    let sum = createVector(0, 0);
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.position, other.position);
      if (d > 0 && d < neighborDist) {
        sum.add(other.velocity);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      sum.normalize();
      sum.mult(this.maxspeed);
      let steer = p5.Vector.sub(sum, this.velocity);
      steer.limit(this.maxforce);
      return steer;
    } else {
      return createVector(0, 0);
    }
  }

  cohere(boids) {
    let neighborDist = 50;
    let sum = createVector(0, 0);
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.position, other.position);
      if (d > 0 && d < neighborDist) {
        sum.add(other.position);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      return this.seek(sum);
    } else {
      return createVector(0, 0);
    }
  }

  explodeFrom(x, y) {
    let center = createVector(x, y);
    let force = p5.Vector.sub(this.position, center);
    force.setMag(2.5);
    this.applyForce(force);
  }
}

class Flock {
  constructor() {
    this.boids = [];
  }

  run() {
    for (let i = this.boids.length - 1; i >= 0; i--) {
      let boid = this.boids[i];
      boid.run(this.boids);
      if (boid.lifespan <= 0) {
        this.boids.splice(i, 1);
      }
    }
  }

  addBoid(b) {
    this.boids.push(b);
  }

  explode(x, y) {
    for (let boid of this.boids) {
      boid.explodeFrom(x, y);
    }
  }
}
```

**Inserta una captura de pantalla o un enlace. UN ENLACE. Si un ENLACE. De nuevo, un ENLACE a un GIF animado o video de tu pieza final en acción.**

https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExaXo2MnZqbXg3a3M5bnhuMGIzY2tiMmZhaXFlZG9xYzR1NHpxZzF6dCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/kauxoXtcswNeew1JMw/giphy.gif
