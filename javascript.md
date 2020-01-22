## Que es ECMAScript y que javascrip?

Es la especificación definida en ECMA-262, para crear un lenguaje de scripting de propósito general. Sinónimo: ECMAScript specification

ECMAScript proporciona las reglas, detalles y directrices que un lenguaje de scripting debe seguir, para que se considere que cumple con ECMAScript.

Un lenguaje de scripting de propósito general, que se ajusta a la especificación ECMAScript.

## Closure

Un closure o clausura es la combinación de una función y el ámbito léxico en el que se declaró dicha función. Es decir los closures o clausuras son funciones que manejan variables independientes. En otras palabras, la función definida en el closure "recuerda" el ámbito en el que se ha creado.

```javascript
function padre() {
  var a = 1;
  function closure() {
    console.log(a);
  }
  closure();
}
padre();
```

La función padre() crea una variable local y una función closure(). Esta función interna es un closure y solo esta disponible dentro de padre(). A diferencia de padre() esta función no tiene variables locales y usa las declaradas dentro de padre().

## IIFE (Inmediately Invoked Function Expression)

Funciones que se ejecutan tan pronto como se definen.

Se compone por dos partes. La primera es la función anónima con alcance léxico encerrado por el  Operador de Agrupación (). Esto impide accesar variables fuera del IIFE, así cómo contaminar el alcance (scope) global. 

La segunda parte crea la expresión de función cuya ejecución es inmediata (), siendo interpretado directamente en el engine de JavaScript.

```javascript
(function () { 
    var aName = "Barry";
})();
// Variable name is not accessible from the outside scope
aName // throws "Uncaught ReferenceError: aName is not defined"
```

```javascript
function crearSuma(a) {
  return funcion(b) {
    return a + b
  }
}
crearSuma(5)(10); // 15
```

## Ambito lexico

En JavaScript las funciones tienen su propio ámbito léxico, lo que quiere decir que depende de cómo son declaradas en el código y no de cuando se ejecutan.

Sólo las funciones pueden crear un nuevo ámbito y como excepción, los bloques catch también crean su propio ámbito. Con la introducción de las variables let y const en ES6, también tenemos a nuestra disposición ámbito de bloque, ver más abajo para ver qué significa esto.

## var / let / const

* __var__

Las variables declaradas con var son procesadas antes de la ejecución del código (hoisting).
El scope de una variable declarada con var, es su contexto de ejecución.
El scope de una variable declarada fuera de la función es global.
Si declaramos la variable dentro de una función IIFE, el valor de nuestra variable vivirá solamente dentro del scope de esta función.
```javascript
var i = 60;
(function explainVar(){
 for( var i = 0; i < 5; i++){
  console.log(i)                   //Output 0, 1, 2, 3, 4
 }
})(); 
console.log("Despues del loop", i) // Output 60
```
Si olvidas declarar una variable dentro de tu función o loop (en este caso no declaramos i dentro del for), el interpreter de Javascript la va a declarar de forma global. El valor de i a nivel global será reasignado por el for loop.

```javascript
var i = 60;
(function explainVar(){
 for( i = 0; i < 5; i++){
  console.log(i)                   //Output 0, 1, 2, 3, 4
 }
})(); 
console.log("Despues del loop", i) // Output 5
```
* __let__

Si una variable es declarada con let en el ámbito global o en el de una función, la variable pertenecerá al ámbito global o al ámbito de la función respectivamente, de forma similar a como ocurría con var.
Pero si declaramos una variable con let dentro un bloque que a su vez está dentro de una función, la variable pertenece solo a ese bloque

```javascript
function foo() {
    let i = 0;
    if(true) {
        let i = 1; // Sería otra variable i solo para el bloque if
        console.log(i); // 1
    }
    console.log(i); // 0
}
foo();
```

Fuera del bloque donde se declara, la variable no esta definida

```javascript
function foo() {
    if(true) {
        let i = 1;
    }
    console.log(i); // ReferenceError: i is not defined
}
foo();
```

* __const__

El ámbito o scope para una variable declarada con const es, al igual que con let, el bloque, pero si la declaración con let previene la sobreescritura de variables, const directamente prohíbe la reasignación de valores.

```javascript
const i = 0;
i = 1; // TypeError: Assignment to constant variable
```

Pero que no se puedan reasignar no significa que sean inmutables. Si el valor de una variable constante es mutable (como un array o un objeto) se pueden cambiar los valores de sus elementos.

```javascript
const user = { name: 'Juan' };
user.name = 'Manolo';
console.log(user.name); // Manolo
```

## Hoisting

