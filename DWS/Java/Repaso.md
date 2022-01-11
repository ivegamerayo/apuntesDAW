# Repaso de Java


## Pedir datos por teclado
Se instancia el objeto 
```java
Scanner teclado = new Scanner(System.in);
```

## Arrays
Los arrays se crean de la siguiente manera
```java
int [] nombre_array = new int [5];
```


# Crear un objeto
Creamos una **Java Class** y le damos un nombre siempre con la primera en **MAYUSCULAS**


```java
package prueba1;


public class Coche {
    private String matricula;
    private String marca;

    public Coche(String matricula, String marca) {
        this.matricula = matricula;
        this.marca = marca;
    }
    
    

    public String getMatricula() {
        return matricula;
    }

    public void setMatricula(String matricula) {
        this.matricula = matricula;
    }

    public String getMarca() {
        return marca;
    }

    public void setMarca(String marca) {
        this.marca = marca;
    }
    
    
    
    
}
```

### Uso de la clase Coche en el main
Nos vamos a crear un objeto de tipo Coche

```java
// Esto es en el main
Coche c1 = new Coche('9823DYV', 'marca');
```


### Ejercicio 1
Diseñar una clase Persona(dni, nombre) con constructor, set, get y toString.
Crear un array donde se almacenan estas 3 personas:
111111A, Pepe
22222B, Jose
33333C, Angel

Recorrer el array y mostrar SOLO los nombres

```java

//Persona.java

package prueba1;

public class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public void setDni(String dni) {
        this.dni = dni;
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    @Override
    public String toString() {
        return "Persona{" + "dni=" + dni + ", nombre=" + nombre + '}';
    }
    
    
    
}
```

```java
// main.java

package prueba1;

public class main {
    public static void main(String[] args) {
        
        Persona [] personas = new Persona[3];
        
        personas[0] = new Persona("11111A", "Pepe");
        personas[1] = new Persona("22222B", "Jose");
        personas[2] = new Persona("33333C", "Angel");
        
         
        for (Persona persona : personas) {
            System.out.println(persona.getNombre());
        }
    }
    }

```


# Ficheros
Lo primero que debemos de hacer es importar el _FileWriter_ y _PrintWriter_



```java
package prueba1;

import java.io.FileWriter;
import java.io.PrintWriter;

public class ficheros {
    public static void main(String[] args) {
        try {
            // fopen de php w
            FileWriter fichero = new FileWriter("personas.txt");
            
            // fopen de php a
            // FileWriter fichero = new FileWriter("personas.txt",true);
            PrintWriter pw = new PrintWriter(fichero);
            
            pw.println("angel:45");
            pw.println("jose:34");
            
            pw.close();
            
            System.out.println("Fichero creado");
        } catch (Exception e) {
        }
    }
}
```