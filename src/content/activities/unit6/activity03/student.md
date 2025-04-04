**1. Ejecuta el ejemplo: ejecuta el código del ejemplo principal de Flocking de TNoC en p5.js. Observa el movimiento colectivo de los “boids” (agentes).**

**2. Identifica las tres reglas: en el código de la clase del agente (ej: Boid), localiza las funciones que implementan las tres reglas fundamentales del Flocking:**
```js
// We accumulate a new acceleration each time based on three rules
  flock(boids) {
    let sep = this.separate(boids); // Separation
    let ali = this.align(boids); // Alignment
    let coh = this.cohere(boids); // Cohesion
    // Arbitrarily weight these forces
    sep.mult(1.5);
    ali.mult(1.0);
    coh.mult(1.0);
    // Add the force vectors to acceleration
    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);
  }
```

**- Separación (Separation): evitar el hacinamiento con vecinos cercanos.**

```js
separate(boids) {
    let desiredSeparation = 25;
    let steer = createVector(0, 0);
    let count = 0;
    // For every boid in the system, check if it's too close
    for (let i = 0; i < boids.length; i++) {
      let d = p5.Vector.dist(this.position, boids[i].position);
      // If the distance is greater than 0 and less than an arbitrary amount (0 when you are yourself)
      if (d > 0 && d < desiredSeparation) {
        // Calculate vector pointing away from neighbor
        let diff = p5.Vector.sub(this.position, boids[i].position);
        diff.normalize();
        diff.div(d); // Weight by distance
        steer.add(diff);
        count++; // Keep track of how many
      }
    }
    // Average -- divide by how many
    if (count > 0) {
      steer.div(count);
    }

    // As long as the vector is greater than 0
    if (steer.mag() > 0) {
      // Implement Reynolds: Steering = Desired - Velocity
      steer.normalize();
      steer.mult(this.maxspeed);
      steer.sub(this.velocity);
      steer.limit(this.maxforce);
    }
    return steer;
  }
  ```

**- Alineación (Alignment): dirigirse en la misma dirección promedio que los vecinos cercanos.**

```js
// Alignment
  // For every nearby boid in the system, calculate the average velocity
  align(boids) {
    let neighborDistance = 50;
    let sum = createVector(0, 0);
    let count = 0;
    for (let i = 0; i < boids.length; i++) {
      let d = p5.Vector.dist(this.position, boids[i].position);
      if (d > 0 && d < neighborDistance) {
        sum.add(boids[i].velocity);
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
```

**- Cohesión (Cohesion): moverse hacia la posición promedio de los vecinos cercanos.**

```js
  // Cohesion
  // For the average location (i.e. center) of all nearby boids, calculate steering vector towards that location
  cohere(boids) {
    let neighborDistance = 50;
    let sum = createVector(0, 0); // Start with empty vector to accumulate all locations
    let count = 0;
    for (let i = 0; i < boids.length; i++) {
      let d = p5.Vector.dist(this.position, boids[i].position);
      if (d > 0 && d < neighborDistance) {
        sum.add(boids[i].position); // Add location
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      return this.seek(sum); // Steer towards the location
    } else {
      return createVector(0, 0);
    }
  }
```


**3. Explica las Reglas: para cada una de las tres reglas, explica con tus propias palabras:**
**- ¿Cuál es el objetivo de la regla?**
**- ¿Cómo calcula el agente la fuerza de dirección correspondiente? (describe la lógica general, ej: “Calcula un vector apuntando lejos de los vecinos demasiado cercanos”).**

*1. Separacion:*

Evitar colisiones entre los boids. 

Si hay otros boids demasiado cerca, calcula un vector en dirección opuesta para alejarse. Cada boid calcula un vector que lo aleja de los vecinos demasiado cercanos. Cuanto más cerca esté un vecino, mayor será la fuerza repulsiva.

El algoritmo encuentra todos los vecinos dentro de un radio de percepción, calcula vectores que apuntan en dirección opuesta a cada vecino cercano (escalados según la distancia), y los promedia para obtener una fuerza de dirección.

*2. Alineacion:*

Hace que el boid se mueva en la misma dirección que sus vecinos. 

El boid calcula la velocidad promedio (dirección y magnitud) de todos los vecinos y ajusta su propia velocidad para alinearse con ese promedio.

Se suman los vectores de velocidad de todos los vecinos dentro del radio de percepción, se calcula el promedio, y se genera una fuerza de dirección que dirige al boid hacia esa dirección promedio.

*3. Cohesion:*

Mantiene al grupo unido, haciendo que el boid se mueva hacia el centro de masa de sus vecinos. 

El boid calcula la posición promedio de todos los vecinos y se dirige hacia ese punto.

Se suman las posiciones de todos los vecinos dentro del radio de percepción, se calcula el promedio para encontrar el "centro de masa", y se genera una fuerza de dirección que guía al boid hacia ese centro.

**4. Identifica parámetros clave: localiza en el código las variables que controlan:**
**- El radio (o distancia) de percepción (perceptionRadius o similar) que define quiénes son los “vecinos”. A veces también hay un ángulo de percepción.**

```let desiredSeparation = 25;```  Distancia mínima que cada boid intenta mantener con sus vecinos (regla de separación)

```let neighborDistance = 50;```  Radio de percepción para considerar a otros boids como vecinos (usado en las reglas de alineación y cohesión)

**- Los pesos o multiplicadores que determinan la influencia relativa de cada una de las tres reglas al combinarlas.**

```
// Arbitrarily weight these forces
    sep.mult(1.5);
    ali.mult(1.0);
    coh.mult(1.0);
```

**- La velocidad máxima (maxspeed) y la fuerza máxima (maxforce) de los agentes (similar a Flow Fields).**

```
this.maxspeed = 3; // Maximum speed
this.maxforce = 0.05; // Maximum steering force
```

**5. Experimenta con modificaciones: realiza al menos una de las siguientes modificaciones en el código, ejecuta y describe el efecto observado en el comportamiento colectivo del enjambre:**
**- Cambia drásticamente el peso de una de las reglas (ej: pon la cohesión a cero, o la separación muy alta).**
**- Modifica significativamente el radio de percepción (hazlo muy pequeño o muy grande).**
**- Introduce un objetivo (target) que todos los boids intenten seguir (usando una fuerza de seek) además de las reglas de flocking, y ajusta su influencia.**
**Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el comportamiento colectivo del enjambre (¿se dispersan? ¿forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.**
