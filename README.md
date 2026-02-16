# RETO #8 – El Zoológico de los UML

## Información Académica

**Universidad:** Escuela Colombiana de Ingeniería Julio Garavito  
**Curso:** Diseño Orientado a Servicios Web  
**Laboratorio:** #2 - Diseño Orientado a Objetos  
**Fecha:** Febrero 2026

### Autores
- **Roger Mauricio Durán Guacaname**
- **Camilo Alfonso León Acosta**

---

## Descripción de la Solución

Este proyecto implementa un sistema orientado a objetos para gestionar un zoológico, aplicando los principios fundamentales de la programación orientada a objetos y las mejores prácticas de diseño de software. El sistema ha sido diseñado siguiendo:

 **Principios SOLID**  
 **Herencia y Polimorfismo**  
 **Encapsulamiento** (atributos privados + getters/setters)  
 **Patrones de Diseño**  
 **Asociaciones entre entidades**

### Entidades Modeladas

El sistema gestiona las siguientes entidades principales:

- **Animales** (mamíferos, reptiles, aves)
- **Cuidadores** (personal especializado)
- **Visitantes** (turistas e interacciones)
- **Interacciones** (registro de eventos)
- **Atributos dinámicos** (características variables)

---

## Diagrama de Clases UML

### Clase Abstracta: Animal

```
Animal (Abstracta)
─────────────────────────────────────────────
- nombre: String
- edad: int
- sonidoCaracteristico: String
- dieta: String
- alimentoPreferido: String
- peso: double
- altura: double
- estadoSalud: EstadoSalud
- habitat: String
- atributosDinamicos: Map<String, String>
─────────────────────────────────────────────
+ hacerSonido(): void (abstracto)
+ comer(): void
+ getNombre(): String
+ setNombre(String): void
+ getEdad(): int
+ setEdad(int): void
+ getPeso(): double
+ setPeso(double): void
+ getAltura(): double
+ setAltura(double): void
+ getEstadoSalud(): EstadoSalud
+ setEstadoSalud(EstadoSalud): void
+ agregarAtributoDinamico(String, String): void
+ getAtributoDinamico(String): String
```

### Enum: EstadoSalud

```
EstadoSalud
─────────────
SANO
ENFERMO
CUARENTENA
```

### Jerarquía de Herencia (Polimorfismo)

```
              Animal (abstracta)
                    |
        ┌───────────┼───────────┐
        |           |           |
    Mamifero     Reptil       Ave
```

#### Implementaciones Polimórficas

Cada clase hija sobrescribe el método `hacerSonido()`:

- **Mamífero** → `"Rugido"` / `"Aullido"` / `"Bramido"`
- **Ave** → `"Canto"` / `"Graznido"` / `"Pío"`
- **Reptil** → `"Silbido"` / `"Siseo"`

**Ejemplo de código:**
```java
@Override
public void hacerSonido() {
    System.out.println(nombre + " hace: " + sonidoCaracteristico);
}
```

---

### Clase Cuidador

```
Cuidador
─────────────────────────────────────────────
- nombre: String
- edad: int
- especialidad: List<TipoAnimal>
- animalesAsignados: List<Animal>
─────────────────────────────────────────────
+ alimentar(Animal): void
+ banar(Animal): void
+ limpiarHabitat(Animal): void
+ getNombre(): String
+ setNombre(String): void
+ getEdad(): int
+ setEdad(int): void
+ getAnimalesAsignados(): List<Animal>
+ asignarAnimal(Animal): void
```

**Relaciones:**
- Un cuidador → **N animales** (1:N)
- Un animal → **N cuidadores** (N:N)

---

### Clase Visitante

```
Visitante
─────────────────────────────────────────────
- nombre: String
- edad: int
- animalesFavoritos: List<Animal>
─────────────────────────────────────────────
+ alimentarAnimal(Animal): void
+ darPropina(Cuidador, double): void
+ subirFotografia(Animal): void
+ agregarAnimalFavorito(Animal): void
+ getNombre(): String
+ setNombre(String): void
+ getEdad(): int
+ setEdad(int): void
```

**Relaciones:**
- Visitante → **Animales** (interacción)
- Visitante → **Cuidadores** (propinas)

---

### Clase Interaccion

