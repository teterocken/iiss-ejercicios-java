# Práctica 1: Anónimos & Cierres

## Ejercicios propuestos

### Ejercicio 1

Dado los siguientes fragmentos de código que implementan un API dado por la interfaz `DataOperations`, responder a las siguientes preguntas:

#### `DataOperations.java`

```java
public interface DataOperations {
    public void print(int[] data);
    public int[] filterPairs(int[] data);
}
```

#### `DataOperationsImpl.java`

```java
public class DataOperationsImpl implements DataOperations {
    @Override
    public void print(int[] data) {
        for(int element: data) {
            System.out.print(element + ", ");
        }
        System.out.println();
    }

    @Override
    public int[] filterPairs(int[] data) {
        int index = 0;
        int[] dataAux = new int[data.length];
        for(int element: data) {
            if((element % 2) != 0) {
                dataAux[index] = element;
                index++;
            }
        }
        return dataAux;
    }
}
```

#### `Main.java`

```java
import java.util.Arrays;

public class Main {
    public static void main(String args[]) {
        int[] data = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        System.out.println("data = " + Arrays.toString(data));
        DataOperations operations = new DataOperationsImpl();
        operations.print(data);
        data = operations.filterPairs(data);
        operations.print(data);
    }
}
```

#### Preguntas propuestas

1. Utilice expresiones *lambda* y el API de *streams* de Java para cambiar la implementación de las operaciones de la interfaz `DataOperations` usando los mecanismos de la programación funcional.

2. Además, haciendo uso de expresiones *labmda* y del API de *streams*, añada a la interfaz de `DataOperations` las siguientes operaciones y su implementación:

- Operación que devuelva la lista de números ordenada descendentemente.
- Operación que multiplique todos los números de la lista por 10 e imprima el resultado.
- Operación que devuelva el resultado de la suma de todos los números de la lista.

#### `DataOperationsImpl.java`

```java
import java.util.function.*;
import java.util.stream.Collectors;
import java.util.Comparator;
import java.util.List;
import java.util.Arrays;
public class DataOperationsImpl implements DataOperations {
    Function<int[], Void> print = (int[] data) -> {for(int element: data) System.out.print(element + ", "); System.out.println();
    return null;
    };

    @Override
    public void print(int[] data)
    {
        print.apply(data);
    }

    Function<int[], int[]> filterPairs = (int[] data) -> {List<Integer> lista = Arrays.stream(data).filter(n->n%2!=0).boxed().collect(Collectors.toList());
        for (int element: data) if (element%2==0) lista.add(0);
        return lista.stream().mapToInt(Integer::intValue).toArray();};
        

    @Override
    public int[] filterPairs(int[] data) {
        return filterPairs.apply(data);
    }

    Function<int[], int[]> orderDesc = (int[] data) -> {return Arrays.stream(data).boxed().sorted(Comparator.reverseOrder()).mapToInt(Integer::intValue).toArray();};

    @Override
    public int[] orderDesc(int[] data)
    {
        return orderDesc.apply(data);
    }

    Function<int[], Void> multiplyByTen = (int[] data) -> {print(Arrays.stream(data).map(n->n*10).toArray());
        return null;
    };

    public void multiplyByTen(int[] data)
    {
        multiplyByTen.apply(data);
    }

    Function<int[], Integer> sumElements = (int[] data) -> {return Arrays.stream(data).sum();};

    public int sumElements(int[] data)
    {
        return sumElements.apply(data);
    }
}
```

### Ejercicio 2

Dado los siguientes fragmentos de código que implementan un API cuya interfaz viene dada por `DataSorter`, responder a las siguientes preguntas.

#### `DataSorter.java`

```java
public interface DataSorter {
    public String[] sort(String[] data);
}
```

#### `DataSorterAsc.java`

```java
import java.util.Arrays;

public class DataSorterAsc implements DataSorter {
    public String[] sort(String[] data) {
        Arrays.sort(data);
        return data;
    }
}
```

#### `DataSorterDesc.java`

```java
import java.util.Arrays;
import java.util.Collections;

public class DataSorterDesc implements DataSorter {
    public String[] sort(String[] data) {
        Arrays.sort(data, Collections.reverseOrder());
        return data;
    }
}

```

#### `Main.java`

```java
import java.util.Arrays;

public class Main {
    public static void main(String args[]) {
        String [] data = {"H", "S", "I", "V", "E", "W", "M", "P", "L",  "C", "N", "K",
                 "O", "A", "Q", "R", "J", "D", "G", "T", "U", "X", "B", "Y", "Z", "F"};
        System.out.println("data = " + Arrays.toString(data));
        DataSorter dataSorter = new DataSorterDesc();
        dataSorter.sort(data);
        System.out.println("data (desc) = " + Arrays.toString(data));
        dataSorter = new DataSorterAsc();
        dataSorter.sort(data);
        System.out.println("data (asc) = " + Arrays.toString(data));
    }
}
```

#### Preguntas propuestas

1. Utilice cierres (*closures*) para cambiar la implementación de las clases `DataSorterAsc` y `DataSorterDesc` usando los mecanismos de la programación funcional.

2. Añada un tercer cambio haciendo uso de cierres (*closures*) para realizar la ordenación aleatoria de los elementos, siguiendo el mismo enfoque aplicado con las clases `DataSorterAsc` y `DataSorterDesc` en el apartado anterior.

#### `DataSorter.java`

```java
public interface DataSorter {
    public String[] sort();
}
```

#### `DataSorterAsc.java`

```java
import java.util.Arrays;

public class DataSorterAsc implements DataSorter {
    private String[] data;
    public DataSorterAsc(String[] data)
    {
        this.data = data;
    }
    public String[] sort() {
        Arrays.sort(data);
        return data;
    }
}
```

#### `DataSorterDesc.java`

```java
import java.util.Arrays;
import java.util.Collections;

public class DataSorterDesc implements DataSorter {
    private String[] data;
    public DataSorterDesc(String[] data)
    {
        this.data = data;
    }
    public String[] sort() {
        Arrays.sort(data, Collections.reverseOrder());
        return data;
    }
}
```

#### `DataSorterRandom.java`

```java
import java.util.Arrays;
import java.util.Collections;

public class DataSorterRandom implements DataSorter {
    private String[] data;
    public DataSorterRandom(String[] data)
    {
        this.data = data;
    }
    public String[] sort() {
        Collections.shuffle(Arrays.asList(data));
        return data;
    }
}
```

#### `Main.java`

```java
import java.util.Arrays;

public class Main {
    public static void main(String args[]) {
        String [] data = {"H", "S", "I", "V", "E", "W", "M", "P", "L",  "C", "N", "K",
                 "O", "A", "Q", "R", "J", "D", "G", "T", "U", "X", "B", "Y", "Z", "F"};
        System.out.println("data = " + Arrays.toString(data));
        DataSorter dataSorter = new DataSorterDesc(data);
        dataSorter.sort();
        System.out.println("data (desc) = " + Arrays.toString(data));
        dataSorter = new DataSorterAsc(data);
        dataSorter.sort();
        System.out.println("data (asc) = " + Arrays.toString(data));
        dataSorter = new DataSorterRandom(data);
        dataSorter.sort();
        System.out.println("data (rand) = " + Arrays.toString(data));
    }
}
```

## Referencias

[Java 8 Stream Tutorial]: https://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/
[[1] Blog: Java 8 Stream Tutorial.][Java 8 Stream Tutorial]
[API Java Streams]: https://www.oracle.com/technetwork/es/articles/java/procesamiento-streams-java-se-8-2763402-esa.html
[[2] Documentación Oficial Java: Procesamiento de datos con streams de Java SE 8.][API Java Streams]
[API Java Funciones Lambda + Streams]: https://www.oracle.com/technetwork/es/articles/java/expresiones-lambda-api-stream-java-2737544-esa.html
[[3] Documentación Oficial Java: Introducción Expresiones Lambda y API Stream en Java SE 8.][API Java Funciones Lambda + Streams]
[Closures in Java]: https://medium.com/@rmanavalan/5-ways-to-implement-closures-in-java-8-590790659ac5
[[4] Blog: 5 ways to implement Closures in Java 8.][Closures in Java]
