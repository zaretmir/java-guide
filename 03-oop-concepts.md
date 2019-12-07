- [Herencia](#herencia)
  - [Clase que hereda de otra: `extends`](#clase-que-hereda-de-otra-extends)
    - [Constructores de la subclase](#constructores-de-la-subclase)
    - [Sobreescritura de métodos de la superclase.](#sobreescritura-de-m%c3%a9todos-de-la-superclase)
  - [Clase que hereda de una clase abstracta](#clase-que-hereda-de-una-clase-abstracta)
- [Interfaces](#interfaces)
  - [Diferencia entre interfaces y clases abstractas](#diferencia-entre-interfaces-y-clases-abstractas)
- [Polimorfismo](#polimorfismo)
  - [Concepto](#concepto)
  - [Casting de objetos](#casting-de-objetos)

# Herencia

Por medio de la herencia, una clase puede adquirir atributos y métodos de otra clase.

Para que una clase herede de otra, usamos la keyword `extends`.

En Java, no existe la herencia múltiple: una clase solo puede extender a una única clase (ver Interfaces).

```java
class Animal{
    void walk(){
        System.out.println("I am walking");
    }
}
```

```java
class Bird extends Animal {
    void fly() {
        System.out.println("I am flying");
    }
}
```

```java
public class Solution {
   public static void main(String[] args){

      Bird bird = new Bird();
      bird.walk();
      bird.fly();
   }
}

// Imprime:
// I am walking
// I am flying
```

## Clase que hereda de otra: `extends`

### Constructores de la subclase

Cuando llamamos al constructor de la subclase, ya sea de manera implícita o explícita, se llama al constructor de la superclase.

```java
class Employee extends Person {
    Employee(int id) {
        super();          // implicitly added by the compiler.
    }
}
```

:warning: Si el constructor por defecto de la superclase requiere argumentos, esto fallará. Tendríamos que o bien usar `super()` pasándole los argumentos, o declarar un constructor sin argumentos en la clase padre (ver: https://stackoverflow.com/questions/9586367/constructor-of-subclass-in-java).

### Sobreescritura de métodos de la superclase.

Los métodos de la superclase se pueden sobreescribir. Pero además, si quisieramos usar el método de la superclase, podemos usar `super.superMetodo()`.

```java
class Manager extends Empleado {

    private double incentivo = 179.52;
    public double getSueldo() { // método sobreescrito
        double sueldoBase = super.getSueldo();
        return sueldoBase + incentivo;
    }
}
```

## Clase que hereda de una clase abstracta

Una clase abstracta es aquella que es declarada como `abstract`. Una clase abstracta _no puede ser instanciada_.

La finalidad de una clase abstracta es servir como un acuerdo de mínimos: todas las clases que extienden a una clase abstracta, _tienen_ que implementar los métodos abstractos de la clase abstracta.

Características:

- Los métodos abstractos de una clase abstracta solo se declaran, pero no se definen.
- Los métodos abstractos de una clase abstracta pueden ser _protected_ o _public_.
- Los métodos abstractos obligan a que la clase sea abstracta.
- La clase abstracta puede tener métodos no abstractos.
- La clase abstracta puede extender de otra clase, abstracta o no.

Ejemplo:

```java
abstract class Book{
    String title;
    abstract void setTitle(String s);
    String getTitle(){
        return title;
    }
}
```

```java
class MyBook extends Book {
    public void setTitle(String s) {
        this.title = s; // podemos acceder a title, porque hereda
    }
}
```

# Interfaces

Las interfaces son un conjunto de directrices (métodos) que deben cumplir las clases. Una clase puede implementar una o más interfaces por medio de la keyword `extends`.

Características:

- Las hay definidas (ya vienen en la API) y propias.
- Solo pueden tener métodos abstractos y constantes `final`, nunca variables.
- Aunque no se escriba explícitamente, todos los métodos son públicos y abstractos.
- No se pueden instanciar.
- Herencia en interfaces: uns interfaz puede extender de otra, usando `extends`.

## Diferencia entre interfaces y clases abstractas

https://javapapers.com/core-java/abstract-and-interface-core-java-2/difference-between-a-java-interface-and-a-java-abstract-class/

# Polimorfismo

## Concepto

Como un objeto de la subclase es también un objeto de la superclase, podemos llamar a un objeto de la subclase allí donde se espere un objeto de la superclase.

Si tipamos un objeto de la subclase como un objeto de la superclase, y llamamos un método sobreescrito, se llamará al método de la superclase.

```java
import java.util.*;

class Empleado {
    private double sueldo = 1543.28;

    public double getSueldo() {
        return this.sueldo;
    }
}


class Manager extends Empleado {

    private double incentivo = 179.52;

    public double getSueldo() { // método sobreescrito
        double sueldoBase = super.getSueldo();
        return sueldoBase + incentivo;
    }
}

public class App {
    public static void main(String[] args) {
        Empleado emp1 = new Empleado();
        System.out.println(emp1.getSueldo()); // 1543.28

        Empleado emp2 = new Manager();
        System.out.println(emp1.getSueldo());  // 1543.28

        Manager emp3 = new Manager();
        System.out.println(emp3.getSueldo());  // 1722.8

        Manager emp4 = new Manager(); // error de compilación
        System.out.println(emp4.getSueldo());
    }
}
```

:memo: **Enlazado dinámico**: es un mecanismo por el cual se escoge, en tiempo de ejecución, el método que responderá a un determinado mensaje. Se encarga el JRE.

Si creamos un array de tipo Superclase, almacenamos en él un objeto de tipo Subclase, y luego accedemos al objeto por medio de la posición, no podremos acceder a los métodos de la subclase, sólo a los de la superclase (ver casting de objetos).

## Casting de objetos

Consiste cambiar el tipo del objeto con la sintaxis `Tipodeseado nombreObjeto = (Tipodeseado) objetoAConvertir`.