En Javascript las variables son “hoisted” o “izadas”. Esto, como su nombre lo indica, quiere decir que una variable es izada(subida) hasta el tope de la función o hasta llegar al inicio del Window Object. Cuando no declaramos una variable, Javascript la crea en el Window Object. Llegando incluso a reasignar el valor de esta sin pedirnos permiso.

El compilador de JavaScript hace una primera pasada por todo el código para ver qué variables y funciones hemos definido y asignarles su ámbito. Hasta que la ejecución no llegue al punto de la asignación la variable tendrá el valor undefined. Esto no ocurre con let y const, que no son creadas hasta que la ejecución no llega al punto donde son declaradas. Así, si creamos una variable de bloque dentro de una condición que no se cumple, nunca se creará, y estaremos ahorrando ese tiempo y esa memoria.

```javascript
console.log(a); // undefined
var a = 1;
console.log(a); // 1

console.log(b);
const b = 1;
console.log(b);
// ReferenceError: Cannot access 'b' before initialization

console.log(c);
let
 c = 1;
console.log(c);
// ReferenceError: Cannot access 'c' before initialization
```

## === vs ==

Los operadores === y !== son los operadores de comparación estricta. Esto significa que si los operandos tienen tipos diferentes, no son iguales

```javascript
1 === "1" // false
1 !== "1"  // true
null === undefined // false
```

Los operadores == y != son los operadores de comparación relajada. Es decir, si los operandos tienen tipos diferentes, JavaScript trata de convertirlos para que fueran comparables.

```javascript
1 == "1" // true
1 != "1" // false
null == undefined // true
```
## Arrow functions

Funciones con dos caracteristicas principales: sintaxis reducida y this no vinculable.

Una arrow function no tiene su propio this, utiliza el valor del contexto de ejecución que la contiene.

Las arrow functions no tienen su propio objeto arguments. Por lo que, en este ejemplo, arguments es simplemente una referencia a los argumentos del contexto superior.

No pueden ser usadas como constructores, arrojarán un error cuando sea usada con new.

```javascript
const materials = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];

console.log(materials.map(material => material.length));
// expected output: Array [8, 6, 7, 9]
```

## undefined / not defined / null

* __undefined__ :
Una variables es declarada, pero no se le asigna ningun valor, o bien, el resultado de una funcion es asignado a una variable, pero la funcion no retorna nada. 

```javascript
let name;
console.log(name); //undefined

let add = (a,b) => {
  let c = a+b;
  //return c;
}
let sum = add(2,3);
console.log(sum); 
//Output: undefined
```

undefined typeof es undefined

* __not defined__:

Cuando no se declara una variable con var, let o const en ninguna parte del codigo.

```javascript
console.log(a);
var a = 5;
//Output:- undefined

console.log(b);
b = 5;
//Output:- "ReferenceError: b is not defined
```

* __null__:

null es un valor de asignación. 
Javascript nunca se setea un valor de null. Es utilizado por los programadores para indicar que un var no tiene ningún valor.

null typeof es un object

```javascript
null == undefined // true
null === undefined // false
```

## Array methods

* __concat():__ Unir dos o más arrays. Este método no cambia los arrays existentes, sino que devuelve un nuevo array

* __indexOf():__ Busca un elemento en el array y devuelve su posición. Busca desde el principio y a partir de la primera posición si no se especifica nada. Retorna -1 si no encuentra.

* __join():__  Junta los elementos de un array en un string con un separador (opcional).

* __pop():__ Borra el último elemento del array y devuelve su contenido.

* __push():__ Añade uno o más elementos al final de un array y devuelve la nueva longitud del array.

* __shift():__ El método shift elimina el elemento en el índice cero y desplaza los valores consecutivos hacia abajo, devolviendo el valor eliminado.

* __unshift():__ agrega uno o más elementos al inicio del array, y devuelve la nueva longitud del array.

* __split():__ retorna un nuevo array a partir de un string y un separador.

* __fill():__ rellena todos los elementos de un arreglo desde el índice start hasta el índice end, con el valor estático de value.

```javascript
// fill with 0 from position 2 until position 4
console.log(array1.fill(0, 2, 4));
// expected output: [1, 2, 0, 0]
```
* __filter():__ crea un nuevo array con todos los elementos que cumplan la condición establecida en el callback.

* __find():__ devuelve el valor del primer elemento del array que cumple la función de prueba proporcionada. En cualquier otro caso se devuelve undefined.

```javascript
const array1 = [5, 12, 8, 130, 44];
const found = array1.find(element => element > 10);
console.log(found);
// expected output: 12
```

* __forEach():__ ejecuta la función indicada una vez por cada elemento del array.

