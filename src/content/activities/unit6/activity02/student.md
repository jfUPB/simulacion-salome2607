**1. Ejecuta el ejemplo: ejecuta el código del ejemplo principal de Flow Fields de TNoC en p5.js. Observa el comportamiento de los vehículos/agentes.**

El código crea un conjunto de agentes/vehículos que se mueven siguiendo un campo de flujo generado con Perlin Noise. Este campo define direcciones de movimiento en cada celda de una cuadrícula, haciendo que las partículas sigan trayectorias fluidas y naturales.

**2. Identifica la estructura del campo: en el código (usualmente en una clase FlowField), localiza cómo se almacena el campo de flujo. ¿Qué estructura de datos se usa (ej: un array 2D)? ¿Qué representa cada elemento de esa estructura? ¿Cómo se calcula inicialmente el vector en cada punto?**

el campo de flujo se almacena como una matriz bidimensional de vectores, donde cada celda contiene una dirección específica y se crea en una funciona aparte. 

Es como una cuadrícula invisible que divide toda la pantalla:
- Cada celda de esta cuadrícula contiene un vector (una dirección con fuerza)
- Estos vectores le dicen a los agentes hacia dónde deberían moverse
- Se calculan inicialmente usando la función noise() de Perlin, que crea patrones suaves y naturales


**3. Analiza el comportamiento del agente: en el código de la clase del vehículo/agente (Vehicle), encuentra la función follow(). Explica con tus palabras:**
 **- ¿Cómo determina el agente qué vector del campo de flujo debe seguir basándose en su posición actual? (pista: implica mapear la posición a índices de la cuadrícula).**
  **- Una vez que tiene el vector deseado del campo, ¿cómo lo utiliza para calcular la fuerza de dirección (steering force)? (pista: implica calcular la diferencia con la velocidad actual y limitar la fuerza).**

```js
  follow(flow) {
    // What is the vector at that spot in the flow field?
    let desired = flow.lookup(this.position);
    // Scale it up by maxspeed
    desired.mult(this.maxspeed);
    // Steering is desired minus velocity
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce); // Limit to maximum steering force
    this.applyForce(steer);
  }
```

Cada agente tiene una función follow(), que le permite seguir el campo de flujo. 

El agente usa su posición actual para buscar en la cuadrícula del campo de flujo el vector que le corresponde. Este proceso se realiza mediante la función lookup() del campo de flujo
- Toma la posición del agente (this.position).
- Convierte esa posición en índices de la cuadrícula usando un mapeo de coordenadas (dividiendo la posición por la resolución del campo de flujo).
- Retorna el vector almacenado en esa celda del campo de flujo.

Una vez que el agente obtiene el vector del flujo, lo utiliza para calcular su nueva dirección de la siguiente manera:
- Escala el vector del campo multiplicándolo por maxspeed, para que el agente intente moverse a su velocidad máxima en esa dirección
- Calcula la diferencia entre la dirección deseada y la velocidad actual (esta diferencia es la "steering force")
- Limita la fuerza de dirección a un máximo (maxforce) para evitar cambios bruscos en el movimiento
- Aplica la fuerza de dirección al agente

**4. Identifica parámetros clave: Localiza en el código las variables que controlan aspectos importantes como:**
  **- La resolución del campo de flujo (el tamaño de las celdas de la cuadrícula).**
  **- La velocidad máxima (maxspeed) y la fuerza máxima (maxforce) de los agentes.**

- Resolución del campo: Determina el tamaño de cada celda de la cuadrícula
- Velocidad máxima (maxSpeed): Controla qué tan rápido pueden moverse los agentes
- Fuerza máxima (maxForce): Limita cuánto pueden cambiar de dirección en cada cuadro

**5. Experimenta con modificaciones: realiza al menos una de las siguientes modificaciones en el código, ejecuta y describe el efecto observado en el comportamiento de los agentes:**
  **- Cambia significativamente la forma en que se generan los vectores del campo (ej: usa una fórmula matemática diferente en lugar de noise(), o cambia drásticamente los parámetros de noise()).**
  **- Modifica sustancialmente la resolución del campo de flujo (hazla mucho más fina o mucho más gruesa).**
  **- Altera considerablemente maxspeed o maxforce de los agentes.**
**Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.**
 
Si modifico la resolucion del campo se cambia el tamaño de cada celda. Por ejemplo puse la resolucion en 5 ``` flowfield = new FlowField(5);``` y los agentes siguen un campo con cambios más detallados y precisos, generando movimientos más pequeños. 

https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExdTcwczRyYWFxcHYyb3NmZ256cGIya3pyczFkbXp2YWM0eTUyZm9zcSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/YKdnNMnkdTBgaaT9Zx/giphy.gif

Si le modifico el maxspeed a los agentes se mueven mas rapido o mas lento. por ejemplo la puse en 8 ```this.maxspeed = 8;``` y se movian mas rapido. Y pues si la pongo mas bajita por ejemplo en 2 se mueven mucho mas lento.

https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExNzRubXRieGpzcGNuNW02NWQwdmN1eTJoaXR5YXhlbnllMXA0MjAzMSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/TQ7eboahMJISVNy5jf/giphy.gif

https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExZnZndmRrNDdncHFuazNvY280cWRtb2YwZ3hrdmkxeG9lOTloc3BpbCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/HzEvRpRB6Z1zIwEtXv/giphy.gif

Si le modifico el maxforce a los agentes el movimiento de cambio de direccion se vuelve mas brusco o mas fluido. por ejemplo si la cambio a 2 se vuelven mas brusco ```this.maxforce = 2;``` y si la pongo mas bajita los movimientos son mas fluidos ```this.maxforce = 0.5;```

https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExNmNoaHQ0Z2dydjNqbXh5bzZhamxpam5rem12dXV1NWppYnpxdzM3OCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/DmwkYQdfN6OEVGukrn/giphy.gif

