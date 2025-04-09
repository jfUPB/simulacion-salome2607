**Un texto donde expliques tu intención de diseño.**

Mi diseño es un algoritmo que genera circulos y que cuando se mueve el mouse los circulos se cambian de color.

**¿Cómo piensas usar los tres conceptos y por qué estos?**

- Ruido Perlin: Se utiliza noise() para generar las coordenadas X e Y de cada figura, lo que permite que las formas se muevan de manera no tan loca por la pantalla.
- Distribución Normal (Gaussiana): Se utiliza randomGaussian() para definir el tamaño de los círculos. Esto genera tamaños más comunes alrededor de 50 píxeles, con algunas figuras más grandes o más pequeñas.
- Distribucion No Uniforme: El código usa la función randomGaussian() para generar el tamaño de los círculos que aplica una distribución no uniforme. O sea hay unos tamaños de circulo que tienen mas probabilidad de salir que otros. 

**Reporta los referentes que usaste para inspirarte.**

Los referentes fueron los de los ejemplos de las actividades. Especialemente del de random walk y el de el video de Patrik Hübner que su pagina con el arte generativo se interactua con el movimiento del mouse.
