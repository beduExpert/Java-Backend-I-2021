`Desarrollo Web` > `BackEnd Básico Java`

## 🧠 Ejemplo 02: Patrones Estructurales

### 🎯 OBJETIVO

- Aprender los casos de uso de los Patrones Estructurales

### 📃 REQUISITOS

1. Tener **Gradle** instalado en el equipo
2. Tener un editor de código instalado en el equipo
3. Tener acceso a la terminal del equipo

### 🎩 DESARROLLO

Los patrones de diseño **estructurales** se enfocan en como las clases y objetos se componen para formar estructuras mayores, los patrones estructurales describen como las estructuras compuestas por clases crecen para crear nuevas funcionalidades de manera de agregar a la estructura flexibilidad y que la misma pueda cambiar en tiempo de ejecución lo cual es imposible con una composición de clases estáticas.

A continuación analizaremos uno de los patrones estructurales más importantes que es **Adapter**.

**Adapter** es un patrón de diseño estructural que permite la colaboración entre objetos con interfaces incompatibles.

Imagina que estás creando una aplicación de monitoreo del mercado de valores. La aplicación descarga la información de bolsa desde varias fuentes en formato XML para presentarla al usuario con bonitos gráficos y diagramas.

En cierto momento, decides mejorar la aplicación integrando una inteligente biblioteca de análisis de una tercera persona. Pero hay una trampa: la biblioteca de análisis solo funciona con datos en formato JSON.

Podrías cambiar la biblioteca para que funcione con XML. Sin embargo, esto podría descomponer parte del código existente que depende de la biblioteca. Y, lo que es peor, podrías no tener siquiera acceso al código fuente de la biblioteca, lo que hace imposible esta solución.

![](img/adapter-problem.png)

Puedes crear un **adaptador**. Se trata de un objeto especial que convierte la interfaz de un objeto, de forma que otro objeto pueda comprenderla.

Un adaptador envuelve uno de los objetos para esconder la complejidad de la conversión que tiene lugar tras bambalinas. El objeto envuelto ni siquiera es consciente de la existencia del adaptador. Por ejemplo, puedes envolver un objeto que opera con metros y kilómetros con un adaptador que convierte todos los datos al sistema anglosajón, es decir, pies y millas.

Los adaptadores no solo convierten datos a varios formatos, sino que también ayudan a objetos con distintas interfaces a colaborar. Funciona así:

El adaptador obtiene una interfaz compatible con uno de los objetos existentes.
Utilizando esta interfaz, el objeto existente puede invocar con seguridad los métodos del adaptador.
Al recibir una llamada, el adaptador pasa la solicitud al segundo objeto, pero en un formato y orden que ese segundo objeto espera.

En ocasiones se puede incluso crear un adaptador de dos direcciones que pueda convertir las llamadas en ambos sentidos.

![](img/adapter-solution.png)

Para este ejemplo supongamos que queremos hacer funciona un Motor Eléctrico en un carro que sólo funciona a través de un Motor Común.

Comenzamos escribiendo la definición de lo que tendría que hacer nuestro Motor:

```java
public abstract class Motor {

  public abstract void encender();

  public abstract void acelerar();

  public abstract void apagar();
}
```

Ahora implementaremos la clase MotorComún a partir de la clase Motor:

```java
public class MotorComun extends Motor {

  public MotorComun() {
    super();
    System.out.println("Creando el motor comun");
  }

  @Override
  public void encender() {
    System.out.println("encendiendo motor comun");
  }

  @Override
  public void acelerar() {
    System.out.println("acelerando el motor comun");
  }

  @Override
  public void apagar() {
    System.out.println("Apagando motor comun");
  }
}
```

Por otro lado, resultado que compramos un Motor Eléctrico que tiene las siguientes operaciones:

```java
public class MotorElectrico {

  private boolean conectado = false;

  public MotorElectrico() {
    System.out.println("Creando motor electrico");
    this.conectado = false;
  }

  public void conectar() {
    System.out.println("Conectando motor electrico");
    this.conectado = true;
  }

  public void activar() {
    if (!this.conectado) {
      System.out.println(
        "No se puede activar porque no " + "esta conectado el motor electrico"
      );
    } else {
      System.out.println("Esta conectado, activando motor" + " electrico....");
    }
  }

  public void moverMasRapido() {
    if (!this.conectado) {
      System.out.println(
        "No se puede mover rapido el motor " +
        "electrico porque no esta conectado..."
      );
    } else {
      System.out.println("Moviendo mas rapido...aumentando voltaje");
    }
  }

  public void detener() {
    if (!this.conectado) {
      System.out.println(
        "No se puede detener motor electrico" + " porque no esta conectado"
      );
    } else {
      System.out.println("Deteniendo motor electrico");
    }
  }

  public void desconectar() {
    System.out.println("Desconectando motor electrico...");
    this.conectado = false;
  }
}
```

Como podemos ver, el Motor Eléctrico tiene más operaciones que un Motor Común, por lo cuál ambas clases son incompatibles. Si un carro funciona únicamente a partir de un Motor Común, no podrá hacerlo con un Motor Eléctrico.

Por lo tanto para que exista una compatibilidad implementamos un *adaptador* que nos ayudará a traducir todo lo que hacer el Motor Eléctrico en un Motor Común:

```java
public class MotorElectricoAdapter extends Motor {

  private MotorElectrico motorElectrico;

  public MotorElectricoAdapter() {
    super();
    this.motorElectrico = new MotorElectrico();
    System.out.println("Creando motor Electrico adapter");
  }

  @Override
  public void encender() {
    System.out.println("Encendiendo motorElectricoAdapter");
    this.motorElectrico.conectar();
    this.motorElectrico.activar();
  }

  @Override
  public void acelerar() {
    System.out.println("Acelerando motor electrico...");
    this.motorElectrico.moverMasRapido();
  }

  @Override
  public void apagar() {
    System.out.println("Apagando motor electrico");
    this.motorElectrico.detener();
    this.motorElectrico.desconectar();
  }
}
```

Con esto, hemos hecho un Motor Eléctrico totalmente compatible con un Motor Común. Por lo tanto escribiremos el siguiente código para probar nuestra implementación:

```java
public class PruebaAdapter {
  public static void main(String [] args) {
    Motor motor = new MotorElectricoAdapter();
    motor.encender();
    motor.acelerar();
    motor.apagar();
  }
}
```