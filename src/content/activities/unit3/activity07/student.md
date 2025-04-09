**un texto donde expliques qué problema le ves a este planteamiento y qué solución propones. ¿Cómo lo implementarías en p5.js?**
 
 Como el argumento force se pasa por referencia y no por valor, cualquier modificación que hagamos a ese vector afectará al objeto original que lo creó. 

 El error está en que la división de la fuerza por la masa se está haciendo directamente en el vector que se pasa como argumento. Como los vectores se pasan por referencia, el vector original se modifica, lo que no es lo que queremos. En lugar de modificar el vector de fuerza directamente, deberíamos trabajar con una copia del vector.

 Para solucionar este problema, podemos crear una copia del vector de fuerza antes de modificarlo. Así, la fuerza original se mantendrá intacta y podremos aplicar correctamente la misma fuerza en múltiples frames sin que se vea afectada.

```js
 applyForce(force) {
  let f = force.copy(); //Make a copy of the vector before using it.

  f.div(this.mass); //Divide the copy by mass.

  this.acceleration.add(f);
}
```
