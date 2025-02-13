**un texto donde expliques la diferencia entre paso por valor y paso por referencia. ¿Qué implica esto en el fragmento de código?**

Paso por valor significa que cuando se pasa una variable a una función (o se asigna a otra variable), se pasa una copia del valor original. Cualquier modificación realizada a esa copia no afecta a la variable original.

Paso por referencia significa que en lugar de pasar una copia del valor, se pasa una referencia o un enlace directo al valor original. Cualquier cambio hecho a la referencia afecta al valor original, ya que ambos apuntan al mismo lugar en la memoria.

```let friction = this.velocity.copy();```
- paso por valor
- Crea una NUEVA instancia de vector
- El nuevo vector tiene los mismos valores pero es independiente
- Modificar friction NO afectará a this.velocity

```let friction = this.velocity;```
- paso por referencia 
- Aquí se está asignando this.velocity a friction por referencia.
- friction y this.velocity apuntan al mismo objeto en memoria
- Modificar friction TAMBIÉN modificará this.velocity

Si se usa ```let friction = this.velocity;``` y luego se modifica friction, también se estará modificando ```this.velocity```. Esto podría generar un comportamiento no deseado, ya que el vector de fricción debería ser independiente de ```this.velocity``` y aplicarse como una fuerza aparte.

Por otro lado, si usas ```let friction = this.velocity.copy();```, estás asegurando que friction es una copia independiente de ```this.velocity```. Esto te permite aplicar la fuerza de fricción sin modificar el vector de velocidad directamente, lo que es el comportamiento correcto en este contexto físico.
