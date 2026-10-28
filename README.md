# clase04-1bim

## Lectura de datos con `Scanner`: uso de `nextLine()` y `nextDouble()`

### 1. Descripción del programa

El programa `Ejemplo2.java` permite ingresar por teclado los nombres y las calificaciones de dos asignaturas. Para ello, utiliza la clase `Scanner` de Java.

En este ejemplo se trabajan los siguientes conceptos:

- Declaración de variables de tipo `String` y `double`.
- Ingreso de datos mediante `Scanner`.
- Lectura de cadenas con `nextLine()`.
- Lectura de números decimales con `nextDouble()`.
- Uso de `Locale.US` para interpretar números decimales.
- Presentación de resultados mediante `printf()`.

### 2. Lectura de datos

Para ingresar información por teclado se crea un objeto de tipo `Scanner`:

```java
Scanner entrada = new Scanner(System.in);
entrada.useLocale(Locale.US);
```

La instrucción `entrada.useLocale(Locale.US)` permite que `nextDouble()` interprete números decimales utilizando el punto (`.`) como separador decimal.

El programa utiliza dos métodos de lectura:

**`nextLine()`**: permite leer una línea completa de texto, incluyendo espacios, hasta encontrar un salto de línea.

```java
nombreAsignatura1 = entrada.nextLine();
```

**`nextDouble()`**: permite leer un número decimal.

```java
notaAsignatura1 = entrada.nextDouble();
```

### 3. ¿Por qué es necesario utilizar `entrada.nextLine()` después de `nextDouble()`?

Observe el siguiente fragmento:

```java
System.out.println("Ingrese nota de asignatura 1");
notaAsignatura1 = entrada.nextDouble();

System.out.println("Ingrese nombre de asignatura 2");
entrada.nextLine(); // consumir el salto de línea pendiente
nombreAsignatura2 = entrada.nextLine();
```

Cuando el usuario introduce un número, por ejemplo:

```text
9.50
```

Y presiona **Enter**, la entrada contiene conceptualmente:

```text
9.50\n
```

Donde `\n` representa el salto de línea.

El método `nextDouble()` lee únicamente el número `9.50`, pero **no consume el salto de línea generado al presionar Enter**.

Por tanto:

```text
Entrada del usuario:  9.50\n
nextDouble() lee:     9.50
Queda pendiente:      \n
```

Si inmediatamente se ejecuta:

```java
nombreAsignatura2 = entrada.nextLine();
```

El método encuentra el salto de línea pendiente, lo consume y devuelve una cadena vacía (`""`).

En consecuencia, el programa continúa sin permitir que el usuario escriba el nombre de la segunda asignatura.

### 4. ¿Cómo solucionamos este problema?

Se incorpora una llamada adicional a `nextLine()`:

```java
entrada.nextLine(); // consumir el salto de línea pendiente
```

Esta instrucción consume el contenido restante de la línea anterior, incluido el salto de línea que quedó pendiente después de `nextDouble()`.

De esta manera, la siguiente instrucción puede leer correctamente el nombre de la segunda asignatura:

```java
nombreAsignatura2 = entrada.nextLine();
```

**Importante:** Aunque frecuentemente se utiliza el comentario `// limpieza de buffer`, técnicamente esta instrucción no vacía todo el búfer de entrada. Su función es consumir el resto de la línea actual.

### 5. Diferencia entre `nextDouble()` y `nextLine()`

| Método | Funcionamiento |
|---|---|
| `nextDouble()` | Lee un número decimal y deja pendiente el salto de línea posterior. |
| `nextInt()` | Lee un número entero y deja pendiente el salto de línea posterior. |
| `nextLine()` | Lee el contenido hasta el siguiente salto de línea y consume dicho salto. |

Cuando se combinan métodos como `nextInt()` o `nextDouble()` con `nextLine()`, es necesario prestar atención a los saltos de línea pendientes.

### 6. Ejemplo de ejecución

```text
Ingrese nombre de asignatura 1
Matemáticas
Ingrese nota de asignatura 1
9.50
Ingrese nombre de asignatura 2
Programación
Ingrese nota de asignatura 2
8.75
```

El programa presenta los resultados:

```text
Asignatura 1: Matemáticas
Nota 1: 9.50
Asignatura 2: Programación
Nota 2: 8.75
```

El formato decimal mostrado puede variar según la configuración regional utilizada por `printf()`.


