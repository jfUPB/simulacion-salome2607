> ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?
Vas a modificar cada una de las simulaciones anteriores incluyen en cada una, al menos un concepto de las unidades anteriores, pero no repitas concepto, la idea es que repases al menos uno de cada unidad.
Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste.
> - Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
> - Incluye un enlace a tu código en el editor de p5.js.
> - Incluye el código fuente de cada una de las simulaciones.
> - Captura de pantallas de cada una de las simulaciones con las imágenes que más te gusten como resultado de la ejecución de cada una de las simulaciones.


### **Simulacion #1**
**- ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?**

se crea una matriz de partículas que se actualiza en cada cuadro del programa. Las partículas se agregan a la matriz cuando se crea una nueva y desaparecen cuando su vida útil llega a cero.
La eliminación de las partículas se realiza mediante la eliminación de aquellas que ya no son visibles o han agotado su ciclo de vida. Para evitar la acumulación de objetos en memoria, las partículas muertas son eliminadas del array con splice(). Esto libera el espacio en el array y asegura que las partículas que ya no son visibles ni activas no ocupen memoria innecesaria. 

**- Explica qué concepto aplicaste, cómo lo aplicaste y por qué.**

ruido de perlin. El ruido de Perlin genera un movimiento más fluido y natural que el uso de valores aleatorios.

Voy a añadir el ruido de Perlin al eje x de la posición de la partícula para que el movimiento horizontal esté influenciado por él. Lo quiero aplicar porque proporciona un desplazamiento más suave que el uso de valores aleatorios, y hace que se vea mas organizado

- En la función update(), hemos añadido el uso de noise() para generar un valor de ruido de Perlin que se mapea al rango -1 a 1 y se suma a la velocidad en el eje x de la partícula. Esto crea un movimiento suave y continuo.
- Variable xOff para el ruido: Cada partícula tiene una variable xOff única, que controla su posición en el ruido de Perlin. Esto asegura que las partículas se muevan de manera diferente entre sí, pero de forma coherente.
- Incremento de xOff: Se incrementa en cada actualización (this.xOff += 0.01) para que el valor del ruido cambie suavemente con el tiempo, generando un movimiento fluido.

**- Enlace a tu código en el editor de p5.js.**

https://editor.p5js.org/salome2607/sketches/Iq0izU1ta 

**- El código de la simulacion.**

```js
let particles = [];
let xOffset = 0; // Variable para el ruido de Perlin

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);
  particles.push(new Particle(width / 2, 20));

  // Looping through backwards to delete particles
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];
    particle.run();
    if (particle.isDead()) {
      // Remove the particle
      particles.splice(i, 1);
    }
  }
}

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    this.xOff = random(100); // Offset único para cada partícula en el ruido de Perlin
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    // Aplicamos el ruido de Perlin al eje x
    let noiseX = map(noise(this.xOff), 0, 1, -1, 1); // Mapeamos el ruido de Perlin a un rango útil
    this.velocity.x += noiseX; // Modificamos la velocidad en el eje x usando el ruido

    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);

    // Incrementa el offset de ruido para suavizar el movimiento
    this.xOff += 0.01;
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}
```

**- Captura de pantalla.**

![Foto](../../../../assets/unidad5/act2-1.gif)


### **Simulacion #2**
**- ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?**

Se usa una clase llamada emitter que es el sistema de particulas y luego se crea un sistema de emitters. Cada vez que se hace clic se crea un nuevo emisor. 
Aqui no se elimina ningún emisor, por lo que se debe tener cuidado con la acumulación de objetos en memoria.

Cada vez que el mouse es presionado, se crea una nueva instancia de Emitter (un sistema emisor de partículas) en la posición donde se hizo clic. Este emisor es agregado al array emitters.
En cada frame (draw()), cada emisor del array emitters llama a su método addParticle(), que crea nuevas partículas en su posición de origen (origin). Estas partículas son almacenadas en el array particles del emisor.

Dentro de cada Emitter, en el método run(), se recorre el array de partículas en orden inverso. Si una partícula ha "muerto" (es decir, si su propiedad lifespan es menor a 0), se elimina del array mediante splice(). Esto permite que las partículas sean removidas del array cuando ya no son útiles, evitando que el array crezca indefinidamente.
La memoria se gestiona eliminando las partículas una vez que ya no son necesarias. Cada vez que una partícula desaparece (cuando su lifespan llega a 0), se elimina del array particles mediante splice()

**- Explica qué concepto aplicaste, cómo lo aplicaste y por qué.**

En esta simulacion aplique el concepto de fuerza de atraccion. En lugar de depender solo de la gravedad o de movimientos al azar, voy a implementar una fuerza de atracción hacia el mouse para que las partículas sean atraídas hacia la posición actual del mouse.

- En el método run() de la clase Particle, he añadido una fuerza que atrae a las partículas hacia la posición actual del mouse.
- La fuerza se calcula restando la posición de la partícula de la posición del mouse: let force = p5.Vector.sub(mouse, this.position). Esto genera un vector que apunta hacia el mouse.
- Luego, se ajusta la magnitud de esta fuerza con force.setMag(0.2) para que las partículas se muevan suavemente hacia el mouse.

**- Enlace a tu código en el editor de p5.js.**

https://editor.p5js.org/salome2607/sketches/kPbzqPABv

**- El código de la simulacion.**

```js
let emitters = [];

function setup() {
  createCanvas(640, 240);
  let text = createP("click to add particle systems");
}

function draw() {
  background(255);
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
  }
}

function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    
    // Fuerza de atracción hacia el mouse
    let mouse = createVector(mouseX, mouseY);
    let force = p5.Vector.sub(mouse, this.position); // Fuerza hacia el mouse
    force.setMag(0.2); // Magnitud controlada de la fuerza
    this.applyForce(force);
    
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Método para actualizar la posición
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0); // Resetea la aceleración después de aplicar las fuerzas
  }

  // Método para mostrar la partícula
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // La partícula ha muerto si su lifespan es menor que 0
  isDead() {
    return this.lifespan < 0.0;
  }
}

class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    // Looping through backwards to delete
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].run();
      if (this.particles[i].isDead()) {
        // Remove the particle
        this.particles.splice(i, 1);
      }
    }
  }
}
```

**- Captura de pantalla.**

![Foto](../../../../assets/unidad5/act2-2.gif)

### **Simulacion #3**
**- ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?**

Se utilizan clases heredadas para crear diferentes tipos de partículas. Se utiliza el polimorfismo para crear las particulas normales y otras que le llaman confetti. Y 

**- Explica qué concepto aplicaste, cómo lo aplicaste y por qué.**

**- Enlace a tu código en el editor de p5.js.**

**- El código de la simulacion.**

**- Captura de pantalla.**

### **Simulacion #4**
**- ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?**

**- Explica qué concepto aplicaste, cómo lo aplicaste y por qué.**

**- Enlace a tu código en el editor de p5.js.**

**- El código de la simulacion.**

**- Captura de pantalla.**

### **Simulacion #5**
**- ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?**

**- Explica qué concepto aplicaste, cómo lo aplicaste y por qué.**

**- Enlace a tu código en el editor de p5.js.**

**- El código de la simulacion.**

**- Captura de pantalla.**
