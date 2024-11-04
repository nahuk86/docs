## Patron Decorator

El **patrón Decorador** es un patrón de diseño estructural que permite agregar funcionalidades adicionales a un objeto de manera dinámica, sin modificar su estructura original. Es una alternativa flexible a la herencia para extender el comportamiento de las clases.

### Conceptos Clave del Patrón Decorador

1. **Extensión de Funcionalidad**:
   - Permite añadir nuevas funcionalidades a un objeto ya existente sin alterar su código.
   - Se pueden combinar múltiples decoradores para añadir diversas funcionalidades.

2. **Composición sobre Herencia**:
   - Promueve el uso de composición en lugar de herencia. Los objetos son envueltos por otros objetos que agregan nuevas funcionalidades.

3. **Interfaz Común**:
   - Tanto el objeto original como los decoradores implementan una interfaz común, lo que permite que los decoradores puedan ser intercambiables o encadenados.

### Estructura del Patrón Decorador

1. **Componente (Component)**:
   - Define una interfaz común para los objetos que pueden tener responsabilidades adicionales.
   
2. **Componente Concreto (ConcreteComponent)**:
   - Es un objeto principal que puede recibir nuevas responsabilidades dinámicamente.

3. **Decorador (Decorator)**:
   - Mantiene una referencia a un objeto `Component` y define una interfaz que conforma a la del componente.
   
4. **Decoradores Concretos (ConcreteDecorators)**:
   - Extienden la funcionalidad del componente, añadiendo responsabilidades adicionales.

### Ejemplo en .NET Framework y C#

Imaginemos un sistema que maneja textos, y queremos agregar características como convertir el texto a mayúsculas y agregar exclamaciones, sin modificar la clase original que maneja el texto.

#### Paso 1: Definir la Interfaz `IText`

```csharp
public interface IText
{
    string GetContent();
}
```

#### Paso 2: Crear el Componente Concreto

```csharp
public class SimpleText : IText
{
    private string _content;

    public SimpleText(string content)
    {
        _content = content;
    }

    public string GetContent()
    {
        return _content;
    }
}
```

#### Paso 3: Crear la Clase Decorador Base

```csharp
public abstract class TextDecorator : IText
{
    protected IText _text;

    public TextDecorator(IText text)
    {
        _text = text;
    }

    public virtual string GetContent()
    {
        return _text.GetContent();
    }
}
```

#### Paso 4: Implementar Decoradores Concretos

**Mayúsculas Decorador:**

```csharp
public class UpperCaseDecorator : TextDecorator
{
    public UpperCaseDecorator(IText text) : base(text) { }

    public override string GetContent()
    {
        return _text.GetContent().ToUpper();
    }
}
```

**Exclamación Decorador:**

```csharp
public class ExclamationDecorator : TextDecorator
{
    public ExclamationDecorator(IText text) : base(text) { }

    public override string GetContent()
    {
        return _text.GetContent() + "!";
    }
}
```

#### Paso 5: Utilizar los Decoradores

```csharp
class Program
{
    static void Main(string[] args)
    {
        IText simpleText = new SimpleText("Hello, world");
        IText upperCaseText = new UpperCaseDecorator(simpleText);
        IText excitedText = new ExclamationDecorator(upperCaseText);

        Console.WriteLine(simpleText.GetContent());       // Output: Hello, world
        Console.WriteLine(upperCaseText.GetContent());    // Output: HELLO, WORLD
        Console.WriteLine(excitedText.GetContent());      // Output: HELLO, WORLD!
    }
}
```

### Resultado

La salida de la ejecución será:

```
Hello, world
HELLO, WORLD
HELLO, WORLD!
```

### Beneficios del Patrón Decorador

1. **Extensión Dinámica**:
   - Agrega comportamientos dinámicamente a los objetos sin modificar su código base.

2. **Combinación Flexible**:
   - Puedes combinar múltiples decoradores para crear nuevas combinaciones de funcionalidades.

3. **Aumenta la Reutilización**:
   - Los decoradores pueden ser reutilizados con diferentes componentes, lo que promueve un diseño modular.

