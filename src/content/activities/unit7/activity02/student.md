- **Identifica conceptos clave: mientras exploras, presta atención a estos conceptos fundamentales: Engine, World, Bodies (y sus tipos: rectángulos, círculos, polígonos), Constraint, MouseConstraint, Runner/Events.**
- **Experimenta con código:**
- **Intenta replicar en p5.js al menos dos experimentos básicos mostrados en el video de Patt Vira o en los ejemplos del sitio web. Por ejemplo:**
**Crear un mundo con gravedad y añadir algunos cuerpos simples (círculos, cajas) que caigan y colisionen.** 
    - **Crear cuerpos estáticos (como el suelo).**
    - **Implementar MouseConstraint para poder interactuar con los cuerpos usando el mouse.**
    - **(Opcional avanzado) Crear una restricción simple (Constraint) entre dos cuerpos.**
- **Explica los conceptos: basándote en tu experimentación y lectura, explica con tus propias palabras qué es y para qué sirve cada uno de los conceptos clave listados en el paso 2 (Engine, World, Bodies, Constraint, MouseConstraint).**

- engine: heart of the system. what manages the physics simulations by updating the state of objects over time. make sure que las fuerzas como gravedad y movimiento sean aplicadas correctamente a los objetos en el world.

- world: environment, container for objects, bodies and constraints.

- body: individual physical object in the simulation. has its own shape (rectangles, circles, poligons) and properties (mass, position, velocity, angle, etc). interactua seugun las reglas de del engine. pueden ser static o dynamic.

- bodies: a namespace to create specific types of bodies with preset properties. namespace is a container that groups related methods, properties or objects together under a common name 

- composite: collection of bodies and constraints managed as a single unit. a constraint is how we connect things together in the world. the world is the top level composite

- render: visualizes the physics simulation

- runner: updates the engine at fixed intervals


**Muestra el código de los dos (o más) experimentos básicos que replicaste integrando Matter.js y p5.js.**

experimento 1:

voy a tratar con el ejemplo del video, que con el clic izquierdo sea generar un box, con el clic del scroll sea generar un polygon y con el clic drecho sea generar un circulo

```js
const {Engine, Body, Bodies, Composite} = Matter;

let engine;
let boxes = []; let ground; let polygons = []; let circles = [];

function setup() {
  createCanvas(400, 400);
  engine = Engine.create();
 
  ground = new Ground(200, 300, 400, 10);
}

function draw() {
  background(220);
  Engine.update(engine);
  for (let i=0; i<boxes.length; i++) {
    boxes[i].display();
  }
  for (let i=0; i<polygons.length; i++) {
    polygons[i].display();
  }
  ground.display();
}

function mousePressed() {
  if (mouseButton === LEFT) {
    boxes.push(new Rect(mouseX, mouseY, 20, 20));
  } else if (mouseButton === CENTER) {
    polygons.push(new Poli(mouseX, mouseY, 6, 20)); // 6 lados, radio 20
  }else if (mouseButton === RIGHT) {
    polygons.push(new Circle(mouseX, mouseY, 15)); // 6 lados, radio 20
  }
  
}
class Ground {
  constructor(x, y, w, h) {
    this.w = w;
    this.h = h;
    
    this.body = Bodies.rectangle(x, y, this.w, this.h, {isStatic: true});
    Composite.add(engine.world, this.body);
  }
  
  display() {
    push();
    rectMode(CENTER);
    let x = this.body.position.x;
    let y = this.body.position.y; 
    translate(x, y);
    rect(0, 0, this.w, this.h);
    pop();
  }
}
class Rect {
  constructor(x, y, w, h) {
    this.w = w;
    this.h = h;
    
    this.body = Bodies.rectangle(x, y, this.w, this.h);
    Body.setAngularVelocity(this.body, 0.2);
    Composite.add(engine.world, this.body);
  }
  
  display() {
    push();
    rectMode(CENTER);
    let x = this.body.position.x;
    let y = this.body.position.y; 
    let angle = this.body.angle;
    translate(x, y);
    rotate(angle);
    rect(0, 0, this.w, this.h);
    pop();
  }
}
class Poli{
  constructor(x, y, sides, radius) {
    this.sides = sides;
    this.radius = radius;
    
    this.body = Bodies.polygon(x, y, this.sides, this.radius);
    Body.setAngularVelocity(this.body, 0.2);
    Composite.add(engine.world, this.body);
  }
  
  display() {
    push();
    let vertices = this.body.vertices;
    fill(127);
    stroke(0);
    strokeWeight(1);
    beginShape();
    for (let i = 0; i < vertices.length; i++) {
      vertex(vertices[i].x, vertices[i].y);
    }
    endShape(CLOSE);
    pop();
  }
}
class Circle {
  constructor(x, y, r) {
    this.r = r;
    
    this.body = Bodies.circle(x, y, this.r);
    Body.setAngularVelocity(this.body, 0.2);
    Composite.add(engine.world, this.body);
  }
  
  display() {
    push();
    ellipseMode(RADIUS);
    let x = this.body.position.x;
    let y = this.body.position.y; 
    let angle = this.body.angle;
    translate(x, y);
    rotate(angle);
    ellipse(0, 0, this.r);
    pop();
  }
}
```

experimento 2: 

voy ahora a intentar con uno de los ejemplos del sitio web. escogi el del bridge y volvi todo circulos.

