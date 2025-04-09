**Observa de nuevo esta parte del código ¿Cuál es la relación entre r y theta con las posiciones x y y? Puedes repasar entonces la definición de coordenadas polares y cómo se convierten a coordenadas cartesianas.**

La relación entre r y theta con las posiciones x e y es que r representa la distancia desde el origen hasta el punto en el plano, mientras que theta es el ángulo que determina la dirección de ese punto desde el origen.

Se usa r y theta para convertir las coordenadas polares a coordenadas cartesianas x y y. para convertir a y se usa la funcion sin() y para convertir a x se utiliza la funcion cos().

**Modifica la función draw(): ¿Qué ocurre? ¿Por qué?**

me sale un error diciendo que x no esta definida. 

En la parte de  ```line(0, 0, x, y);``` hay un error porque x e y no están definidos. La línea debería dibujarse usando las componentes del vector v ```let v = p5.Vector.fromAngle(theta);```que se esta usando en vez de covertir las coordenadas polares a cartesianas. 

**Ahora realiza esta modificación: ¿Qué ocurre aquí? ¿Por qué?**

ahora si funciona, porque se estan usando las componentes del vector ```line(0, 0, v.x, v.y);```. Se esta dibujando una línea desde el centro de la pantalla hasta el punto (v.x, v.y), que ya está escalado correctamente en función del valor de r. No es necesario multiplicar v.x y v.y por r porque el vector ya tiene la magnitud adecuada.

```let v = p5.Vector.fromAngle(theta,r);``` ademas se esta creando el vector a partir del ángulo theta, pero con una magnitud de r. a diferencia con la versión anterior es que ahora fromAngle te permite pasar un segundo parámetro, que define la longitud o magnitud del vector.De esta manera, no es necesario multiplicar el vector por r después de crearlo, ya que la longitud del vector se establece automáticamente en r.



