**¿Qué pasa si en un frame actúan sobre un objeto dos fuerzas? ¿Cómo calculas la aceleración resultante?**

**¿Por qué es necesario multiplicar la aceleración por cero en cada frame?**
**¿Por qué se multiplica por cero justo al final de update()?**

Es necesario multiplicar la acelarion por 0 en cada frame porque asi  se calculen las fuerzas desde cero, sino entonces se irian sumando la aceleracion con las aceleraciones de los frames anteriores lo cual harai que el movimiento no sea realista.

se multiplica justo al final porque es despues de usarla. Porque en el update primero se usa la aceleración actual para actualizar la velocidad y luego se usa la velocidad para actualizar la posición. despues de eso ya si se reinicia la aceleración para el siguiente frame
