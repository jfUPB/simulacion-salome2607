 **el código  y enlace de cada una de las simulaciones y un texto donde expliques cómo modelaste cada fuerza.**

 - friccion:

mi experimento consiste en tres bolitas en tres diferentes superficies con diferentes fricciones. las bolitas se pueden arrastrar en la superficie con el mouse y segun el coeficiente de friccion de cada superficie la bolita se arrastra mas facil o mas dificil.

```js
let movers = [];
let surfaces = [];

function setup() {
  createCanvas(640, 240);
  createP('Arrastra las bolitas con el mouse. Cada superficie tiene diferente fricción.');

  // Crear superficies con diferentes fricciones
  surfaces.push(new Surface(0, 0, width, height / 3, 0.0001, 'green'));   // Baja fricción
  surfaces.push(new Surface(0, height / 3, width, height / 3, 0.001, 'blue'));   // Media fricción
  surfaces.push(new Surface(0, 2 * height / 3, width, height / 3, 0.01, 'red'));   // Alta fricción

  // Crear un Mover en cada superficie
  for (let i = 0; i < surfaces.length; i++) {
    let yPosition = surfaces[i].y + surfaces[i].h / 2;
    movers.push(new Mover(width / 2, yPosition, 5));
  }
}

function draw() {
  background(255);

  // Dibujar superficies
  for (let surface of surfaces) {
    surface.display();
  }

  // Aplicar fuerzas y dibujar bolitas
  for (let i = 0; i < movers.length; i++) {
    let mover = movers[i];
    let surface = surfaces[i];

    // Aplicar fricción dependiendo de la superficie
    if (surface.contains(mover)) {
      let friction = surface.calculateFriction(mover);
      mover.applyForce(friction);
    }

    mover.update();
    mover.show();

    // Si el mouse está presionado y está sobre la bolita, la bolita sigue al mouse
    if (mouseIsPressed && mover.isMouseOver()) {
      let mouseForce = createVector(mouseX - mover.position.x, 0); // Movimiento solo horizontal
      mouseForce.setMag(0.5); // Ajustar la magnitud
      mover.applyForce(mouseForce);
    }

    mover.bounceEdges(); // Rebota en los bordes
  }
}

class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127, 127);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  isMouseOver() {
    let distance = dist(mouseX, mouseY, this.position.x, this.position.y);
    return distance < this.radius;
  }

  bounceEdges() {
    let bounce = -0.9;
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= bounce;
    } else if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= bounce;
    }
  }
}

class Surface {
  constructor(x, y, w, h, mu, color) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.mu = mu; // Coeficiente de fricción
    this.color = color;
  }

  contains(mover) {
    let pos = mover.position;
    return pos.x > this.x && pos.x < this.x + this.w && pos.y > this.y && pos.y < this.y + this.h;
  }

  calculateFriction(mover) {
    let diff = mover.velocity.copy();
    diff.normalize();
    diff.mult(-1); // Dirección opuesta al movimiento
    let normal = mover.mass;
    let frictionMag = this.mu * normal;
    diff.setMag(frictionMag);
    return diff;
  }

  display() {
    fill(this.color);
    rect(this.x, this.y, this.w, this.h);
  }
}
```

link-> https://editor.p5js.org/salome2607/full/FkXHq3Eiq

 - Resistencia del aire y de fluidos:

mi experimento consiste en que hay varias bolitas de diferentes masas y caen a traves de diferentes liquidos. primero caen por el aire, luego a agua y luego a miel. asi se demuestran las diferentes resistencias segun donde esta y segun el liquido, afectando como se mueven las bolitas. 

```js
let movers = [];
let liquids = [];

function setup() {
  createCanvas(640, 480);

  // Inicializa un array de objetos Mover.
  for (let i = 0; i < 9; i++) {
    // Usa una masa aleatoria para cada uno.
    let mass = random(0.5, 4);
    movers[i] = new Mover(40 + i * 70, 0, mass);
  }

  // Creamos 3 líquidos con diferentes coeficientes de resistencia y colores
  liquids.push(new Liquid(0, 0, width, height / 3, 0.01, color(135, 206, 250)));  // Aire (poca resistencia, color azul claro)
  liquids.push(new Liquid(0, height / 3, width, height / 3, 0.05, color(100, 149, 237)));  // Agua (resistencia moderada, color azul)
  liquids.push(new Liquid(0, (height / 3) * 2, width, height / 3, 0.1, color(255, 165, 0)));  // Miel (alta resistencia, color naranja)
}

function draw() {
  background(255);

  // Dibujar los líquidos.
  for (let i = 0; i < liquids.length; i++) {
    liquids[i].show();
  }

  // Actualizamos el movimiento de cada objeto Mover
  for (let i = 0; i < movers.length; i++) {
    for (let j = 0; j < liquids.length; j++) {
      // Si el objeto está dentro de un líquido, aplicamos la fuerza de arrastre.
      if (liquids[j].contains(movers[i])) {
        let dragForce = liquids[j].drag(movers[i]);
        movers[i].applyForce(dragForce);
      }
    }

    // Aplicar la gravedad (escalada por la masa del objeto).
    let gravity = createVector(0, 0.1 * movers[i].mass);
    movers[i].applyForce(gravity);

    // Actualizar y mostrar los movimientos.
    movers[i].update();
    movers[i].show();
    movers[i].checkEdges();
  }
}

// Clase para los objetos que se mueven.
class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = this.mass * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);  // Resetear aceleración
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127);
    ellipse(this.position.x, this.position.y, this.radius * 2);
  }

  checkEdges() {
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= -0.9; // Rebote en el borde inferior
    }
  }
}

// Clase para los líquidos.
class Liquid {
  constructor(x, y, w, h, c, col) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c;  // Coeficiente de arrastre
    this.col = col;  // Color del líquido
  }

  contains(mover) {
    // Verificar si el objeto está dentro del líquido.
    let l = mover.position;
    return l.x > this.x && l.x < this.x + this.w &&
           l.y > this.y && l.y < this.y + this.h;
  }

  drag(mover) {
    // Fuerza de arrastre = -0.5 * c * v^2 * área
    let speed = mover.velocity.mag();
    let dragMagnitude = this.c * speed * speed;

    // Dirección de la fuerza de arrastre es opuesta a la velocidad.
    let dragForce = mover.velocity.copy();
    dragForce.mult(-1);

    // Escalar por la magnitud calculada.
    dragForce.setMag(dragMagnitude);
    return dragForce;
  }

  show() {
    noStroke();
    fill(this.col);  // Color del líquido
    rect(this.x, this.y, this.w, this.h);
  }
}
```

link -> https://editor.p5js.org/salome2607/full/LIrOMwCJx

 - Atracción gravitacional:

