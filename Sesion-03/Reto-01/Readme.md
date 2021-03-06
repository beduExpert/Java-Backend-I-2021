## Reto 01: Prueba de Exceptions

### 馃幆 OBJETIVO

- Realizar una prueba unitaria que se encargue de validar que al momento de validar una funcionalidad, se lanza la excepci贸n esperada.

### DESARROLLO

Al momento de mostrar una demo de nuestra calculadora al cliente, este qued贸 maravillado de su correcto funcionamiento y la ejecuci贸n correcta de todas las operaciones que nos pidi贸 realizar durante la demo. Sin embargo, not贸 que al momento de realizar la implementaci贸n de la calculadora omitimos una operaci贸n que para 茅l es esencial en sus operaciones diarias: la divisi贸n. Tu reto ser谩 arreglar este terrible error que hemos cometido al entregar una aplicaci贸n incompleta al cliente.

La divisi贸n es una de las cuatro operaciones b谩sicas de la aritm茅tica que consiste en averiguar cu谩ntas veces un n煤mero (divisor) est谩 contenido en otro n煤mero (dividendo). Sin embargo, y a diferencia de las otras tres operaciones, la divisi贸n presenta una complicaci贸n. Existe un valor que podemos asignar al divisor y que causa que la divisi贸n explote al encontrar una singularidad espacial en un punto importante el cual podemos ver en la siguiente imagen:

![imagen](img/img_01.png)


En matem谩ticas, la divisi贸n entre cero es una divisi贸n en la que el divisor es igual a cero, y que no tiene un resultado bien definido. En aritm茅tica y 谩lgebra, es considerada una *indefinici贸n*, y su mal uso puede dar lugar a aparentes paradojas matem谩ticas.

Como queremos evitarle molestias a nuestro cliente y demostrarle que nuestra calculadora est谩 preparada para todos los posibles valores que le ingresemos debes validar, en una prueba unitaria desarrollada usando JUnit, que si intentamos realizar una divisi贸n entre cero se lanzar谩 una excepci贸n de tipo "IllegalArgumentException" con el mensaje mostrado a continuaci贸n:


```java
    public float divide(float a, float b){

        if(b == 0){
            throw new IllegalArgumentException("No es posible dividir un valor entre 0");
        }

        return a / b;
    }
```

隆Buena suerte!


<details>
  <summary>Soluci贸n</summary>

Agregamos el m茅todo de prueba en la clase `CalculadoraTest` siguiendo la misma estructura que para el resto de los m茅todos:

```java

  @Test
  @DisplayName("Prueba divisi贸n")
  void divideTest() {
    
  }
```

La soluci贸n al reto puede hacerse de dos formas. En la primera debemos hacerlo en dos pasos. Primero, usamos el m茅todo `assertThrows` para indicar que esperamos que la invocaci贸n al m茅todo `divide` lance una excepci贸n. Este m茅todo recibe dos par谩metros, en el primero debemos indicar la clase de la excepci贸n. En el segundo par谩metro usaremos una funci贸n lambda para realizar la invocaci贸n al m茅todo que estamos probando. El llamado a `assertThrows` regresar谩 la excepci贸n lanzada por el m茅todo:

```java
  @Test
  @DisplayName("Prueba divisi贸n")
  void divideTest() {
  
    Throwable exception = assertThrows(IllegalArgumentException.class, () -> calculadora.divide(100, 0));

 }
```

El segundo paso consiste en obtener el mensaje (el texto) de la excepci贸n para comprobar que es el mismo texto que estamos esperando. Por lo que la prueba completa queda de la siguiente forma:

```java
    @Test
    @DisplayName("Prueba divisi贸n")
    void divideTest() {
        Throwable exception = assertThrows(IllegalArgumentException.class, () -> calculadora.divide(100, 0));

        assertEquals("No es posible dividir un valor entre 0", exception.getMessage());
    }
```

La segunda forma de implementar la soluci贸n nos permite usar una versi贸n sobrecargada de `assertThrows` que recibe como tercer par谩metro el mensaje que estamos esperando obtener:

```java
    @Test
    @DisplayName("Prueba divisi贸n")
    void divideTest() {
        assertThrows(IllegalArgumentException.class, () -> calculadora.divide(100, 0), "No es posible dividir un valor entre 0");
    }
```

Al ejecutar la prueba anterior debes obtener el siguiente mensaje indicando que la prueba fue satisfactoria y que el cliente finalmente nos pagar谩 por nuestra aplicaci贸n:

![imagen](img/img_02.png)


</details>