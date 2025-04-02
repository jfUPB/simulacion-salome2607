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

Cada agente tiene una función follow(), que le permite seguir el campo de flujo.

Para determinar que vector debe seguir se toma la posición actual (x, y) del agente y se convierte en índices de la cuadrícula (col, row). Se usa floor(x / resolution) para encontrar la celda correspondiente en field[].



**4. Identifica parámetros clave: Localiza en el código las variables que controlan aspectos importantes como:**
  **- La resolución del campo de flujo (el tamaño de las celdas de la cuadrícula).**
  **- La velocidad máxima (maxspeed) y la fuerza máxima (maxforce) de los agentes.**

- Resolución del campo: Determina el tamaño de cada celda de la cuadrícula (ej: resolution = 20)
- Velocidad máxima (maxSpeed): Controla qué tan rápido pueden moverse los agentes
- Fuerza máxima (maxForce): Limita cuánto pueden cambiar de dirección en cada cuadro

**5. Experimenta con modificaciones: realiza al menos una de las siguientes modificaciones en el código, ejecuta y describe el efecto observado en el comportamiento de los agentes:**
  **- Cambia significativamente la forma en que se generan los vectores del campo (ej: usa una fórmula matemática diferente en lugar de noise(), o cambia drásticamente los parámetros de noise()).**
  **- Modifica sustancialmente la resolución del campo de flujo (hazla mucho más fina o mucho más gruesa).**
  **- Altera considerablemente maxspeed o maxforce de los agentes.**
**Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.**
