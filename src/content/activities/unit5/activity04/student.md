**1. ¿Cómo se gestiona la creación y la desaparición de las partículas y cómo se gestiona la memoria?**

La creación y desaparición de partículas se maneja a través de un sistema de partículas en el que cada partícula tiene una vida limitada. en cada frame se crea la particula desde el emisor y acada particula tiene un lifespan que va decreciendo y cuando llega a 0 la particula se desaparece y elimina de la memoria.

**2. ¿Cómo se aplica el marco de movimiento motion 101 en los sistemas de partículas que analizaste en la actividad anterior?**

El motion 101 se aplica usando la velocidad, acelearcion y posicion. se crean vectores para cada uno de estas y la velocidad se actualiza en cada frame sumando la aceleración, y la posición se actualiza sumando la velocidad. 

**3. ¿Cómo se aplican fuerzas externas a los sistemas de partículas que trabajaste en la unidad? ¿Qué fuerzas se aplicaron y cómo están modeladas?**

las fuerzas externas se aplican principalmente en el ruido de Perlin y la fuerza inicial de las partículas.
El ruido genera una fuerza continua que varía de manera suave y afecta las partículas de forma no brusca y  actúa como una fuerza externa que cambia suavemente la trayectoria de las partículas.

**4. ¿Cómo se aplicaste los conceptos de herencia y polimorfismo en los sistemas de partículas que trabajaste en la unidad?**

La herencia se aplica al crear una clase base, que contiene las propiedades y métodos comunes para todas las partículas y luego cree subclases que son las de circulos y cuadrados y estas heredan de la clase base. Esto hace que ambas tengan el comportamiento básico de la clase base pero que se puedan cambiar aspectos específicos como la forma.

El polimorfismo se aplica cuando se sobrescribi el método display() en cada una de las subclases para que se dibuje la forma que es.

