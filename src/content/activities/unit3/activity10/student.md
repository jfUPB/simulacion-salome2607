 **el código  y enlace de cada una de las simulaciones y un texto donde expliques cómo modelaste cada fuerza.**

 - friccion: tres bolitas en tres diferentes usperficies con diferentes fricciones

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
 - Atracción gravitacional:
