# Práctica 1: Herencia, composición y polimorfismo

## Ejercicios propuestos

### Ejercicio 1

Dados los siguientes fragmentos de código, responder a las siguientes preguntas:

#### `ElementsSet.java`

```java
public class ElementsSet<E> extends HashSet<E> {
    //Number of attempted elements insertions using the "add" method
    private int numberOfAddedElements = 0;

    public ElementsSet() {}

    @Override
    public boolean add(E element) {
        numberOfAddedElements++; //Counting the element added
        return super.add(element);
    } 

    @Override
    public boolean addAll(Collection<? extends E> elements) {
        numberOfAddedElements += elements.size(); //Counting the elements added
        return super.addAll(elements);
    } 

    public int getNumberOfAddedElements() {
        return numberOfAddedElements;
    }
}
```

#### `Main.java`

```java
    ...
    ElementsSet<String> set = new ElementsSet<String>();
    set.addAll(Arrays.asList("One", "Two", "Three"));
    System.out.println(set.getNumberOfAddedElements());
    ...
```

#### Preguntas propuestas

a) ¿Es el uso de la herencia adecuado para la implementación de la clase `ElementsSet`? ¿Qué salida muestra la función `System.out.println` al invocar el método `getNumberOfAddedElements`, 3 o 6?

El uso de la herencia no es adecuado, puesto que se reutiliza parte de la interfaz de HashSet para añadir una funcionalidad, una variable que almacena el número de elementos que hay en el set; sin embargo, esta funcionalidad, de la cual además se implementa un getter, no implementa realmente una función nueva, puesto que en HashSet, ya existe el método size(), que devuelve el tamaño del set.

La función muestra 6.

b) En el caso de que haya algún problema en la implementación anterior, proponga una solución alternativa usando composición/delegación que resuelva el problema.

Usando composición, podría eliminarse el extends, y definir el ElementsSet sobre un atributo HashSet privado, lo cual permitiría reutilizar el comportamiento de HashSet, sin tener necesariamente que utilizar su interfaz. El código de ElementsSet.java quedaría así:

```java
public class ElementsSet<E>{
    private HashSet<E> HashSet_;

    public ElementsSet() 
    {
        HashSet_ = new HashSet<E>();
    }

    public boolean add(E element) {
        return HashSet_.add(element);
    } 

    @Override
    public boolean addAll(Collection<? extends E> elements) {
        return HashSet_.addAll(elements);
    } 

    public int getNumberOfAddedElements() {
        return HashSet_.size();
    }
}
```

### Ejercicio 2

Dado los siguientes fragmentos de código responder a las siguientes preguntas:

#### `Animal.java`

```java
public abstract class Animal {
    //Number of legs the animal holds
    protected int numberOfLegs = 0;

    public abstract String speak();
    public abstract boolean eat(String typeOfFeed);
    public abstract int getNumberOfLegs();
}
```

#### `Cat.java`

```java
public class Cat extends Animal {
    @Override
    public String speak() {
        return "Meow";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if(typeOfFeed.equals("fish")) {
            return true;
        } else {
            return false;
        }
    }

    @Override
    public int getNumberOfLegs() {
        return super.numberOfLegs;
    }
}
```

#### `Dog.java`

```java
public class Dog extends Animal {
    @Override
    public String speak() {
        return "Woof";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if(typeOfFeed.equals("meat")) {
            return true;
        } else {
            return false;
        }
    }

    @Override
    public int getNumberOfLegs() {
        return super.numberOfLegs;
    }
}
```

#### `Main.java`

```java
    ...
    Animal cat = new Cat();
    Animal dog = new Dog();
    System.out.println(cat.speak());
    System.out.println(dog.speak());
    ...
```

#### Preguntas propuestas

a) ¿Es correcto el uso de herencia en la implementación de las clases `Cat` y `Dog`? ¿Qué beneficios se obtienen?

El uso de la herencia, es correcto a la hora de proporcionar una interfaz común (Animal), para la cual implementar un funcionamiento específico para cada método según la subclase que extienda a Animal. Pese a esto, la herencia es mejorable, puesto que se especializa de más en el caso del método getNumberOfLegs, y además, el atributo numberOfLegs es intocable, sería necesario implementar un setter.

b) En el caso de que el uso de la herencia no sea correcto, proponga una solución alternativa. ¿Cuáles son los beneficios de la solución propuesta frente a la original?

Yo propondría los siguientes cambios en Animal.java, Cat.java y Dog.java:

#### `Animal.java`

```java
public abstract class Animal {
    //Number of legs the animal holds
    protected int numberOfLegs = 0;

    public abstract String speak();
    public abstract boolean eat(String typeOfFeed);
    public int getNumberOfLegs()
    {
        return numberOfLegs;
    }
    public void setNumberOfLegs(int numberOfLegs_)
    {
        this.numberOfLegs = numberOfLegs_;
    }
}
```

#### `Cat.java`

```java
public class Cat extends Animal {
    @Override
    public String speak() {
        return "Meow";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if(typeOfFeed.equals("fish")) {
            return true;
        } else {
            return false;
        }
    }
}
```

#### `Dog.java`

```java
public class Dog extends Animal {
    @Override
    public String speak() {
        return "Woof";
    }

    @Override
    public boolean eat(String typeOfFeed) {
        if(typeOfFeed.equals("meat")) {
            return true;
        } else {
            return false;
        }
    }
}
```

De esta manera tan solo se define el funcionamiento en subclase de los métodos que requieren un funcionamiento específico en ellas, y los métodos con un funcionamiento común entre subclases son definidos en al superclase.