```js
// Define variables globally
var engine, world, render, runner, bridge, stack, mouseConstraint;

function setup() {
    // Create the canvas
    createCanvas(800, 600);
    
    // Initialize the Matter.js engine
    var Engine = Matter.Engine,
        Render = Matter.Render,
        Runner = Matter.Runner,
        Bodies = Matter.Bodies,
        Composites = Matter.Composites,
        Mouse = Matter.Mouse,
        MouseConstraint = Matter.MouseConstraint,
        Composite = Matter.Composite,
        Common = Matter.Common;

    engine = Engine.create();
    world = engine.world;

    // Create a Matter.js renderer and associate it with the p5.js canvas
    render = Render.create({
        element: document.body,
        engine: engine,
        options: {
            width: 800,
            height: 600,
            showAngleIndicator: true,
            wireframes: false, // Show the bodies as solid
        }
    });

    Render.run(render);

    // Create the Matter.js runner
    runner = Runner.create();
    Runner.run(runner, engine);

    // Create the bridge using composites
    var group = Matter.Body.nextGroup(true);
    bridge = Composites.stack(160, 290, 15, 1, 0, 0, function(x, y) {
        return Bodies.circle(x - 20, y, 20, {
            collisionFilter: { group: group },
            chamfer: 5,
            density: 0.005,
            frictionAir: 0.05,
            render: {
                fillStyle: '#060a19'
            }
        });
    });

    Composites.chain(bridge, 0.3, 0, -0.3, 0, {
        stiffness: 0.99,
        length: 0.0001,
        render: {
            visible: false
        }
    });

    // Create stack of blocks
    stack = Composites.stack(250, 50, 6, 3, 0, 0, function(x, y) {
        return Bodies.circle(x, y, 20, Common.random(20, 40));
    });

    // Add all objects to the world
    Composite.add(world, [
        bridge,
        stack,
        Bodies.rectangle(30, 490, 220, 380, {
            isStatic: true,
            chamfer: { radius: 20 }
        }),
        Bodies.rectangle(770, 490, 220, 380, {
            isStatic: true,
            chamfer: { radius: 20 }
        }),
        Matter.Constraint.create({
            pointA: { x: 140, y: 300 },
            bodyB: bridge.bodies[0],
            pointB: { x: -25, y: 0 },
            length: 2,
            stiffness: 0.9
        }),
        Matter.Constraint.create({
            pointA: { x: 660, y: 300 },
            bodyB: bridge.bodies[bridge.bodies.length - 1],
            pointB: { x: 25, y: 0 },
            length: 2,
            stiffness: 0.9
        })
    ]);

    // Set up mouse interaction
    var mouse = Mouse.create(render.canvas);
    mouseConstraint = MouseConstraint.create(engine, {
        mouse: mouse,
        constraint: {
            stiffness: 0.1,
            render: {
                visible: false
            }
        }
    });

    Composite.add(world, mouseConstraint);

    // Sync the mouse with the Matter.js render
    render.mouse = mouse;

    // Fit the render viewport to the scene
    Render.lookAt(render, {
        min: { x: 0, y: 0 },
        max: { x: 800, y: 600 }
    });
}

function draw() {
    // Update the engine on every frame
    Engine.update(engine);

    // Clear the background on each frame
    background(255);
}
```

**Incluye una captura de pantalla o ENLACE un GIF (no olvides, enlace) de cada experimento funcionando.**

exp 1

https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExenpqemx6bm44OHdvM3R5dG80bTB0d3Z0cW8xdmt3ZnVhMnV2bTRrZiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/DkOPZPm1nN8kvll0ej/giphy.gif

exp 2

https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExZnVlMnhzNHRyM3h0NDdwaXZldm95djNwenRlenMzcmF6ZXFpMDZ5ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/cxztgn3vEnnw4ohqIU/giphy.gif


**Proporciona tu explicación clara y concisa de los conceptos clave (Engine, World, Bodies, Constraint, MouseConstraint).**

- engine: es lo que maneja todo. es como el corazón del sistema. Se encarga de que las físicas funcionen bien, actualizando el estado de los objetos con el tiempo. Aplica fuerzas como la gravedad y el movimiento para que todo se vea real.
- world: es como el mundo o escenario donde están todos los objetos, los cuerpos y las conexiones.
- body: es cada objeto físico dentro de la simulación. Tiene forma (como rectángulos, círculos o polígonos) y propiedades como masa, posición, velocidad, ángulo, etc. Sigue las reglas del engine y puede ser estático (no se mueve) o dinámico (sí se mueve).
- bodies: es un como un grupo o espacio donde hay métodos listos para crear diferentes tipos de cuerpos ya configurados.
- constraints: es como una cuerda o conexión invisible que une dos cuerpos o un cuerpo a un punto fijo. 
- mouseconstraints: es un tipo especial de constraint que conecta el mouse con un cuerpo del mundo. es para agarrar y mover objetos con el mouse, como si los estuvieras agarrando con la mano.

**Menciona brevemente cualquier dificultad encontrada al configurar o usar Matter.js inicialmente.**

tuve la dificultad de que me habia equivocado escribiendo la cosa para poder usar la libreria en la parte del index, pero el profe me ayudo. tambien tuve problemas para poder que los ejemplos de la pagina web me funcionaran, me toco decirle a chat que como los modificaba para que em funcionaran en p5js.
