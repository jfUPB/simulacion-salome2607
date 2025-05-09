**Diseña el proceso generativo: ¿Cuál será la lógica central que genere los visuales?**
- **Elige uno o varios conceptos/técnicas de simulación o generación del curso que se alineen con tu concepto visual (ej: sistema de partículas cuyas fuerzas son moduladas por el FFT, un flow field cuya dirección cambia con la amplitud, agentes tipo boids cuya cohesión depende de los graves, formas que crecen usando ruido Perlin modificado por el ritmo…).**

voy a usar flow fields. Partículas nacen desde un centro invisible (corazón) y son guiadas por un campo de flujo (flow field) generado por perlin noise y el campo va variando segun la musica asi afectando como se mueven las particulas

La resolucion del campo y la cantidad de particulas emitidas varían con el volumen (amplitud). Colores y la velocidad de las particulas responden a las frecuencias agudas (emoción intensa).

- **Describe cómo los inputs de audio y de interacción modularán los parámetros clave de tu algoritmo generativo. Sé específico (ej: “La amplitud controlará el número de partículas emitidas”, “La energía en los agudos del FFT aumentará la velocidad máxima de los agentes”, “El mouseX cambiará el factor de ruido en el flow field”).**

la amplitud (volumen)
- opacidad de las particulas -> más volumen = más visibles.
- resolucion del campo -> más volumen = mayor resolución (campo más complejo, movimientos más finos y densos).

la frecuencia
- colores 
- velocidad -> mas agudos = mayor velocidad, mas graves = menor velocidad.

mouse 
- cantidad de particulas con el scroll 
- destellos de particulas doradas con el clic
- barra espaceadora baja el volumen de la musica temporalmente y las particulas se desvanecen mas rapido

- **Asegúrate de que tu diseño incluya elementos generativos que hagan que la visualización no sea idéntica cada vez, incluso con la misma música (ej: uso de aleatoriedad controlada, ruido, condiciones iniciales variables).**

- Cada partícula tiene una vida útil aleatoria (entre 2 y 7 segundos).
- El campo de flujo se basa en ruido Perlin que se actualiza cada cierto tiempo con variaciones sutiles.
- La posición inicial de cada partícula tiene un margen de error aleatorio alrededor del centro.

**Define los outputs visuales:**
- **¿Qué elementos visuales específicos se generarán? (partículas, líneas, formas, colores, texturas…).**
- **¿Qué propiedades de estos elementos serán controladas dinámicamente por el proceso generativo (y, por ende, por los inputs)? (posición, tamaño, color, opacidad, velocidad, conexión, etc.).**

Partículas que se mueven suavemente por el campo. Líneas o estelas que dejan las partículas al moverse, como si dibujaran memorias o emociones en el aire. Destellos dorados efímeros al hacer clic. las propiedades que seran controladas son la velocidad, color, opacidad, cantidad

**Documenta el diseño: describe tu algoritmo (proceso) y los visuales resultantes (outputs) de forma clara. Puedes usar:**
- **Texto detallado.**
- **Pseudocódigo para la lógica clave.**
- **Diagramas de flujo o conceptuales que muestren cómo los inputs afectan al proceso y éste a los outputs.**

- Se genera un flow field con ruido Perlin que cambia según el volumen.
- Desde un centro invisible, nacen partículas con dirección definida por el campo.
- Cada partícula:
  - Tiene una velocidad y color determinadas por las frecuencias.
  - Cambia de opacidad según la amplitud.
  - Se desvanece al final de su vida útil (aleatoria).
- El usuario puede:
  - Aumentar/disminuir la canitidad de particulas con scroll.
  - Generar chispas doradas al hacer clic.
  - "Silenciar el corazón" con la barra espaciadora, provocando desvanecimiento rápido.