```
Interaccion
─────────────────────────────────────────────
- tipo: String
- fecha: Date
- visitante: Visitante
- animal: Animal
─────────────────────────────────────────────
+ getTipo(): String
+ setTipo(String): void
+ getFecha(): Date
+ setFecha(Date): void
+ getVisitante(): Visitante
+ getAnimal(): Animal
```

---

## Relaciones UML

### Herencia (Generalización)
```
Animal ◁── Mamifero
Animal ◁── Ave
Animal ◁── Reptil
```

### Asociaciones
```
Cuidador ◇────── Animal     (Agregación N:N)
Visitante ─────── Animal    (Asociación)
Visitante ─────── Cuidador  (Asociación)
Interaccion ─────▶ Visitante (Composición)
Interaccion ─────▶ Animal    (Composición)
```

**Diagrama de Relaciones:**
```
┌──────────┐         cuida          ┌──────────┐
│ Cuidador │◇──────────────────────◇│  Animal  │
└──────────┘                        └──────────┘
                                         △
                                         │ hereda
     ┌───────────────┬─────────────┬────┴────┐
     │               │             │         │
┌──────────┐   ┌─────────┐   ┌─────────┐    │
│ Mamifero │   │   Ave   │   │ Reptil  │    │
└──────────┘   └─────────┘   └─────────┘    │
                                             │
┌───────────┐    interactúa                  │
│ Visitante │─────────────────────────────────┘
└───────────┘
     │
     │ genera
     ▼
┌──────────────┐
│ Interaccion  │
└──────────────┘
```

---

## Encapsulamiento

Todos los atributos de las clases son declarados como **`private`** para proteger la integridad de los datos y se accede a ellos mediante **métodos públicos**.

### Ejemplo de Encapsulamiento:

```java
public class Animal {
    private String nombre;  // Privado
    private int edad;       // Privado
    
    // Getters y Setters
    public String getNombre() {
        return nombre;
    }
    
    public void setNombre(String nombre) {
        if (nombre != null && !nombre.isEmpty()) {
            this.nombre = nombre;
        }
    }
    
    public int getEdad() {
        return edad;
    }
    
    public void setEdad(int edad) {
        if (edad >= 0) {
            this.edad = edad;
        }
    }
}
```

**Ventajas:**
- Control de validación en los setters
- Protección de datos internos
- Facilita el mantenimiento
- Permite cambios internos sin afectar código externo

---

## Polimorfismo

El polimorfismo se aplica principalmente en el método `hacerSonido()`, permitiendo que cada tipo de animal responda de manera diferente al mismo mensaje.

### Ejemplo de Uso:

```java
Animal animal1 = new Mamifero("León", 5);
Animal animal2 = new Ave("Águila", 3);
Animal animal3 = new Reptil("Serpiente", 2);

animal1.hacerSonido(); // Output: "León hace: Rugido"
animal2.hacerSonido(); // Output: "Águila hace: Graznido"
animal3.hacerSonido(); // Output: "Serpiente hace: Silbido"
```

**Beneficios:**
- Código más flexible y extensible
- Permite tratar objetos diferentes de manera uniforme
- Facilita la adición de nuevos tipos sin modificar código existente

---

## Atributos Dinámicos

Para permitir características variables sin modificar la estructura de las clases, se implementó un sistema de atributos dinámicos usando:

```java
private Map<String, String> atributosDinamicos = new HashMap<>();
```

### Método de Implementación:

```java
public void agregarAtributoDinamico(String clave, String valor) {
    atributosDinamicos.put(clave, valor);
}

public String getAtributoDinamico(String clave) {
    return atributosDinamicos.get(clave);
}
```

### Ejemplos de Uso:

```java
animal.agregarAtributoDinamico("colorPelaje", "Blanco");
animal.agregarAtributoDinamico("origen", "África");
animal.agregarAtributoDinamico("rareza", "En peligro");
animal.agregarAtributoDinamico("historialMedico", "Vacunado");
```

**Ventajas:**
- No requiere modificar la clase Animal
- Flexibilidad total para nuevas características
- Fácil de implementar y mantener

---

## Principios SOLID Aplicados

### **S** — Single Responsibility Principle (Responsabilidad Única)

Cada clase tiene una única razón para cambiar:

- **Animal**: Gestiona datos y comportamiento del animal
- **Cuidador**: Gestiona el cuidado de animales
- **Visitante**: Gestiona interacciones turísticas
- **Interaccion**: Registra eventos específicos

