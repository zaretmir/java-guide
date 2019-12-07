# Sistema de tipos en Java

## Tipos primitivos

Son los tipos básicos, para los cuales no es necesario crear explícitamente un objeto. Los podemos distingur porque se escriben con minúscula. Además **no** admiten valores `null`\*. En Java existen 8 tipos primitivos:

1. `char`: para representar letras individuales. 16 bit.
2. `boolean`: `true` o `false`. 16 bits.
3. `byte`: números enteros entre -128 y 127. 8 signed bit.
4. `short`: números enteros entre -32768 y 32767. 16 signed bit.
5. `int`: enteros desde $-2^{31}$ hasta $2^{32}$. 32 signed bit.
6. `long`: enteros desde $-2^{63}$ hasta $2^{64}$. 64 signed bit.
7. `float`: para representar números decimales con hasta 7 dígitos de precisión. Para declarar un float debemos añadir una `f` al final, si no, será guardado como double. 32 signed bit.
8. `double`: decimales con hasta 16 dígitos de precisión. 64 signed bit.

:point_right: https://stackoverflow.com/questions/11047276/why-cant-primitive-data-types-be-null-in-java

## Modificadores de acceso [revisar]

|                                        | `private` | `default` | `protected` | `public` |
| -------------------------------------- | --------- | --------- | ----------- | -------- |
| solo dentro de la clase                |           |           |             |          |
| cualquier clase del mismo paquete      |           |           |             |          |
| clases extiendan, de cualquier paquete |           |           |             |          |
| cualquier clase de cualquier paquete   |

# Stdin y stdout

## Leer input

Existen varias maneras de leer inputs en Java. La más popular es usar `Scanner`, especificándole que el stream de entrada sera System.in ()

```java
public class Application {
    public static void main(String args[]) {
        Scanner scanner = new Scanner(System.in);
        String inputString = scanner.next();
        int inputInt = scanner.nextInt();

        System.out.println("La cadena de entrada es: ", inputString);
        System.out.println("El numero de entrada es: ", inputInt);

    }
}
```

## `static`: miembros de clase vs miembros de instancia

Podemos usar la keyword `static` con:

- variables (obtenemos una _variable de clase_)
- métodos (obtenemos un _método de clase_)
- bloque
- clase anidada

### 1. Atributo static: variables de clase

Definir una variable como _static_ significa que el espacio de memoria que se utiliza para almacenar su valor es el mismo para cualquier instancia de la clase.

:memo: Con un atributo estático, el valor es compartido por todos los objetos.

Si es público, podemos modificarlo "de manera estática", es decir, utilizando el nombre de la clase, sin necesidad de instanciar [comprobar codigo]:

```java
// Persona.java
public class Persona {
  static public String nombreEstatico = "Nombre Estatico";
  public String nombreMiembroClase = "Nombre Miembro de la clase";
}

// Main.java
public class Main {
  static public main(String[] args) {
    // Acceso a atributo estático
    System.out.println(Persona.nombreEstatico);

    // Acceso a atributo de instancia
    Persona persona = new Persona();
    System.out.println(persona.nombreMiembroClase);
}
```

### 2. Método static: método de clase

Un método con `static` es un método que pertenece a la clase y no a las instancias. Esto significa que se puede invocar el método sin tener que crear instancias de la clase.

:memo: **Los métodos static no pueden operar sobre atributos no estáticos (atributos de instancia)**. Intentarlo da error en tiempo de compilación.

Si se pudiera, quería decir que podemos cambiar el valor de un atributo de instancia en todos los objetos a la vez. Por ejemplo:

```java
// Coche.java
public class Coche {
    public static string tipo = "vehiculo";
    public string color;

    public static cambiarColor() {
        this.color = "negro" // Dará error de compilación
    }
}

// Main.java
public class Main {
  static public main(String[] args) {
    Coche c1 = new Coche();
    c1.color = "rojo";

    Coche c2 = new Coche();
    c2.color = "azul";
}

```

Si se me permitiera acceder mediante un método estático al atributo de instancia, cambiaría ese atributo en TODOS los objetos a la clase. Convertiría esos coches en coches de color negro.

### 3. Bloques

Si queremos inicializar los atributos de clase, podemos hacerlo en una sola línea como ya hemos visto. Sin embargo, si la inicialización es compleja (por ejemplo, porque hay que realizar una comprobación, un bucle for, etc), tenemos dos opciones: utilizar un método estático, o usar un *static block-+.

Los _satic block_ se ejecutan durante el _classloading_, antes de ejecutar el método main.

```java
class A2 {
    static {
        int counter = 0;
        System.out.println("static block is invoked");
    }

    public static void main(String args[]) {
        System.out.println("Hello main");
    }
}

/*
 * Ouput:
 * static block is invoked
 * Hello main
*/
```