### Conclusión

El patrón Decorador es ideal cuando necesitas agregar funcionalidades a objetos de manera dinámica y flexible. Esto te permite evitar la proliferación de subclases y mantener un código más limpio y modular.


-----------------

## Patrón Facade

El **patrón Facade** es un patrón de diseño estructural que proporciona una interfaz simplificada para un conjunto de interfaces más complejas en un subsistema. Este patrón es útil para ocultar las complejidades de un sistema y proporcionar una interfaz más amigable o fácil de usar. 

### Conceptos Clave del Patrón Facade

1. **Simplificación de la Interacción**:
   - Proporciona una interfaz de alto nivel que hace que el subsistema sea más fácil de usar.
   - Oculta los detalles internos y la complejidad de un sistema, exponiendo solo lo que es necesario.

2. **Reducción del Acoplamiento**:
   - Desacopla el código del cliente del subsistema más complejo, facilitando el mantenimiento y la escalabilidad del sistema.

3. **Coordinación de Subsistemas**:
   - Coordina las interacciones entre diferentes componentes del subsistema, haciendo que el cliente no necesite preocuparse por cómo estos interactúan entre sí.

### Ejemplo en .NET Framework y C#

Imaginemos que estamos desarrollando un sistema que gestiona la reproducción de música. Este sistema tiene varias clases internas para cargar archivos, decodificar formatos, y controlar el audio. 

#### Paso 1: Subsistema Complejo

```csharp
public class FileLoader
{
    public void Load(string fileName)
    {
        Console.WriteLine($"Loading file: {fileName}");
    }
}

public class AudioDecoder
{
    public void Decode(string fileName)
    {
        Console.WriteLine($"Decoding audio file: {fileName}");
    }
}

public class AudioPlayer
{
    public void Play()
    {
        Console.WriteLine("Playing audio...");
    }
}
```

#### Paso 2: Crear la Facade

```csharp
public class MusicPlayerFacade
{
    private FileLoader _fileLoader;
    private AudioDecoder _audioDecoder;
    private AudioPlayer _audioPlayer;

    public MusicPlayerFacade()
    {
        _fileLoader = new FileLoader();
        _audioDecoder = new AudioDecoder();
        _audioPlayer = new AudioPlayer();
    }

    public void PlayMusic(string fileName)
    {
        _fileLoader.Load(fileName);
        _audioDecoder.Decode(fileName);
        _audioPlayer.Play();
    }
}
```

#### Paso 3: Utilizar la Facade

```csharp
class Program
{
    static void Main(string[] args)
    {
        MusicPlayerFacade musicPlayer = new MusicPlayerFacade();
        musicPlayer.PlayMusic("song.mp3");
    }
}
```

### Resultado Esperado

Cuando ejecutas este código, verás la siguiente salida en la consola:

```
Loading file: song.mp3
Decoding audio file: song.mp3
Playing audio...
```

### Beneficios del Patrón Facade

1. **Simplifica la Interfaz del Cliente**:
   - En lugar de interactuar con varias clases, el cliente solo interactúa con la `MusicPlayerFacade`.

2. **Oculta la Complejidad Interna**:
   - Los detalles de cómo se cargan, decodifican y reproducen los archivos de audio están ocultos detrás de la fachada.

3. **Mejor Mantenibilidad**:
   - Cambios en las clases internas del subsistema no afectan al cliente siempre que la interfaz de la fachada permanezca constante.

### Conclusión

El patrón Facade es ideal cuando necesitas simplificar la interacción con sistemas complejos o legacy, proporcionando una única interfaz de acceso, mejorando la facilidad de uso y manteniendo el bajo acoplamiento entre sistemas.


---------------------
## patrón Singleton

El **patrón Singleton** es un patrón de diseño creacional que asegura que una clase tenga **una única instancia** y proporciona un punto de acceso global a esa instancia. Es útil cuando necesitas garantizar que un objeto particular se cree una sola vez y se utilice en todo el sistema.

