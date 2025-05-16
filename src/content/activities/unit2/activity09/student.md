**Aceleración constante.**

En el experimento que hice con aceleracion constante el circulito tiene una aceleración constante que lo hace moverse cada vez más rápido en una dirección fija


```js
let position;
let velocity;
let acceleration;

function setup() {
  createCanvas(400, 400);
  position = createVector(50, 50);
  velocity = createVector(0, 0);
  acceleration = createVector(0.05, 0.1); // Aceleración constante
}

function draw() {
  background(220);

  // Actualizar velocidad y posición
  velocity.add(acceleration);
  position.add(velocity);

  // Dibujar el objeto
  fill(127);
  ellipse(position.x, position.y, 20, 20);

  // Si sale de la pantalla, lo reiniciamos
  if (position.x > width || position.y > height) {
    position.set(50, 50);
    velocity.set(0, 0);
  }
}
```

**Aceleración aleatoria.**

En este el circulito se mueve locamente, cambiando de velocidad aleatoriamente, yendo rapido y lento. tambien cambia de direccion, esto porque , el objeto cambia de dirección porque la aceleración en este caso es un vector que cambia tanto en magnitud como en dirección cada vez que se actualiza, lo que afecta directamente la velocidad del objeto.

```js
let position;
let velocity;

function setup() {
  createCanvas(400, 400);
  position = createVector(width / 2, height / 2);
  velocity = createVector(0, 0);
}

function draw() {
  background(220);

  // Aceleración aleatoria
  let acceleration = createVector(random(-0.2, 0.2), random(-0.2, 0.2));

  // Actualizar velocidad y posición
  velocity.add(acceleration);
  position.add(velocity);

  // Dibujar el objeto
  fill(127);
  ellipse(position.x, position.y, 20, 20);

  // Si el objeto toca los bordes, lo reiniciamos
  if (position.x > width || position.x < 0 || position.y > height || position.y < 0) {
    position.set(width / 2, height / 2);
    velocity.set(0, 0);
  }
}
```

**Aceleración hacia el mouse.**

En este experimento, el objeto es atraído hacia el puntero del mouse. La aceleración siempre apunta hacia la posición del cursor. La aceleracion no es constante, cambia segun que tan lejos este del mouse. Si esta cerca la aceleracion es baja y si esta lejos la aceleracion aumenta par apoder seguirlo. 

```js
let position;
let velocity;

function setup() {
  createCanvas(400, 400);
  position = createVector(random(width), random(height));
  velocity = createVector(0, 0);
}

function draw() {
  background(220);

  // Calcula la aceleración hacia el mouse
  let mouse = createVector(mouseX, mouseY);
  let acceleration = p5.Vector.sub(mouse, position);
  acceleration.setMag(0.2); // Establece la magnitud de la aceleración

  // Actualizar velocidad y posición
  velocity.add(acceleration);
  velocity.limit(5); // Limitar la velocidad máxima
  position.add(velocity);

  // Dibujar el objeto
  fill(127);
  ellipse(position.x, position.y, 20, 20);
}
```
