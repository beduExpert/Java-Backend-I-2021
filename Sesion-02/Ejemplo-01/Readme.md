`Desarrollo Web` > `BackEnd Básico Java`

## 🧠 Ejemplo 01: Patrones Creacionales

### 🎯 OBJETIVO

- Aprender los casos de uso de los Patrones Creacionales

### 📃 REQUISITOS

1. Tener **Gradle** instalado en el equipo
2. Tener un editor de código instalado en el equipo
3. Tener acceso a la terminal del equipo

### 🎩 DESARROLLO

Los patrones de diseño **creacionales** son aquellos que nos proponen soluciones para problemas relacionados con la instanciación de objetos. Automatizar el proceso de construcción de un objeto complejo, limitar la instanciación de una clase a un único objeto al que se pueda acceder de manera global o usar un objeto como prototipo para crear otros objetos similares son algunos de los usos de los patrones de diseño creacionales.

A continuación analizaremos uno de los patrones creacionales más importantes que es **Singleton**.

El patrón **Singleton** resuelve un problema no tan común pero de suma importancia: *garantizar que una clase tenga una única instancia.* ¿Por qué querría alguien controlar cuántas instancias tiene una clase? El motivo más habitual es controlar el acceso a algún recurso compartido, por ejemplo, una base de datos o un archivo o bien evitar el consumo excesivo de memoria al momento de crear instancias de diferentes clases.

![](img/singleton-explanation.png)

Funciona así: imagina que has creado un objeto y al cabo de un tiempo decides crear otro nuevo. En lugar de recibir un objeto nuevo, obtendrás el que ya habías creado.

Ten en cuenta que este comportamiento es imposible de implementar con un constructor normal, ya que una llamada al constructor siempre debe devolver un nuevo objeto por diseño.

Todas las implementaciones del patrón **Singleton** tienen estos dos pasos en común:

- Hacer privado el constructor por defecto para evitar que otros objetos utilicen el operador new con la clase **Singleton**.
- Crear un método de creación estático que actúe como constructor. Tras bambalinas, este método invoca al constructor privado para crear un objeto y lo guarda en un campo estático. Las siguientes llamadas a este método devuelven el objeto almacenado en caché.
- Si tu código tiene acceso a la clase **Singleton**, podrá invocar su método estático. De esta manera, cada vez que se invoque este método, siempre se devolverá el mismo objeto.

![](img/singleton-comic.png)

A continuación tenemos su implementación en Java:

```java
public class Singleton {

  private static Singleton instance;
  public String value;

  private Singleton(String value) {
    this.value = value;
  }

  public static Singleton getInstance(String value) {
    if (instance == null) {
      instance = new Singleton(value);
    }

    return instance;
  }
}
```

Con dicha implementación garantizamos que al momento de llamar el método estático `getInstance` obtendremos siempre la misma instancia sin importar el valor que tenga como parámetro el método.

Ahora implementamos una clase que utilice nuestra implementación de Singleton para verificar su funcionamiento:

```java
public class PruebaSingleton {

  public static void main(String[] args) {
    Singleton singleton = Singleton.getInstance("FOO");
    Singleton anotherSingleton = Singleton.getInstance("BAR");

    System.out.println(singleton.value);
    System.out.println(anotherSingleton.value);
  }
}
```

> 💡 *Nota: Recuerda que todos los ejemplos y retos de esta sesión utilizarán la misma configuración de Gradle, cambiando únicamente la clase principal del proyecto*

```groovy
plugins {
  id 'application'
}

application {
  mainClass = "PruebaSingleton"
}
```