### Conceptos Clave del Patrón Singleton

1. **Instancia Única**:
   - Garantiza que solo exista una única instancia de la clase en todo el ciclo de vida de la aplicación.
   
2. **Acceso Global**:
   - Proporciona un punto de acceso global a esa instancia, lo que significa que la misma instancia se comparte y reutiliza.

3. **Control de Instanciación**:
   - El control de instanciación se realiza dentro de la propia clase, evitando que otras partes del código creen nuevas instancias.

### Casos de Uso Comunes

- **Configuraciones**: Para gestionar configuraciones globales de la aplicación.
- **Conexiones a Bases de Datos**: Para gestionar una única conexión a la base de datos.
- **Logging**: Para mantener un único logger que registre todas las actividades de la aplicación.

### Implementación en C# (.NET Framework)

Aquí te muestro una implementación clásica del patrón Singleton en C#:

#### Paso 1: Crear la Clase Singleton

```csharp
public class Singleton
{
    // Campo estático para la única instancia de la clase.
    private static Singleton _instance;

    // Constructor privado para evitar la instanciación directa.
    private Singleton()
    {
        Console.WriteLine("Singleton Instance Created");
    }

    // Propiedad para obtener la instancia única.
    public static Singleton Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new Singleton();
            }
            return _instance;
        }
    }

    // Método de ejemplo
    public void DoSomething()
    {
        Console.WriteLine("Singleton is working!");
    }
}
```

#### Paso 2: Usar el Singleton

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Intentamos obtener la misma instancia varias veces.
        Singleton instance1 = Singleton.Instance;
        Singleton instance2 = Singleton.Instance;

        instance1.DoSomething();

        // Verificamos si ambas instancias son iguales.
        if (instance1 == instance2)
        {
            Console.WriteLine("Both instances are the same.");
        }
        else
        {
            Console.WriteLine("Instances are different.");
        }
    }
}
```

### Resultado Esperado

Cuando ejecutas este código, deberías ver algo como:

```
Singleton Instance Created
Singleton is working!
Both instances are the same.
```

### Características Importantes

1. **Constructor Privado**:
   - Previene que otras clases instancien el Singleton directamente.

2. **Instancia Estática**:
   - La instancia es almacenada en un campo estático para asegurar que haya solo una instancia.

3. **Lazily Initialized**:
   - La instancia se crea solo cuando es necesaria (al primer acceso).

### Variaciones y Mejoras

- **Hilo Seguro (Thread-Safe)**:
  En aplicaciones concurrentes, se puede mejorar el Singleton para hacerlo seguro para hilos utilizando técnicas como el bloqueo (lock):

  ```csharp
  private static readonly object _lock = new object();
  
  public static Singleton Instance
  {
      get
      {
          lock (_lock)
          {
              if (_instance == null)
              {
                  _instance = new Singleton();
              }
              return _instance;
          }
      }
  }
  ```

- **Versión Eager**:
  La instancia puede ser creada al cargar la clase si sabes que siempre la necesitarás:

  ```csharp
  private static readonly Singleton _instance = new Singleton();
  
  public static Singleton Instance => _instance;
  ```

### Beneficios del Patrón Singleton

1. **Control Estricto de la Instancia**:
   - Garantiza que solo haya una instancia del objeto.

2. **Acceso Global**:
   - Facilita el acceso a la instancia desde cualquier parte del código.

3. **Ahorro de Recursos**:
   - Puede ahorrar recursos al compartir la misma instancia entre múltiples partes de la aplicación.

### Conclusión

El patrón Singleton es útil para escenarios donde necesitas una única instancia global que sea accesible desde diferentes partes de la aplicación. Aunque es poderoso, se debe usar con cuidado para evitar problemas de acoplamiento excesivo y dificultades en las pruebas unitarias.


------------------------

## patrón Composite

El **patrón Composite** es un patrón de diseño estructural que se utiliza para representar **jerarquías de objetos** de manera que los objetos individuales (hojas) y los grupos de objetos (compuestos) se traten de la misma manera. Es ideal para trabajar con estructuras de tipo árbol, como sistemas de archivos, menús de aplicaciones, o jerarquías de productos en un catálogo.

### Conceptos Clave del Patrón Composite

1. **Composición Recursiva**:
   - Permite que los clientes traten objetos individuales y composiciones de objetos de manera uniforme.

2. **Componente (Component)**:
   - Define una interfaz común para todos los objetos en la jerarquía.

3. **Hoja (Leaf)**:
   - Representa objetos individuales que no tienen descendientes. Implementa la interfaz del componente.

4. **Compuesto (Composite)**:
   - Representa un grupo de objetos que pueden contener hojas o más compuestos. Implementa la interfaz del componente y maneja sus hijos.

### Estructura del Patrón Composite

1. **Component**: Define la interfaz común para hojas y compuestos.
2. **Leaf**: Implementa la interfaz del componente y no tiene hijos.
3. **Composite**: Implementa la interfaz del componente y maneja la agregación de hijos.

### Ejemplo en .NET Framework y C#

Supongamos que queremos modelar una estructura de carpetas y archivos, donde cada carpeta puede contener otros archivos y carpetas.

#### Paso 1: Definir la Interfaz `IComponent`

```csharp
public interface IComponent
{
    void Display(int depth);
}
```

#### Paso 2: Crear la Clase `Leaf` (Archivo)

```csharp
public class File : IComponent
{
    private string _name;

