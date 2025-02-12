**¿Qué problema le ves a este planteamiento? ¿Qué solución propones? ¿Cómo lo implementarías en p5.js?**

el problema es que en el método applyForce(force) sobrescribe el valor de la aceleración en lugar de sumarlo a la aceleración existente. entonces no se esta haciendo sumatoria de fuerzas y la aceleracion que da no es la correcta.

Si solo se asigna this.acceleration = force, se esta ignorando las fuerzas que ya se han aplicado previamente en los frames anteriores.

se tendria que sumar en vez de solo ser igual. 

```js
// Método para aplicar una fuerza al objeto
  applyForce(force) {
    // Suma la fuerza a la aceleración existente
    this.acceleration.add(force);
  }
```

lo implementaria 
