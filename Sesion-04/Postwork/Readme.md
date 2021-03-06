## Sesi贸n 4: Aplicaci贸n de l铆nea de comandos

### 馃幆 OBJETIVO

- Implementar una aplicaci贸n de l铆nea de comandos utilizando Spring Boot

### DESARROLLO

Dentro de las muchas bondades con las que cuenta en Framework de Spring, y en particular Spring Boot, se encuentra la opci贸n de poder ejecutar comandos al momento de iniciar la aplicaci贸n. Gracias a esto puedes proporcionar algunas instrucciones, rutas, o valores particulares a tu aplicaci贸n.

En este postwork deber谩s implementar la interface `CommandLineRunner` que proporciona Spring Boot, para que por medio de l铆nea de comandos solicite dos valores y los establezca como atributos de una clase `Persona`. 

Recuerda que para leer datos a trav茅s de una l铆nea de comandos se utiliza la clase `Scanner` del paquete `java.util`.

Recuerda que en el archivo de configuraci贸n de Gradle debes agregar la siguiente l铆nea:

```groovy
bootRun {
  standardInput = System.in
}
```
<details>
  <summary>Soluci贸n</summary>

  
Crea un proyecto usando Spring Initializr desde el IDE IntelliJ Idea como lo hiciste en la primera sesi贸n. Selecciona las siguientes opciones:

    Grupo, artefacto y nombre del proyecto.
    Tipo de proyecto: **Gradle**.
    Lenguaje: **Java**.
    Forma de empaquetar la aplicaci贸n: **jar**.
    Versi贸n de Java: **11** o superior.

![imagen](img/img_02.png)

En la siguiente ventana no selecciones ninguna dependencia; no las necesitaremos en este proyecto. Presiona el bot贸n `Finish`.

Crea un nuevo paquete llamado `model` y dentro crea una clase `Persona`. Esta clase debe tener dos atributos de tipo `String`, un `nombre` y un `telefono`:

```java
public class Persona {
    private String nombre;
    private String telefono;
}
```

Agrega los m茅todos **setter** y **getter** de los atributos. Agrega tambi茅n dos constructores, uno que no reciba ning煤n par谩metro y otro que reciba los dos valores anteriores. Tambi茅n, sobreescribe el m茅todo `toString` para mostrar estos valores. Esto ayudar谩 al momento de mostrar los valores de la instancia de `Persona` en la l铆nea de comandos:

```java
public class Persona {
    private String nombre;
    private String telefono;

    public Persona() {
    }

    public Persona(String nombre, String telefono) {
        this.nombre = nombre;
        this.telefono = telefono;
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getTelefono() {
        return telefono;
    }

    public void setTelefono(String telefono) {
        this.telefono = telefono;
    }

    @Override
    public String toString() {
        return "Persona{" +
                "nombre='" + nombre + '\'' +
                ", telefono='" + telefono + '\'' +
                '}';
    }
}

```

Spring Initializr crea de forma autom谩tica una clase con el mismo nombre del proyecto y el postfijo `Application`, `SolucionApplication` en este ejemplo. Esa clase estar谩 decorada con la anotaci贸n `@SpringBootApplication`. Modifica esta clase para hacer que implemente la interface `CommandLineRunner`

```java
@SpringBootApplication
public class SolucionApplication implements CommandLineRunner {

    public static void main(String[] args) {
        SpringApplication.run(SolucionApplication.class, args);
    }
}
```

`CommandLineRunner` contiene un solo m茅todo el cual se ejecuta al momento de iniciar la aplicaci贸n. Es dentro de este m茅todo donde deber谩s colocar el c贸digo de la aplicaci贸n.

```java
@Override
public void run(String... args) throws Exception {

}
```

En el cuerpo de `run` usa una instancia de `Scanner` para leer la entrada que el usuario proporcione a trav茅s de la entrada est谩ndar (el teclado). Aqu铆 deber谩s leer el texto introducido y luego usarlo para establecer los valores de los atributos de `Persona`.

```java
@SpringBootApplication
public class SolucionApplication implements CommandLineRunner {

    public static void main(String[] args) {
        SpringApplication.run(SolucionApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
         Scanner reader = new Scanner(System.in);

        System.out.println("Introduce el nombre: ");
        String nombre = reader.nextLine();

        System.out.println("Introduce el tel茅fono: ");
        String telefono = reader.nextLine();
    }
}

```

Ahora usa los valores anteriores para crear una nueva instancia de `Persona` usando el constructor que recibe ambos, y luego imprime el objeto creado:

```java
   @Override
    public void run(String... args) throws Exception {
        Scanner reader = new Scanner(System.in);

        System.out.println("Introduce el nombre: ");
        String nombre = reader.nextLine();

        System.out.println("Introduce el tel茅fono: ");
        String telefono = reader.nextLine();

        Persona persona = new Persona(nombre, telefono);

        System.out.println(persona);
    }
```

Ejecuta la aplicaci贸n. Si lo haces desde IntelliJ Idea deber谩s hacer clic en la consola y comenzar a escribir cuando se soliciten los datos:


![imagen](img/img_03.png)

La aplicaci贸n se detendr谩 de forma autom谩tica cuando el resultado se imprima en pantalla.

</details>