```java
// CORRECTO: Responsabilidad única
class Animal {
    private String nombre;
    public void hacerSonido() { ... }
}

// INCORRECTO: Múltiples responsabilidades
class Animal {
    private String nombre;
    public void hacerSonido() { ... }
    public void guardarEnBaseDeDatos() { ... } // No es su responsabilidad
}
```

---

### **O** — Open/Closed Principle (Abierto/Cerrado)

El sistema está **abierto para extensión** pero **cerrado para modificación**:

```java
// Extensión sin modificar código existente
class Pez extends Animal {
    @Override
    public void hacerSonido() {
        System.out.println("Glup glup");
    }
}

class Anfibio extends Animal {
    @Override
    public void hacerSonido() {
        System.out.println("Croac croac");
    }
}
```

**No se requiere modificar:**
- La clase `Animal`
- La clase `Cuidador`
- El resto del sistema

---

### **L** — Liskov Substitution Principle (Sustitución de Liskov)

Cualquier subclase puede sustituir a su clase padre sin romper la funcionalidad:

```java
public void alimentarAnimal(Animal animal) {
    animal.comer();  // Funciona con cualquier tipo de Animal
}

// Uso
alimentarAnimal(new Mamifero());  // Funciona
alimentarAnimal(new Ave());       // Funciona
alimentarAnimal(new Reptil());    // Funciona
```

---

### **I** — Interface Segregation Principle (Segregación de Interfaces)

Se pueden crear interfaces específicas para evitar métodos innecesarios:

```java
interface Alimentable {
    void alimentar();
}

interface Bañable {
    void bañar();
}

interface Cuidable extends Alimentable, Bañable {
    void revisarSalud();
}
```

**Ventaja:** Las clases solo implementan lo que necesitan.

---

### **D** — Dependency Inversion Principle (Inversión de Dependencias)

Las clases de alto nivel dependen de **abstracciones**, no de implementaciones concretas:

```java
// CORRECTO: Depende de la abstracción
class Cuidador {
    public void alimentar(Animal animal) {  // Animal es abstracto
        animal.comer();
    }
}

// INCORRECTO: Depende de implementación concreta
class Cuidador {
    public void alimentar(Mamifero mamifero) {  // Muy específico
        mamifero.comer();
    }
}
```

---

## Patrones de Diseño Aplicados

### 1. Factory Method (Método Fábrica)

Permite crear objetos sin especificar la clase exacta:

```java
public class AnimalFactory {
    public static Animal crearAnimal(String tipo, String nombre, int edad) {
        switch (tipo.toLowerCase()) {
            case "mamifero":
                return new Mamifero(nombre, edad);
            case "ave":
                return new Ave(nombre, edad);
            case "reptil":
                return new Reptil(nombre, edad);
            default:
                throw new IllegalArgumentException("Tipo de animal desconocido");
        }
    }
}

// Uso
Animal leon = AnimalFactory.crearAnimal("mamifero", "León", 5);
Animal aguila = AnimalFactory.crearAnimal("ave", "Águila", 3);
```

**Ventajas:**
- Centraliza la creación de objetos
- Facilita cambios futuros
- Reduce dependencias

---

### 2. Strategy (Estrategia)

Para manejar diferentes tipos de dietas:

```java
interface DietaStrategy {
    void alimentar(Animal animal);
}

class Carnivoro implements DietaStrategy {
    public void alimentar(Animal animal) {
        System.out.println(animal.getNombre() + " come carne");
    }
}

class Herbivoro implements DietaStrategy {
    public void alimentar(Animal animal) {
        System.out.println(animal.getNombre() + " come plantas");
    }
}

class Omnivoro implements DietaStrategy {
    public void alimentar(Animal animal) {
        System.out.println(animal.getNombre() + " come de todo");
    }
}
```

---

### 3. Decorator (Decorador)

Para agregar funcionalidades dinámicas sin modificar la clase original:

```java
abstract class AnimalDecorator extends Animal {
    protected Animal animalDecorado;
    
    public AnimalDecorator(Animal animal) {
        this.animalDecorado = animal;
    }
}

class AnimalConRegistroMedico extends AnimalDecorator {
    private String historialMedico;
    
    public AnimalConRegistroMedico(Animal animal, String historial) {
        super(animal);
        this.historialMedico = historial;
    }
    
    public String getHistorialMedico() {
        return historialMedico;
    }
}
```

