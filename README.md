# Clean Code y principios SOLID en Java

Repositorio de práctica sobre **código limpio, pruebas unitarias, TDD y diseño orientado a objetos**. Reúne ejercicios pequeños e independientes que permiten comparar implementaciones iniciales con versiones refactorizadas y explorar las decisiones detrás de cada principio SOLID.

No es una aplicación integral ni una API: cada directorio con `pom.xml` es un proyecto Maven separado.

## Stack y herramientas

- **Java**: programación orientada a objetos, interfaces, herencia, polimorfismo y excepciones personalizadas.
- **Biblioteca estándar de Java**: colecciones (`List` y `Map`), fechas y horas (`java.time`), operaciones matemáticas y lectura/escritura de archivos.
- **Maven**: gestión de dependencias y compilación de cada ejercicio.
- **JUnit Jupiter 5.7.0**: pruebas unitarias de la calculadora y del controlador de vuelos, con `@Test`, `@BeforeEach`, aserciones y verificación de excepciones.
- **Maven Surefire**: ejecución de las pruebas configuradas en los proyectos que incluyen JUnit.

Los proyectos de pruebas y SOLID configuran compilación para **Java 8**. Los proyectos introductorios no fijan una versión del compilador en su `pom.xml`; más abajo se indica cómo especificarla al ejecutarlos.

## Contenido

```text
CleanCode/
├── 1-introduccion-clean-code/
│   ├── introduccion-1/                 # Cálculo de área
│   ├── introduccion-1-refactorizado/
│   ├── introduccion-2/                 # Registro de empleados
│   └── introduccion-2-refactorizado/
├── 2_7-unit-tests/                     # Calculadora y pruebas unitarias
└── 4-tdd-example/                      # Gestión de vuelos y pruebas
SOLID/
├── 1-SRP/                              # Responsabilidad única
├── 2-OCP/                              # Abierto/cerrado
├── 3-LSP/                              # Sustitución de Liskov
├── 4-ISP/                              # Segregación de interfaces
└── 5-DIP/                              # Inversión de dependencias
```

### Clean Code

| Ejercicio | Qué se practica |
| --- | --- |
| [Introducción: cálculo de área](CleanCode/1-introduccion-clean-code/) | Comparación de una calculadora con su versión refactorizada, que explicita el cálculo del área de un triángulo rectángulo a partir de la hipotenusa y un cateto. |
| [Introducción: registro de empleados](CleanCode/1-introduccion-clean-code/) | Refactorización de un logger con nombres más descriptivos, métodos pequeños y responsabilidades separadas. Registra valores en días hábiles y, el primer día del mes, procesa el máximo del registro anterior. |
| [Pruebas unitarias](CleanCode/2_7-unit-tests/) | Multiplicación, división y valor absoluto; se prueban resultados y la excepción personalizada para división por cero. |
| [Ejemplo de TDD: vuelos](CleanCode/4-tdd-example/) | Gestión en memoria de vuelos: alta, eliminación y búsqueda por referencia, origen o rango de salida. Las pruebas incluyen referencias duplicadas y vuelos inexistentes. |

El ejemplo de vuelos usa `LocalDateTime` y una colección en memoria; no implementa una base de datos ni un servicio web. Los tests muestran casos de uso y límites, y sirven como punto de partida para seguir practicando TDD.

### Principios SOLID

Cada proyecto contiene los paquetes `badexample` y `refactored`, pensados para comparar el problema de diseño con una alternativa:

| Principio | Ejemplo |
| --- | --- |
| [**S** — Single Responsibility Principle](SOLID/1-SRP/) | Un libro y sus páginas se separan de la responsabilidad de guardar el libro en un archivo. |
| [**O** — Open/Closed Principle](SOLID/2-OCP/) | Una empresa trabaja con la abstracción `Employee`, permitiendo representar distintos tipos de empleado, como programadores y managers. |
| [**L** — Liskov Substitution Principle](SOLID/3-LSP/) | Se distingue un mamífero terrestre de un mamífero general: un perro puede caminar, mientras que un delfín no necesita implementar esa operación. |
| [**I** — Interface Segregation Principle](SOLID/4-ISP/) | Las operaciones básicas y trigonométricas se separan en interfaces para que cada calculadora implemente solo las que necesita. |
| [**D** — Dependency Inversion Principle](SOLID/5-DIP/) | `Company` recibe una fábrica de persistencia y depende de abstracciones, con implementaciones de ejemplo en memoria y para MySQL. |

La implementación `MySqlEmployeePersistence` es un **esqueleto didáctico**: no establece conexiones ni ejecuta consultas reales. En el ejemplo SRP, la persistencia de libros escribe archivos en `/tmp`.

## Requisitos

- JDK 8 y Apache Maven instalados y disponibles en la terminal. Java 8 es la referencia más directa para la configuración existente; con JDK más recientes, los plugins antiguos podrían presentar incompatibilidades.
- Acceso a los repositorios de Maven para descargar dependencias y plugins la primera vez.

Comprobá las instalaciones con:

```bash
java -version
mvn -version
```

## Compilación y pruebas

Desde la raíz del repositorio, ejecutá cada proyecto por separado con `mvn -f`:

```bash
# Pruebas de la calculadora
mvn -f CleanCode/2_7-unit-tests/pom.xml test

# Pruebas del ejemplo de vuelos
mvn -f CleanCode/4-tdd-example/pom.xml test

# Compilar un ejemplo SOLID
mvn -f SOLID/5-DIP/pom.xml package
```

Reemplazá la ruta del `pom.xml` para compilar otro ejercicio. Los proyectos SOLID tienen configuración de JUnit, pero actualmente no incluyen clases de prueba; `mvn test` en ellos no equivale a ejecutar los tests de Clean Code.

Para los proyectos introductorios, indicá explícitamente la versión de Java al compilar. Por ejemplo:

```bash
mvn -f CleanCode/1-introduccion-clean-code/introduccion-1-refactorizado/pom.xml \
  -Dmaven.compiler.source=1.8 -Dmaven.compiler.target=1.8 package
java -cp CleanCode/1-introduccion-clean-code/introduccion-1-refactorizado/target/classes \
  com.danielblanco.cleancode.Main
```

Este ejemplo imprime el área calculada para los valores definidos en su `Main`. Los dos ejercicios de registro también tienen una clase `Main`; ejecutarla puede crear, modificar o eliminar `log.txt` y modificar `lastMonthMax.txt` en el **directorio de trabajo actual**, según la fecha. Usá un directorio de trabajo de prueba si querés preservar los archivos existentes.

## Cómo recorrer los ejercicios

1. Compará cada implementación original con su versión refactorizada y buscá cambios en nombres, responsabilidades y dependencias.
2. Leé los tests junto con el código de la calculadora y de vuelos; agregá casos antes de modificar el comportamiento que quieras practicar.
3. En SOLID, recorré los cinco principios en orden y contrastá `badexample` con `refactored`. Probá extender cada modelo y observá qué clases necesitan cambios.

Son ejemplos educativos: algunas implementaciones son deliberadamente simples o incompletas y no deben interpretarse como soluciones listas para producción.
