**¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?**

mag() calcula la magnitud o el largo del vector. Devuelve la distancia entre los dos puntos del vector.

magSq() es la maginitud del vector pero al cuadrado. 

**¿Para qué sirve el método normalize()?**

Escala los componentes del vector para que su magnitud sea 1. O sea convierte el vector en un vector unitario pero mantiene la direccion del vector. 

**Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?**

El meotodo dot sirve para calcular el producto punto entre dos vectores. 

El producto punto es una operación que mide cuánto de un vector apunta en la misma dirección que otro vector. El producto de punto es un número que describe la superposición entre dos vectores. 

**El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?**

la version estatica:  Llamas a p5.Vector.dot() directamente sin necesitar un vector de origen. Pasas ambos vectores como argumentos.

```js
let v1 = createVector(1, 2);
let v2 = createVector(2, 3);
let dotProduct = p5.Vector.dot(v1, v2);  // Producto punto entre v1 y v2
```

la version de instancia: Se utiliza cuando ya tienes un vector creado y llamas al método dot() sobre el vector actual, pasando el segundo vector como argumento.

```js
let v1 = createVector(1, 2);
let v2 = createVector(2, 3);
let dotProduct = v1.dot(v2);  // Producto punto entre v1 y v2
```

**Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.**

El producto cruz es como un vector que sale derecho del plano creado por dos vectores, o sea resulta en un vector perpendicular a los otros dos.

La magnitud del vector resultante representa el área del paralelogramo formado por los dos vectores originales y la orientacion es perpendicular a ambos vectores, en la dirección determinada por la regla de la mano derecha.

**¿Para que te puede servir el método dist()?**

calcula la distancia entre dos puntos representados por vectores 

**¿Para qué sirven los métodos normalize() y limit()?**

normalize hace la magnitus de un vector igual a 1 y limit le pone un limite maximo a la magnitud de un vector. 

pueden ser utiles para la velocidad de movimiento de un vector, por ejemplo normalize para mover el vector a una velocidad constante y limit cuando no se quiere que el vector exceda una velocidad maxima.
