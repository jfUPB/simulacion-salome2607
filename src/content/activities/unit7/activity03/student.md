**Indica claramente la palabra elegida.**

tengo dos ideas
equilibrio, desintegrar

las letras balanceadas una ecima de la otra o en palitos cada una y toca ponerlas con el mouse sin que se caigan. las letras se van desintegrando en particulas cuando les das clic 

creo que voy a escoger: equilibrio

**Explica tu idea conceptual: ¿Cómo la animación física representa el significado de la palabra?**

las letras que forman la palabra aparecen desordenadas en el suelo y deben ser colocadas cuidadosamente sobre palitos verticales. El usuario debe organizarlas manualmente usando el mouse, intentando mantenerlas estables. Esta interacción representa el esfuerzo de lograr el equilibrio, tanto literal como simbólicamente.

**Describe brevemente los aspectos técnicos clave de tu implementación: ¿Cómo formaste las letras con Matter.js? ¿Qué propiedades físicas fueron importantes? ¿Usaste restricciones?**

Cada letra de la palabra fue creada como un cuerpo rectangular en Matter.js, con dimensiones personalizadas y un color distinto. Las letras tienen baja fricción y alta restitución para que puedan moverse y rebotar ligeramente al caer. Los "palitos" sobre los que deben colocarse son cuerpos estáticos y verticales. No se use  restricciones (Constraints), ya que el objetivo es que el equilibrio sea mas dificil y dependiente de la habilidad del usuario para poner las letras correctamente. 

**Incluye el código completo de tu sketch final.**

```js
let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Body = Matter.Body,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;

let engine;
let world;
let boxes = [];
let sticks = [];
let ground;
let letters = ['E','Q','U','I','L','I','B','R','I','O'];

function setup() {
  createCanvas(1000, 400);
  engine = Engine.create();
  world = engine.world;
  
  createP(
    "💡 Arrastra las letras con el mouse y colócalas sobre los palitos sin que se caigan formando la palabra EQUILIBRIO. ¡Tu reto es lograr que todas queden en equilibrio al mismo tiempo!.<br>" 
  );

  
  engine.gravity.y = 0.7;

  let spacing = width / (letters.length + 1);

  // Palitos más anchos
  for (let i = 0; i < letters.length; i++) {
    let x = spacing * (i + 1);
    let stick = Bodies.rectangle(x, 250, 20, 120, {
      isStatic: true
    });
    sticks.push(stick);
    World.add(world, stick);
  }

  // Letras 
  for (let i = 0; i < letters.length; i++) {
    let x = random(100, width - 100);
    let y = random(height - 100, height - 40);
    let box = Bodies.rectangle(x, y, 60, 60, {
      restitution: 0.05,    // rebote muy bajo
      friction: 1,          // mucha fricción
      density: 0.005        // más peso
    });
    box.letter = letters[i];
    box.color = color(random(100, 255), random(100, 255), random(100, 255));
    boxes.push(box);
    World.add(world, box);
  }

  // Suelo
  ground = Bodies.rectangle(width / 2, height + 20, width, 40, { isStatic: true });
  World.add(world, ground);

  // Mouse control
  let canvasMouse = Mouse.create(canvas.elt);
  let mConstraint = MouseConstraint.create(engine, {
    mouse: canvasMouse,
    constraint: {
      stiffness: 0.2,
      render: { visible: false }
    }
  });
  World.add(world, mConstraint);
}

function draw() {
  background(230);
  Engine.update(engine);

  // Palitos
  for (let stick of sticks) {
    push();
    translate(stick.position.x, stick.position.y);
    rotate(stick.angle);
    rectMode(CENTER);
    fill(150);
    rect(0, 0, 20, 120);
    pop();
  }

  // Letras
  for (let b of boxes) {
    push();
    translate(b.position.x, b.position.y);
    rotate(b.angle);
    rectMode(CENTER);
    fill(b.color);
    rect(0, 0, 60, 60);
    fill(255);
    textAlign(CENTER, CENTER);
    textSize(32);
    text(b.letter, 0, 0);
    pop();
  }

  // Suelo
  fill(100);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width, 40);
}
```

**Inserta una captura de pantalla estática Y un enlace a un GIF animado (¡Esencial!) que muestre tu tipografía semántica animada en acción.**

https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExajlzZnRya2Q0cHhiaHI5aWM0ZGoxYTRvOHZjb3UyNXJteWE4NHJ1eSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/PDEfhHZUHAHOhyrDBQ/giphy.gif


