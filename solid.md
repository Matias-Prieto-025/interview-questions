# SOLID

### Single Responsibility Principle

El principio SRP defiende que una clase debería centrarse en tener una única responsabilidad, es decir, que sus métodos tengan alta cohesión.

```javascript
// Viola el principio de SRP porque estamos mezclando los settings del usuario con su autenticación.
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}


// Cada clase tiene una unica responsabilidad
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

### Principio de Abierto Cerrado (Open / Closed Principle)

El principio OCP explica que nuestras clases deben estar abiertas para ser extendidas pero cerradas para ser modificadas. Parece algo complejo pero básicamente podemos decir que debemos abstraer nuestras dependencias.

```javascript
class Png {
  constructor(name) {
    this.name = name;
  }

  // ...
}
class Printer {
  function printFile(file) {
    if (file instanceof Pdf) {
      // Print Pdf...
    } else if (file instanceof Png) {
      // Print Png...
    }
  }
}
```

printFile recibe un objeto que nos obliga a comprobar el tipo para poder imprimir cada archivo.

```javascript
class Printable {
  function print() {
    // ...
  }
}

class Pdf extends Printable {
  constructor(name, size) {
    super();
    this.name = name;
    this.size = size;
  }

  // Override
  function print() {
    // ...
  }
}

class Png extends Printable {
  constructor(name) {
    super();
    this.name = name;
  }

  // Override
  function print() {
    // ...
  }
}

class Printer {
  function printFile(file) {
    file.print();
  }
}

```

Ahora podríamos crear una clase Jpeg que extienda de Printable y no tendríamos que modificar nada del código que ya teníamos escrito, sólo implementar su propia función print.

### Principio de Sustitución de Liskow (Liskov Substitution Principle)

Liskov explicaba que si tenemos un objeto de tipo A y otro de tipo B, siendo B una subclase de A, se deberían poder reemplazar los objetos de tipo B por los de tipo A. En resumen, los subtipos deberían poder ser reemplazables por sus tipos base.

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  constructor(size) {
    super(size, size);
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach((rectangle) => {
    const area = rectangle.getArea();
  });
}

const rectangles = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(4)];
renderLargeRectangles(rectangles);
```

### Principio de Segregación de Interfaces (Interface Segregation Principle)

“Haz interfaces que sean específicas para un tipo de cliente”, es decir, para una finalidad concreta.

En este sentido, según el Interface Segregation Principle (ISP), es preferible contar con muchas interfaces que definan pocos métodos que tener una interface forzada a implementar muchos métodos a los que no dará uso.


```javascript

class Car {
  startEngine() {}
  accelerate() {}
  backToThePast() {}
  backToTheFuture() {}
}

class Seat extends Car {
  startEngine() {
    // start engine...
  }
  accelerate() {
    // accelerate...
  }
}

class DeloRean extends Car {
  startEngine() {
    // start engine...
  }
  accelerate() {
    // accelerate...
  }
  backToThePast() {
    // back to the past...
  }
  backToTheFuture() {
    // back to the future...
  }
}

```
Al incluir en la clase abstracta Car los métodos de una clase concreta, en este caso DeloRean, obligamos a que todas las clases que implementan nuestra interfaz o clase abstracta también implementen esos métodos.

Al separar las interfaces podemos implementar concreciones mucho mejor.

En Javascript no se puede extender de varias interfaces a la vez por lo que este principio no aplica a la perfección (pero sí en Typescript). Podríamos hacer que TimeMachine implemente Car. No es muy idiomático la herencia en sí en Javascript, un work aroundla podria ser la composición a la herencia. Podríamos hacer una mezcla para implementar un ejemplo de este principio:


```javascript
class Car {
  startEngine() {}
  accelerate() {}
}

class Seat extends Car {
  startEngine() {
    // start engine...
  }
  accelerate() {
    // accelerate...
  }
}

const TimeMachine = Super => class extends Super {
  backToThePast() {}
  backToTheFuture() {}
};

class DeloRean extends TimeMachine(Car) {
  startEngine() {
    // start engine...
  }
  accelerate() {
    // accelerate...
  }
  backToThePast() {
    // back to the past...
  }
  backToTheFuture() {
    // back to the future...
  }
}
```

### Principio de Inversión de Dependencias (Dependency Inversion Principle)

“Depende de abstracciones, no de clases concretas”.

Los módulos de alto nivel no deberían depender de módulos de bajo nivel. Ambos deberían depender de abstracciones.

Las abstracciones no deberían depender de los detalles. Los detalles deberían depender de las abstracciones.

El objetivo del Dependency Inversion Principle (DIP) consiste en reducir las dependencias entre los módulos del código, es decir, alcanzar un bajo acoplamiento de las clases.

Como es un lenguaje dinámico, JavaScript no requiere el uso de abstracciones para facilitar el desacoplamiento. Por lo tanto, la estipulación de que las abstracciones no deben depender de los detalles no es particularmente relevante para aplicaciones de JavaScript. Sin embargo, la estipulación de que los módulos de alto nivel no deben depender de módulos de bajo nivel si que lo es.

# DRY

“Don’t repeat yourself”.

Cada pieza de funcionalidad debe tener una única, no ambigua y representativa identidad dentro del sistema.

Básicamente lo que se intenta evitar con este principio es que no se duplique el código, porque lo que ocurre luego que el mantenimiento será mucho más difícil ya que no sabremos donde tenemos que cambiar cosas porque no están definidas claramente.

# KISS

“Keep it simple, Stupid!”

Este patrón lo que nos dice es que cualquier sistema va a funcionar mejor si se mantiene sencillo que si se vuelve complejo. Es decir que la sencillez tiene que ser una meta en el desarrollo y que la complejidad innecesaria debe ser eliminada.