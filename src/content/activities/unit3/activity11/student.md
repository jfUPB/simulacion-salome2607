**diseñar una simulación interactiva, en la que se muestre el problema de los n-cuerpos**

**El código de tu simulación**

```js
let systems = []; // Lista de sistemas solares

function setup() {
  createCanvas(640, 640);
  createNewSystem(width / 2, height / 2); // Crear el primer sistema solar en el centro
  background(255);
}

function draw() {
  background(0, 50);

  for (let system of systems) {
    system.run();
  }
}

// Crear un nuevo sistema solar en la posición del clic
function mousePressed() {
  createNewSystem(mouseX, mouseY);
}

// Función para crear un nuevo sistema solar
function createNewSystem(x, y) {
  let newSun = new Mover(0, 0, 0, 0, 100, color(255, 204, 0)); // El nuevo "sol", tamaño reducido
  let newMovers = [];
  
  for (let i = 0; i < 50; i++) { // Reducido el número de planetas para mejor visualización
    let pos = p5.Vector.random2D();
    let vel = pos.copy();
    vel.setMag(random(5, 10)); // Velocidades más lentas
    pos.setMag(random(50, 75)); // Órbitas más pequeñas
    vel.rotate(PI / 2);
    let m = random(5, 10); // Planetas más pequeños
    newMovers[i] = new Mover(pos.x, pos.y, vel.x, vel.y, m, color(random(255), random(255), random(255)));
  }

  systems.push(new SolarSystem(newSun, newMovers, x, y)); // Agregar el sistema solar a la lista de sistemas
}

class SolarSystem {
  constructor(sun, movers, x, y) {
    this.sun = sun;
    this.movers = movers;
    this.position = createVector(x, y); // Posición del sistema solar
    this.t = 0; // Para controlar el "latido" del sol
  }

  run() {
    push();
    translate(this.position.x, this.position.y);

    // Atraer las partículas hacia el sol
    for (let mover of this.movers) {
      this.sun.attract(mover);
      for (let other of this.movers) {
        if (mover !== other) {
          mover.attract(other);
        }
      }
    }

    // Actualizar y mostrar los planetas
    for (let mover of this.movers) {
      mover.update();
      mover.show();
    }

    this.showSun(); // Mostrar el sol con el efecto de latido
    pop();
  }

  showSun() {
    noStroke();
    let pulse = sin(this.t) * 5; // Efecto de latido más pequeño
    this.t += 0.05;
    fill(255, 204, 0, 150 + pulse * 10); // Color brillante con latido
    ellipse(0, 0, (this.sun.r + pulse) * 2); // Sol más pequeño
  }
}

class Mover {
  constructor(x, y, vx, vy, m, col) {
    this.pos = createVector(x, y);
    this.vel = createVector(vx, vy);
    this.acc = createVector(0, 0);
    this.mass = m;
    this.r = sqrt(this.mass) * 1;
    this.color = col;
    this.trail = [];
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  attract(mover) {
    let force = p5.Vector.sub(this.pos, mover.pos);
    let distanceSq = constrain(force.magSq(), 50, 500); // Ajuste de distancia para atracción
    let G = 1;
    let strength = (G * (this.mass * mover.mass)) / distanceSq;
    force.setMag(strength);
    mover.applyForce(force);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.set(0, 0);
    this.trail.push(this.pos.copy());

    if (this.trail.length > 50) {
      this.trail.shift();
    }
  }

  show() {
    noStroke();
    fill(this.color);

    noFill();
    beginShape();
    for (let v of this.trail) {
      vertex(v.x, v.y);
    }
    endShape();

    fill(this.color);
    ellipse(this.pos.x, this.pos.y, this.r * 2); // Planetas más pequeños
  }
}
```

**Un texto donde expliques cómo modelaste el problema de los n-cuerpos.**

me inspire mucho en uno de los ejemplos del texto. queria que fuera como un sistema solar, el los planetas dan vueltas alrededor del sol. 

en esta simulacion múltiples planetas orbitan alrededor de un sol central, siguiendo las leyes de la gravitación. Para modelar la interacción entre los cuerpos, utilicé la ley de gravitación universal de Newton

Cada planeta en la simulación experimenta una atracción hacia el sol, y esta fuerza depende de la masa del sol y de la distancia entre el planeta y el sol. Esto mismo se aplica también para la atracción entre planetas, haciendo que no solo orbitan alrededor del sol, sino que también pueden influir en los movimientos de los demás cuerpos cercanos.

La fuerza gravitacional calculada entre dos cuerpos se convierte en una aceleración utilizando la segunda ley de Newton. La aceleración es luego sumada a la velocidad del planeta, y esta velocidad se suma a su posición para actualizar el movimiento del cuerpo. Así, los planetas se desplazan a lo largo de órbitas simuladas que se ajustan dinámicamente a las interacciones con otros cuerpos.

Ademas añadi la capacidad de crear múltiples sistemas solares haciendo clic en el canvas. Cada clic genera un nuevo sol y un conjunto de planetas que orbitan a su alrededor, todos siguiendo las mismas leyes gravitacionales.

**No olvides incluir un enlace a tu simulación en el editor de p5.js.**

https://editor.p5js.org/salome2607/full/PAKAz8OkZ 

**Adicionalmente, captura una imagen del resultado.**

![Foto](../../../../assets/unidad3/videoUni3.gif)


