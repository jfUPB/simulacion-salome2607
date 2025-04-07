**Diferencias fundamentales: ¿Cuál dirías que es la diferencia principal en cómo Flow Fields y Flocking logran el movimiento coordinado o dirigido de los agentes? (piensa en dónde reside la “inteligencia” o las reglas: ¿En el entorno o en las interacciones entre agentes?).**

En Flow Fields, la inteligencia está en el entorno: un campo vectorial invisible guía el movimiento de cada agente según su posición
Los agentes simplemente “leen” el vector que les corresponde en el campo y lo siguen.

En Flocking, la inteligencia está en las interacciones locales entre agentes. 
Cada agente toma decisiones con base en sus vecinos: se separa de los que están muy cerca, se alinea con los que van en la misma dirección y se agrupa con los que están a cierta distancia.

Es como la diferencia entre seguir las corrientes de un río (Flow Fields) versus un grupo de peces coordinándose entre sí (Flocking).

**Tipos de comportamiento emergente: basado en tu análisis y aplicación, ¿Qué tipo de comportamiento colectivo o patrón visual crees que es más fácil o natural lograr con Flow Fields? ¿Y con Flocking? Da ejemplos.**

Con flowfields creo que es mas facil representar fenómenos dirigidos por fuerzas física, flujos naturales, corrientes. Por ejemplo corrientes de viento o de agua.

con flocking creo que seria mas facil crear patrones como de colectivos o grupales. por ejemplo simular bandadas de pájaros, cardúmenes de peces o movimientos de multitudes

**Ventajas y desventajas: en tu opinión, ¿Cuáles podrían ser las ventajas o desventajas de usar uno u otro algoritmo para ciertos tipos de efectos visuales o simulaciones?**

Flow Fields tiene la ventaja de que es muy fácil de controlar desde el entorno, ya que los agentes solo siguen las direcciones que les da el campo. Esto lo hace mejor para efectos visuales suaves como viento o agua. Sin embargo, no permite simular interacciones entre agentes. En cambio, Flocking es muy bueno para representar comportamientos grupales, de grupos que se mueven juntos. Yo pienso que el de Flocking da una sensación de vida, como si los agentes realmente tuvieran conciencia y se relacionaran entre ellos. Siento que si se usa Flow Fields para simular seres vivos, se vería muy artificial. Aunque Flocking es más complejo y consume más recursos, genera una coordinación más natural y creíble.

**El agente autónomo: ¿Cómo te ayudaron estos dos ejemplos (Flow Fields y Flocking) a entender mejor el concepto de “agente autónomo”? ¿Qué características definen a un agente en estos sistemas?**

Ambos algoritmos ayudan a entender que un agente autónomo es una entidad que Tiene comportamiento propio y sigue reglas, puede percibir el entorno o a otros agentes y toma decisiones locales, pero el conjunto genera patrones globales.

En Flow Fields, los agentes demuestran autonomía limitada (siguen reglas simples basadas en su posición), mientras que en Flocking exhiben mayor autonomía social (toman decisiones basadas en relaciones dinámicas con otros).

**Emergencia: ¿En qué momento observaste “comportamiento emergente” (complejidad o patrones no programados explícitamente) al trabajar con estos algoritmos?**

 
 
 noté que cuando mezclaba emociones distintas, surgían áreas del canvas donde predominaba una emoción, generando algo parecido a “zonas mentales” con distintos estados anímicos. Este