```javascript
const array1 = ['a', 'b', 'c'];
array1.forEach(element => console.log(element));
// expected output: "a"
// expected output: "b"
// expected output: "c"
```

* __includes():__ determina si una matriz incluye un determinado elemento, devuelve true o false según corresponda.

* __map():__ crea un nuevo array con los resultados de la llamada a la función indicada aplicados a cada uno de sus elementos.

* __splice():__ cambia el contenido de un array eliminando elementos existentes y/o agregando nuevos elementos.

```javascript
//Añade elementos a un array
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 0, "Lemon", "Kiwi");
//["Banana", "Orange", "Lemon", "Kiwi", "Apple", "Mango"]

//Borrar elementos de un Array
var fruits = ["Banana", "Orange", "Apple", "Mango", "Kiwi"];
fruits.splice(2, 2);
//["Banana", "Orange", "Kiwi"]
```

* __reduce():__ ejecuta una función reductora sobre cada elemento de un array, devolviendo como resultado un único valor.

```javascript
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```

* __reverse():__ El método reverse cruza los elementos del objeto matriz invocados en su lugar, mutando la matriz, y retornando una referencia a la misma.

* __toString():__ devuelve una cadena de caracteres representando el array especificado y sus elementos

* __sort():__ Por defecto, el método sort() ordena un array pero trata los elementos como strings.
Si queremos ordenar un array de números del 1 al 20 nos devolverá (1,10,11,12,13,14,15,16,17,18,19,2,20,3,……..)
Para que los ordene como números tenemos que incluir una función de comparación
```javascript
Ordenar de menor a mayor
arrayNumeros.sort(function(a,b){return a-b});
De mayor a menor
arrayNumeros.sort(function(a,b){return b-a})
```

## Que es modularizacion?
Separar el codigo en porciones de codigo, especializados e independientes (encapsulan logica en forma de caja negra). 

## Por que son necesarios lo modulos?
Se crean bloques de codigo que son mas faciles de mantener y entender. Encapsulan funcnionlidad (caja negra), reutilizacion de codigo.

## Que es programacion funcional?
Paradigma de programacion. separa las estructuras de datos y las funciones que operan sobre ellas. 
Los programas se construyen mediante la composición de funciones, de manera que una función realiza su trabajo llamando a otras funciones cada vez más simples.
Las funciones son un elemento de primer orden en el lenguaje, al igual que pueden ser los números o las cadenas de texto 
(Pueden guardarse en estructuras de datos, pasarse como argumentos y devolverse desde otras funciones).
Una función sólo retorna un valor y no produce cambios de estado.
El resultado de evaluar una expresión depende única y exclusivamente de sus parámetros de entrada 
(ante una misma entrada siempre obtenemos el mismo resultado)

## Que es Reactive Programming?
La programación reactiva es la programación con flujos de datos asíncronos.
Los buses de eventos (por ejemplo eventos de clicks) son un flujo de eventos asíncronos, en el que se pueden observar y reaccionar en consecuencia.
Un stream es una secuencia de eventos ordenados en el tiempo. Puede emitir tres cosas diferentes: un valor (de algún tipo), un error y una señal de "completado".
La gran mayoría de eventos que se producen son de naturaleza asíncrona, como un click de ratón, un envío de información desde backend o los caracteres que 
escribimos en un campo de búsqueda. Todas esas casuísticas tienen tres características en común:
    No sabemos con certeza cuándo se producirán (ni momento ni frecuencia).
    Queremos reaccionar ante ellos cuando se produzcan, bien sea operando con ellos o filtrándolos.
    Pueden ocurrir (clicks sucesivos) y cambiar (criterio de búsqueda) más de una vez a lo largo del tiempo.
Esos cambios y esa continuidad a lo largo del tiempo dan lugar a que sean “observables”. 

## Pure functions (Functional Programming)
Una funcion cuyo retorno es el mismo para los mismos inputs (argumentos), y no tiene efectos secundarios.
Una funcion pura solo tiene acceso a lo que se le pasa.

## Immutability
Es un principio fundamental de la programación funcional que se está extendiendo también en la programación orientada a objeto. Su planteamiento básico es muy sencillo: un dato u objeto, una vez creado, no puede ser cambiado, manteniendo su estado original en todo momento. Si por algún motivo se tuviera que cambiar el dato, entonces se obtendría una copia con los datos modificados, pero nunca se cambian los valores originales
Un valor inmutable es un valor que no se puede cambiar luego de ser definido, se puede modificar pero debe ser en un objeto diferente.
Lenguajes imperativos como JavaScript por defecto son mutables.

