

**¿Qué resultado esperas obtener?**

Antes de llamar a playingVector(posicion), posicion será el vector (6, 9).
Después de llamar a playingVector(posicion), espero que posicion haya cambiado a (20, 30).

**¿Qué resultado obtuviste?**

Use toString para poder ver los valores del vector en la consola.

```js
let position;

function setup() {
    createCanvas(400, 400);
    posicion = createVector(6,9);
    console.log("antes");
    console.log(posicion.toString()); 
    playingVector(posicion);
    console.log("despues");
    console.log(posicion.toString());
    noLoop();
}

function playingVector(v){
    v.x = 20;
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
    //console.log(posicion.toString());
}
```

me dice los valores antes y despues de la funcion playing vector

![{40170F37-B5B6-46E2-93D2-3E4FA1ECBBBB}](https://github.com/user-attachments/assets/c16aa35a-c856-4f7e-a5b1-bc3b0478b128)


**Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.**

> - Paso por valor: Cuando una función recibe una copia del valor de una variable. Cualquier modificación dentro de la función no afecta el valor original.
> - Paso por referencia: Cuando una función recibe una referencia al valor original. Las modificaciones hechas en la función afectan el valor original.

> - Los tipos primitivos se pasan por valor, lo que significa que las funciones reciben una copia del valor, y los cambios dentro de la función no afectan el valor original.
> - Los objetos y vectores en p5.js (que son objetos) se pasan por referencia, lo que permite modificar el valor original desde dentro de una función.

**¿Qué tipo de paso se está realizando en el código?**

En este se esta usando paso por referencia porque cuando se usa playingVector el valor original de posicion si se afecta

**¿Qué aprendiste?**

aprendi la diferencia entre paso de referencia y paso por valor.  y que tipo de dato se deberia pasar por cual.
