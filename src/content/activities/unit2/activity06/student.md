**Código con la modificación.**

```
let t = 0;
let direction = 1;

function setup() {
    createCanvas(500, 500);
}

function draw() {
    background(200);

    let v0 = createVector(mouseX, mouseY);
    let v1 = createVector(width-10-mouseX, 0);
    let v2 = createVector(0, height-10-mouseY);
    let v3 = p5.Vector.lerp(v1, v2, t);

    // Colores
    let colorStart = color(255, 0, 0); // Rojo
    let colorEnd = color(0, 0, 255);   // Azul
    let lerpedColor = lerpColor(colorStart, colorEnd, t); // Interpolación de color

    let connectingVector = p5.Vector.sub(v2, v1);
    drawArrow(p5.Vector.add(v0, v1), connectingVector, 'green'); // Dibuja el vector verde entre las puntas del rojo y azul
    drawArrow(v0, v1, 'red');   // Vector rojo
    drawArrow(v0, v2, 'blue');  // Vector azul
    drawArrow(v0, v3, lerpedColor); // Vector morado con interpolación de color

    // Actualización de t para mover el vector morado
    t += 0.01 * direction;
    if (t >= 1 || t <= 0) {
        direction *= -1;
    }
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```

**Explica cómo solucionaste el problema.**

```
let v0 = createVector(mouseX, mouseY);
let v1 = createVector(width-10-mouseX, 0);
let v2 = createVector(0, height-10-mouseY);
```

Hice que el punto de donde salen los vectores sea determinado por donde esta el mouse

Luego para que el final del vector rojo no salga del canvas le puse que sea el ancho menos 10 y menos lo del mouse y lo mismo hice con el vector azul pero con la altura. 