## Async patterns
https://lemoncode.net/lemoncode-blog/2018/1/29/javascript-asincrono
* __Callbacks:__

Un callback no es más que una función que se pasa como argumento de otra función, y que será invocada para completar algún tipo de acción. En nuestro contexto asíncrono, un callback representa el '¿Qué quieres hacer una vez que tu operación asíncrona termine?'. Por tanto, es el trozo de código que será ejecutado una vez que una operación asíncrona notifique que ha terminado. Esta ejecución se hará en algún momento futuro, gracias al mecanismo que implementa el event loop.
```javascript
setTimeout(function(){
  console.log("Hola Mundo con retraso!");
}, 1000)
```
Problema: Los callbacks también pueden lanzar a su vez llamadas asíncronas, asi que pueden anidarse tanto como se desee. Inconveniente, podemos acabar con un Callback Hell 

* __Promises:__

Una promesa es un objeto que representa el resultado de una operación asíncrona. Este resultado podría estar disponible ahora, en el futuro, o nunca.
Una promise un objeto al que le adjuntamos callbacks, en lugar de pasarlos directamente a la función asíncrona.
Una Promesa se encuentra en uno de los siguientes estados:
  * pendiente (pending): estado inicial, no cumplida o rechazada.
  * cumplida (fulfilled): significa que la operación se completó satisfactoriamente.
  * rechazada (rejected): significa que la operación falló.

Cuando cualquiera de estas dos opciones sucede, los métodos asociados, encolados por el método then de la promesa, son llamados.
Como los métodos Promise.prototype.then() y Promise.prototype.catch() retornan promesas, éstas pueden ser encadenadas.

- Promise.all() acepta un array de promesas y devuelve una nueva promesa cuya resolución se completará con éxito una vez que todas las promesas originales se hayan resuelto satisfactoriamente, o en caso de fallo, será rechazada en cuanto una de las promesas originales sea rechazada. Esta promesa compuesta, además, nos devolverá un array con los resultados de cada una de las promesas originales.
- Promise.race() es similar con la diferencia de un pequeño matiz. La promesa compuesta que devuelve .race() será resuelta tan pronto como se resuelva alguna de las promesas originales, ya sea con éxito o fallo. 


* __Async/Await:__

La declaración de función async define una función asíncrona, la cual devuelve una promise.

Cuando la función async devuelve un valor, Promise se resolverá con el valor devuelto. Si la función async genera una excepción o algún valor, Promise se rechazará con el valor generado.

Una función async puede contener una expresión await, la cual pausa la ejecución de la función asíncrona y espera la resolución de la Promise pasada y, a continuación, reanuda la ejecución de la función async y devuelve el valor resuelto.

```javascript
const checkServerWithSugar = async (url) => {
  try {
    const response = await fetch(url);
    return `Estado del servidor: ${response.status === 200 ? "OK" : "NOT OK"}`;
  } catch (e) {
    throw `Manejo intero del error. Error original: ${e}`;
  }
}

checkServerWithSugar(document.URL.toString())
  .then(result => console.log(result))
  .catch(e => console.log(`Error Capturado Fuera de la función async: ${e}`));
```

## Generators

Un generador es una función especial en JavaScript que puede pausar su ejecución y retomarla en un punto arbitrario. Para definirlos utilizamos dos nuevas palabras reservadas del lenguaje: function* y yield.
El uso más habitual de los generadores es crear Iteradores. Un Iterador es un objeto que devuelve un elemento de una colección cada vez que llamamos a su método .next.

En el momentoen el que llamamos al método .next del iterador, éste ejecuta la función del generador hasta llegar al primer yield que encuentra, que detiene la ejecución de la función y produce un resultado.

El resultado es siempre un objeto con dos propiedades, value y done.

En la siguiente llamada a .next la función continúa desde el yield y hasta el siguiente yield, y así hasta encontrar un return que devolverá true como valor de done.
```javascript
function* counterGenerator() {
  let i = 0
  while (true) {
    yield i
    i++
  }
}

var counter = counterGenerator()

counter.next() // { value: 0, done: false }
counter.next() // { value: 1, done: false }
counter.next() // { value: 2, done: false }
... // hasta el infinito y más allá!
```

En el ejemplo anterior hemos usado todo el tiempo un bucle while (true) sin bloquear o saturar la cpu y sin ninguna alerta por parte de node. Esto es así porque yield pausa la ejecución de la función, y por lo tanto, pausa el bucle infinito, cada vez que produce un valor.

Esto es lo que se llama evaluación perezosa y es un concepto importante en lenguajes funcionales como Haskell. Básicamente nos permite tener listas o estructuras de datos “infinitas” y operar sobre ellas, por ejemplo podemos tener un operador take(n) que toma los N primeros elementos de una lista infinita

