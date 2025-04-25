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

- updates the engine at fixed intervals


**Muestra el código de los dos (o más) experimentos básicos que replicaste integrando Matter.js y p5.js.**

experimento 1:


experimento 2: 


**Incluye una captura de pantalla o ENLACE un GIF (no olvides, enlace) de cada experimento funcionando.**


**Proporciona tu explicación clara y concisa de los conceptos clave (Engine, World, Bodies, Constraint, MouseConstraint).**


**Menciona brevemente cualquier dificultad encontrada al configurar o usar Matter.js inicialmente.**

