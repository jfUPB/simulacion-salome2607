**1. Ejecuta el ejemplo: ejecuta el código del ejemplo principal de Flocking de TNoC en p5.js. Observa el movimiento colectivo de los “boids” (agentes).**


**2. Identifica las tres reglas: en el código de la clase del agente (ej: Boid), localiza las funciones que implementan las tres reglas fundamentales del Flocking:**
**- Separación (Separation): evitar el hacinamiento con vecinos cercanos.**



**- Alineación (Alignment): dirigirse en la misma dirección promedio que los vecinos cercanos.**



**- Cohesión (Cohesion): moverse hacia la posición promedio de los vecinos cercanos.**



**3. Explica las Reglas: para cada una de las tres reglas, explica con tus propias palabras:**
**- ¿Cuál es el objetivo de la regla?**
**- ¿Cómo calcula el agente la fuerza de dirección correspondiente? (describe la lógica general, ej: “Calcula un vector apuntando lejos de los vecinos demasiado cercanos”).**

 1. Separacion:

 2. Alineacion:

 3. Cohesion:

**4. Identifica parámetros clave: localiza en el código las variables que controlan:**
**- El radio (o distancia) de percepción (perceptionRadius o similar) que define quiénes son los “vecinos”. A veces también hay un ángulo de percepción.**
**- Los pesos o multiplicadores que determinan la influencia relativa de cada una de las tres reglas al combinarlas.**
**- La velocidad máxima (maxspeed) y la fuerza máxima (maxforce) de los agentes (similar a Flow Fields).**


**5. Experimenta con modificaciones: realiza al menos una de las siguientes modificaciones en el código, ejecuta y describe el efecto observado en el comportamiento colectivo del enjambre:**
**- Cambia drásticamente el peso de una de las reglas (ej: pon la cohesión a cero, o la separación muy alta).**
**- Modifica significativamente el radio de percepción (hazlo muy pequeño o muy grande).**
**- Introduce un objetivo (target) que todos los boids intenten seguir (usando una fuerza de seek) además de las reglas de flocking, y ajusta su influencia.**
**Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el comportamiento colectivo del enjambre (¿se dispersan? ¿forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.**