**Uso:**
```java
Animal leon = new Mamifero("León", 5);
Animal leonConHistorial = new AnimalConRegistroMedico(leon, "Vacunación al día");
```

---

## Características Bonus (Mejoras Adicionales)

### Clase Zoologico

```java
public class Zoologico {
    private String nombre;
    private String direccion;
    private List<Animal> animales;
    private List<Cuidador> cuidadores;
    private List<Visitante> visitantes;
    
    public void agregarAnimal(Animal animal) { ... }
    public void contratarCuidador(Cuidador cuidador) { ... }
    public void registrarVisita(Visitante visitante) { ... }
    public List<Animal> getAnimalesPorTipo(Class<?> tipo) { ... }
}
```

### Sistema de Registro de Visitas

```java
public class RegistroVisitas {
    private Date fecha;
    private Visitante visitante;
    private List<Interaccion> interacciones;
    
    public void registrarInteraccion(Interaccion interaccion) { ... }
    public List<Interaccion> getInteraccionesPorAnimal(Animal animal) { ... }
}
```

### Historial Médico Especializado

```java
public class HistorialMedico {
    private Animal animal;
    private List<Consulta> consultas;
    private List<Vacuna> vacunas;
    private List<Tratamiento> tratamientos;
    
    public void agregarConsulta(Consulta consulta) { ... }
    public void registrarVacuna(Vacuna vacuna) { ... }
    public EstadoSalud evaluarEstado() { ... }
}
```

### Interfaces Adicionales

```java
interface Interactuable {
    void interactuar(Visitante visitante);
}

interface Cuidable {
    void recibirCuidado(Cuidador cuidador);
}

interface Fotografiable {
    void tomarFotografia(Visitante visitante);
}
```

---

## Resumen del Diagrama UML (Vista General)

```
                    ┌─────────────┐
                    │  Zoologico  │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
     ┌────▼────┐      ┌────▼────┐     ┌────▼────┐
     │ Animal  │      │Cuidador │     │Visitante│
     │(abstract)│      └────┬────┘     └────┬────┘
     └────┬────┘           │               │
          │                │               │
    ┌─────┼─────┐          │               │
    │     │     │          │               │
┌───▼──┐ │ ┌───▼──┐       │               │
│Mamife│ │ │ Ave  │       │               │
│ ro   │ │ └──────┘       │               │
└──────┘ │                │               │
     ┌───▼──┐             │               │
     │Reptil│             │               │
     └──────┘             │               │
                          │               │
                     ┌────▼───────────────▼────┐
                     │     Interaccion         │
                     └─────────────────────────┘
```

---

## Cómo Extender el Sistema

### Agregar un Nuevo Tipo de Animal

```java
public class Anfibio extends Animal {
    private boolean esAcuatico;
    
    public Anfibio(String nombre, int edad) {
        super(nombre, edad);
        this.sonidoCaracteristico = "Croac";
        this.dieta = "Insectívoro";
    }
    
    @Override
    public void hacerSonido() {
        System.out.println(nombre + " hace: Croac croac");
    }
}
```

### Agregar Nueva Funcionalidad a Cuidador

```java
public class Cuidador {
    // ... código existente ...
    
    public void entrenarAnimal(Animal animal, String habilidad) {
        System.out.println("Entrenando a " + animal.getNombre() + 
                         " en: " + habilidad);
    }
}
```

---

## Conclusiones

Este proyecto demuestra la aplicación práctica de:

**Diseño Orientado a Objetos**: Modelado efectivo de entidades del mundo real  
**Principios SOLID**: Código mantenible, escalable y robusto  
**Patrones de Diseño**: Soluciones comprobadas a problemas comunes  
**Buenas Prácticas**: Encapsulamiento, polimorfismo y herencia  
**Flexibilidad**: Sistema extensible sin modificar código existente

El sistema resultante es **profesional**, **escalable** y **fácil de mantener**, siguiendo las mejores prácticas de la ingeniería de software moderna.

---

## Referencias

- Gamma, E., Helm, R., Johnson, R., & Vlissides, J. (1994). *Design Patterns: Elements of Reusable Object-Oriented Software*
- Martin, R. C. (2008). *Clean Code: A Handbook of Agile Software Craftsmanship*
- Martin, R. C. (2017). *Clean Architecture: A Craftsman's Guide to Software Structure and Design*

---

**Escuela Colombiana de Ingeniería Julio Garavito**  
*Diseño Orientado a Servicios Web - 2026*