    public File(string name)
    {
        _name = name;
    }

    public void Display(int depth)
    {
        Console.WriteLine(new string('-', depth) + _name);
    }
}
```

#### Paso 3: Crear la Clase `Composite` (Carpeta)

```csharp
public class Directory : IComponent
{
    private string _name;
    private List<IComponent> _children = new List<IComponent>();

    public Directory(string name)
    {
        _name = name;
    }

    public void Add(IComponent component)
    {
        _children.Add(component);
    }

    public void Remove(IComponent component)
    {
        _children.Remove(component);
    }

    public void Display(int depth)
    {
        Console.WriteLine(new string('-', depth) + _name);
        foreach (var child in _children)
        {
            child.Display(depth + 2);
        }
    }
}
```

#### Paso 4: Utilizar el Patrón Composite

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Crear hojas
        File file1 = new File("File1.txt");
        File file2 = new File("File2.txt");
        File file3 = new File("File3.txt");

        // Crear compuestos
        Directory root = new Directory("Root");
        Directory subdir1 = new Directory("Subdir1");
        Directory subdir2 = new Directory("Subdir2");

        // Armar la jerarquía
        root.Add(file1);
        root.Add(subdir1);
        subdir1.Add(file2);
        subdir1.Add(subdir2);
        subdir2.Add(file3);

        // Mostrar la estructura
        root.Display(1);
    }
}
```

### Resultado Esperado

Cuando ejecutas este código, deberías ver la siguiente salida en la consola:

```
-Root
--File1.txt
--Subdir1
----File2.txt
----Subdir2
------File3.txt
```

### Beneficios del Patrón Composite

1. **Tratar Objetos y Composiciones Uniformemente**:
   - Puedes manejar objetos individuales y composiciones de objetos con la misma lógica.

2. **Facilita la Extensión**:
   - Es fácil agregar nuevos tipos de componentes o compuestos sin cambiar el código existente.

3. **Flexible y Reutilizable**:
   - Puedes reutilizar y extender los componentes fácilmente, ya que la jerarquía es manejada de forma uniforme.

### Casos de Uso Comunes

- **Sistemas de Archivos**: Directorios y archivos.
- **Interfaz de Usuario**: Contenedores y componentes de UI.
- **Menús**: Submenús y elementos de menú.
- **Gráficos**: Formas complejas construidas a partir de formas simples.

### Conclusión

El patrón Composite es ideal para trabajar con estructuras jerárquicas complejas y es particularmente útil cuando necesitas tratar tanto elementos individuales como grupos de elementos de manera uniforme. Esto permite escribir código más limpio, más modular, y más fácil de mantener.