## Destructuring

Expresión de JavaScript que hace posible la extracción de datos de arreglos u objetos usando una sintaxis que equivale a la construcción de arreglos y objetos literales.

```javascript
let a, b, rest;
[a, b] = [10, 20];
console.log(a); // 10
console.log(b); // 20

[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

({ a, b } = { a: 10, b: 20 });
console.log(a); // 10
console.log(b); // 20


// Propuesta de etapa 4 (terminada)
({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
console.log(a); // 10
console.log(b); // 20
console.log(rest); // {c: 30, d: 40}
```

Asignación básica de variables

```javascript
const foo = ['uno', 'dos', 'tres'];

const [rojo, amarillo, verde] = foo;
console.log(rojo); // "uno"
console.log(amarillo); // "dos"
console.log(verde); // "tres"
```

Asignación separada de la declaración
A una variable se le puede asignar su valor mediante la desestructuración separada de la declaración de la variable.

```javascript
let a, b;

[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

## Spread operator

Permite a un elemento iterable tal como un arreglo o cadena ser expandido en lugares donde cero o más argumentos (para llamadas de  función) ó elementos (para Array literales) son esperados, o a un objeto ser expandido en lugares donde cero o más pares de valores clave (para literales Tipo Objeto) son esperados.

* __Usar los elementos de un arreglo como argumentos de una función__

```javascript
function myFunction(x, y, z) { }
var args = [0, 1, 2];
myFunction(...args);
```

* __Expandir Array literales__

```javascript
var parts = ['shoulders', 'knees']; 
var lyrics = ['head', ...parts, 'and', 'toes']; 
// ["head", "shoulders", "knees", "and", "toes"]
```

* __Copiar un arreglo__

```javascript
var arr = [1, 2, 3];
var arr2 = [...arr]; // like arr.slice()
arr2.push(4); 

// arr2 becomes [1, 2, 3, 4]
// arr remains unaffected
```

* __Concatenar__

Alternativa a concat()

```javascript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1 = [...arr1, ...arr2];
```

En objects, copia sus propiedades enumerables desde un objeto provisto dentro de un nuevo objeto.

Shallow-cloning (excluyendo prototype) o la combinación de objetos es ahora posible usando una sintáxis más corta que Object.assign().

```javascript
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };

var clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }

var mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 }
```

## Map vs Object

Map es un tipo de coleccion de datos en el cual los datos se almacenan en forma de par: una clave unica, y un valor asociado a esa key.
Debido al que la key sea unica, no hay pares duplicados.

```javascript
var miMapa = new Map();

var claveObj = {},
    claveFunc = function () {},
    claveCadena = "una cadena";

// asignando valores
miMapa.set(claveCadena, "valor asociado con 'una cadena'");
miMapa.set(claveObj, "valor asociado con claveObj");
miMapa.set(claveFunc, "valor asociado with claveFunc");

miMapa.size; // 3

// obteniendo los valores
miMapa.get(claveCadena);    // "valor asociado con 'una cadena'"
miMapa.get(claveObj);       // "valor asociado con claveObj"
miMapa.get(claveFunc);      // "valor asociado con claveFunc"

miMapa.get("una cadena");   // ""valor asociado con 'una cadena'" porque claveCadena === 'una cadena'
```

https://medium.com/front-end-weekly/es6-map-vs-object-what-and-when-b80621932373

## ES6 features

* Const
* Block scope variables (let)
* Arrow functions
* Simple and intuitive default values for function parameters.
* Rest param
* Spread operator
* String Interpolation

## Como vaciar array?

```javascript
var list = [1, 2, 3, 4];
function empty() {
    //empty your array
    list.length = 0;
}
empty();
```

Si se hiciera list = [], se asignaria una nueva referencia en memoria a esa variable, pero todo el contenido anterior aun permaneceria en memoria.

array.length = 0 limpia todo del arreglo, incluyendo otras referencias al arreglo - Ej: (a = [1,2,3]; a2 = a;) - 

## Prototype

## Tipos de datos primitivos

Un primitivo (valor primitivo, tipo de datos primitivo) es un dato que no es un objeto y no tiene métodos. En JavaScript hay 6 tipos de datos primitivos: string , number , boolean , null , undefined , symbol (nuevo en ECMAScript 2015).

La mayoría del tiempo, un valor primitivo es representado directamente en el nivel más bajo de la interpretación del lenguaje.

Todos los primitivos son inmutables (no pueden ser cambiados).

## Deep copy y Shalow copy

