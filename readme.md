# Jerga de la Programación Funcional

La programación funcional ofrece muchas ventajas, de ahí el auge de su popularidad. Sin embargo, cada paradigma de programación trae consigo una jerga única, y la programación funcional no es la excepción. Al proveer un glosario, creemos que podemos ayudar a aprender programación funcional de una manera más fácil.

Los ejemplos presentados están escritos en JavaScript (ES2015). [Por qué JavaScript?](https://github.com/hemanth/functional-programming-jargon/wiki/Why-JavaScript%3F)

_Este es un trabajo en progreso; siéntase libre en enviar PR._

__Traducciones__

 * [Inglés](https://github.com/hemanth/functional-programming-jargon)
 * [Portugués](https://github.com/alexmoreno/jargoes-programacao-funcional)

__Tabla de Contenido__
<!-- RM(noparent,notop) -->

* [Aridad](#aridad)
* [Funciones de Orden Superior (FOS)](#funciones-de-orden-superior-fos)
* [Aplicación Parcial](#aplicaci%C3%B3n-parcial)
* [Currying](#currying)
* [Auto Currying](#auto-currying)
* [Composición de Funciones](#composici%C3%B3n-de-funciones)
* [Pureza](#pureza)
* [Efectos secundarios](#efectos-secundarios)
* [Idempotente](#idempotente)
* [Estilo Point-Free](#estilo-point-free)
* [Predicado](#predicado)
* [Funtor](#funtor)
* [Lambda](#lambda)
* [Cálculo lambda](#calculo-lambda)


<!-- /RM -->

## Aridad

El número de argumentos que acepta una función. Derivada de unaria, binaria, ternaria, etc… Esta palabra tiene la distinción de estar compuesta de dos sufijos, “-aria” y “-idad”. Por ejemplo, la suma, acepta dos argumentos, y por lo tanto se define como una función binaria o una función de aridad dos. De igual manera, una función que acepta un número variable de argumentos es llamada de aridad variable.

```js
const suma = (a, b) => a + b

const aridad = suma.length
console.log(aridad) // 2

// La aridad de la suma es 2
```


## Funciones de Orden Superior (FOS)

Función que acepta una función como argumento y/o retorna otra función.

```js
const filtro = (predicado, xs) => {
  const resultado = []
  for (let idx = 0; idx < xs.length; idx++) {
    if (predicado(xs[idx])) {
      resultado.push(xs[idx])
    }
  }
  return resultado
}
```

```js
const es = (tipo) => (x) => Object(x) instanceof tipo
```

```js
filtro(es(Number), [0, '1', 2, null]) // [0, 2]
```

## Aplicación Parcial

Aplicar parcialmente una función significa crear una nueva función al llamar la función original fijando algunos de sus argumentos.


```js
// Helper para crear funciones aplicadas parcialmente
// Acepta una función y algunos argumentos
const parcial = (f, ...args) =>
  // retorna una función que acepta el resto de los argumentos
  (...masArgs) =>
    // y llama la función original con todos ellos
    f(...args, ...masArgs)

// Algo para aplicar
const suma3 = (a, b, c) => a + b + c

// Aplicar parcialmente `2` y `3` a `sumar3` retorna una función que acepta un argumento
const cincoMas = parcial(suma3, 2, 3) // (c) => 2 + 3 + c

cincoMas(4) // 9
```

También podemos utilizar `Function.prototype.bind` para aplicar parcialmente una función en JS:

```js
const suma1mas = suma3.bind(null, 2, 3) // (c) => 2 + 3 + c
```

La aplicación parcial permite crear funciones simples a partir de funciones complejas a medida que uno va obteniendo datos. La funciones [“currying”](#currying) son parcialmente aplicada de manera automática.

## Currying

Es el proceso de convertir una función que acepta múltiples argumentos a una función que acepta dichos argumentos uno a la vez.

Cada vez que la función es invocada solo acepta un solo argumento y retorna otra función que a su vez acepta un solo argumento hasta que todos los argumentos hayan sido pasados.

```js
const suma = (a, b) => a + b

const sumaCurried = (a) => (b) => a + b

sumaCurried(40)(2) // 42.

const suma2 = sumaCurried(2) // (b) => 2 + b

suma2(10) // 12

```

## Auto Currying

Transformar una función que acepta múltiples argumentos en una que al recibir menos argumentos que el número total que acepta, retorna una función que acepta el resto de los argumentos. Cuando la función recibe el número correcto de argumentos, entonces es evaluada.

Underscore, lodash, y ramda tienen una función `curry` que funciona de esta manera.

```js
const suma = (x, y) => x + y

const sumaCurried = _.curry(suma)
sumaCurried(1, 2) // 3
sumaCurried(1) // (y) => 1 + y
sumaCurried(1)(2) // 3
```

__Lecturas Adicionales (en inglés)__

 * [Favoring Curry](http://fr.umio.us/favoring-curry/)
 * [Hey Underscore, You're Doing It Wrong!](https://www.youtube.com/watch?v=m3svKOdZijA)

## Composición de Funciones

El acto de unir dos funciones para formar una tercera función donde la salida de una función es la entrada de la otra.

```js
const componer = (f, g) => (a) => f(g(a)) // Definición
const redondearYtoString = componer((val) => val.toString(), Math.floor) // Uso
redondearYtoString(121.212121) // '121'
```


## Pureza

Una función es pura si el valor de retorno está solamente determinado por sus valores de entrada, y no produce efectos secundarios.

```js
const saludar = (nombre) => 'Hola ' + nombre

saludar('Amelie') // 'Hola Amelie'

```

Contrario a:

```js

let saludo

const saludar = () => {
  saludo = 'Hola, ' + window.name
}

saludar() // "Hola Amelie"

```

## Efectos secundarios

Se dice que una función o expresión tiene efectos secundarios si aparte de retornar un valor, esta interactúa (lee o escribe) con algún estado mutable externo.

```js
const diferenteCadaVez = new Date()
```

```js
console.log('E/S (entrada y salida) es un efecto secundario!')
```

## Idempotente

Una función es idempotente cuando al ser invocada con su resultado no produce un resultado diferente.

```
f(f(x)) ≍ f(x)
```

```js
Math.abs(Math.abs(10))
```

```js
sort(sort(sort([2, 1])))
```


## Estilo Point-Free

Escribir funciones donde su definición no  identifica explícitamente los argumentos usados. Este estilo usualmente requiere [currying](#currying) u otras [Funciones de Orden Superior (FOS)](#funciones-de-orden-superior-fos). También conocido como Programación Tácita.

```js
// Dado
const map = (fn) => (lista) => lista.map(fn)
const suma = (a) => (b) => a + b

// Entonces

// No es points-free - `números` es un argumento explicito
const incrementarTodos = (números) => map(suma(1))(números)

// Es Points-free - la lista de números es un argumento implícito
const incrementarTodos2 = map(suma(1))
```

`incrementarTodos` identifica y usa el parámetro `números`, por lo tanto no es point-free.  `incrementarTodos2` esta escrita mediante la simple combinación de funciones y valores, sin hacer mención de sus argumentos. Por lo tanto __es__ points-free.

Las definiciones de funciones escritas utilizando el estilo Point-free se ven justo como las asignaciones normales sin la utilización de `function` o `=>`.


## Predicado
Un predicado es una función que retorna verdadero o falso para un valor dado. Un uso comun de un predicado es en el callback del metodo filter de un arreglo.

```js
const predicado = (a) => a > 2

;[1, 2, 3, 4].filter(predicado) // [3, 4]
```

<!--
## Contracts

TODO

## Guarded Functions

TODO

## Categories

Objects with associated functions that adhere to certain rules. E.g. [Monoid](#monoid)

## Value

Anything that can be assigned to a variable.

```js
5
Object.freeze({name: 'John', age: 30}) // The `freeze` function enforces immutability.
;(a) => a
;[1]
undefined
```

## Constant

A variable that cannot be reassigned once defined.

```js
const five = 5
const john = {name: 'John', age: 30}
```

Constants are [referentially transparent](#referential-transparency). That is, they can be replaced with the values that they represent without affecting the result.

With the above two constants the following expression will always return `true`.

```js
john.age + five === ({name: 'John', age: 30}).age + (5)
```
-->
## Funtor

Un objeto que implementa una función `map` la cual, mientras se ejecuta sobre cada valor del objeto para producir un nuevo objeto, se adhiere a dos reglas:

```js
// preserva la identidad
object.map(x => x) === object
```

and

```js
// composición
object.map(x => f(g(x))) === object.map(g).map(f)
```

(Sea `f`, `g` funciones arbritarias)

Un funtor muy común en JavaScript es `Array` ya que cumple las dos reglas de funtor: 

```js
[1, 2, 3].map(x => x) // = [1, 2, 3]
```

y

```js
const f = x => x + 1
const g = x => x * 2

;[1, 2, 3].map(x => f(g(x))) // = [3, 5, 7]
;[1, 2, 3].map(g).map(f)     // = [3, 5, 7]
```
<!--
## Pointed Functor
An object with an `of` function that puts _any_ single value into it.

ES2015 adds `Array.of` making arrays a pointed functor.

```js
Array.of(1) // [1]
```

## Lift

Lifting is when you take a value and put it into an object like a [functor](#pointed-functor). If you lift a function into an [Applicative Functor](#applicative-functor) then you can make it work on values that are also in that functor.

Some implementations have a function called `lift`, or `liftA2` to make it easier to run functions on functors.

```js
const liftA2 = (f) => (a, b) => a.map(f).ap(b)

const mult = a => b => a * b

const liftedMult = liftA2(mult) // this function now works on functors like array

liftedMult([1, 2], [3]) // [3, 6]
liftA2((a, b) => a + b)([1, 2], [3, 4]) // [4, 5, 5, 6]
```

Lifting a one-argument function and applying it does the same thing as `map`.

```js
const increment = (x) => x + 1

lift(increment)([2]) // [3]
;[2].map(increment) // [3]
```


## Referential Transparency

An expression that can be replaced with its value without changing the
behavior of the program is said to be referentially transparent.

Say we have function greet:

```js
const greet = () => 'Hello World!'
```

Any invocation of `greet()` can be replaced with `Hello World!` hence greet is
referentially transparent.

##  Equational Reasoning

When an application is composed of expressions and devoid of side effects, truths about the system can be derived from the parts.
-->
## Lambda

Una función anónima que puede tratarse como un valor.

```js
;(function (a) {
  return a + 1
})

;(a) => a + 1
```

Lambdas a menudo se pasan como argumentos de funciones de Orden Superior.

```js
[1, 2].map((a) => a + 1) // [2, 3]
```

Se puede asignar un lambda a una variables.

```js
const add1 = (a) => a + 1
```

## Cálculo lambda

Una rama de las matemáticas que utiliza funciones para crear un [modelo universal de computación](https://es.wikipedia.org/wiki/Modelo_de_computaci%C3%B3n).
<!--
## Lazy evaluation

Lazy evaluation is a call-by-need evaluation mechanism that delays the evaluation of an expression until its value is needed. In functional languages, this allows for structures like infinite lists, which would not normally be available in an imperative language where the sequencing of commands is significant.

```js
const rand = function*() {
  while (1 < 2) {
    yield Math.random()
  }
}
```

```js
const randIter = rand()
randIter.next() // Each execution gives a random value, expression is evaluated on need.
```

## Monoid

An object with a function that "combines" that object with another of the same type.

One simple monoid is the addition of numbers:

```js
1 + 1 // 2
```
In this case number is the object and `+` is the function.

An "identity" value must also exist that when combined with a value doesn't change it.

The identity value for addition is `0`.
```js
1 + 0 // 1
```

It's also required that the grouping of operations will not affect the result (associativity):

```js
1 + (2 + 3) === (1 + 2) + 3 // true
```

Array concatenation also forms a monoid:

```js
;[1, 2].concat([3, 4]) // [1, 2, 3, 4]
```

The identity value is empty array `[]`

```js
;[1, 2].concat([]) // [1, 2]
```

If identity and compose functions are provided, functions themselves form a monoid:

```js
const identity = (a) => a
const compose = (f, g) => (x) => f(g(x))
```
`foo` is any function that takes one argument.
```
compose(foo, identity) ≍ compose(identity, foo) ≍ foo
```

## Monad

A monad is an object with [`of`](#pointed-functor) and `chain` functions. `chain` is like [`map`](#functor) except it un-nests the resulting nested object.

```js
// Implementation
Array.prototype.chain = function (f) {
  return this.reduce((acc, it) => acc.concat(f(it)), [])
}

// Usage
;['cat,dog', 'fish,bird'].chain((a) => a.split(',')) // ['cat', 'dog', 'fish', 'bird']

// Contrast to map
;['cat,dog', 'fish,bird'].map((a) => a.split(',')) // [['cat', 'dog'], ['fish', 'bird']]
```

`of` is also known as `return` in other functional languages.
`chain` is also known as `flatmap` and `bind` in other languages.

## Comonad

An object that has `extract` and `extend` functions.

```js
const CoIdentity = (v) => ({
  val: v,
  extract () {
    return this.val
  },
  extend (f) {
    return CoIdentity(f(this))
  }
})
```

Extract takes a value out of a functor.

```js
CoIdentity(1).extract() // 1
```

Extend runs a function on the comonad. The function should return the same type as the comonad.

```js
CoIdentity(1).extend((co) => co.extract() + 1) // CoIdentity(2)
```

## Applicative Functor

An applicative functor is an object with an `ap` function. `ap` applies a function in the object to a value in another object of the same type.

```js
// Implementation
Array.prototype.ap = function (xs) {
  return this.reduce((acc, f) => acc.concat(xs.map(f)), [])
}

// Example usage
;[(a) => a + 1].ap([1]) // [2]
```

This is useful if you have two objects and you want to apply a binary function to their contents.

```js
// Arrays that you want to combine
const arg1 = [1, 3]
const arg2 = [4, 5]

// combining function - must be curried for this to work
const add = (x) => (y) => x + y

const partiallyAppliedAdds = [add].ap(arg1) // [(y) => 1 + y, (y) => 3 + y]
```

This gives you an array of functions that you can call `ap` on to get the result:

```js
partiallyAppliedAdds.ap(arg2) // [5, 6, 7, 8]
```

## Morphism

A transformation function.

### Endomorphism

A function where the input type is the same as the output.

```js
// uppercase :: String -> String
const uppercase = (str) => str.toUpperCase()

// decrement :: Number -> Number
const decrement = (x) => x - 1
```

### Isomorphism

A pair of transformations between 2 types of objects that is structural in nature and no data is lost.

For example, 2D coordinates could be stored as an array `[2,3]` or object `{x: 2, y: 3}`.

```js
// Providing functions to convert in both directions makes them isomorphic.
const pairToCoords = (pair) => ({x: pair[0], y: pair[1]})

const coordsToPair = (coords) => [coords.x, coords.y]

coordsToPair(pairToCoords([1, 2])) // [1, 2]

pairToCoords(coordsToPair({x: 1, y: 2})) // {x: 1, y: 2}
```



## Setoid

An object that has an `equals` function which can be used to compare other objects of the same type.

Make array a setoid:

```js
Array.prototype.equals = (arr) => {
  const len = this.length
  if (len !== arr.length) {
    return false
  }
  for (let i = 0; i < len; i++) {
    if (this[i] !== arr[i]) {
      return false
    }
  }
  return true
}

;[1, 2].equals([1, 2]) // true
;[1, 2].equals([0]) // false
```

## Semigroup

An object that has a `concat` function that combines it with another object of the same type.

```js
;[1].concat([2]) // [1, 2]
```

## Foldable

An object that has a `reduce` function that can transform that object into some other type.

```js
const sum = (list) => list.reduce((acc, val) => acc + val, 0)
sum([1, 2, 3]) // 6
```

## Traversable

TODO

## Type Signatures

Often functions in JavaScript will include comments that indicate the types of their arguments and return values.

There's quite a bit of variance across the community but they often follow the following patterns:

```js
// functionName :: firstArgType -> secondArgType -> returnType

// add :: Number -> Number -> Number
const add = (x) => (y) => x + y

// increment :: Number -> Number
const increment = (x) => x + 1
```

If a function accepts another function as an argument it is wrapped in parentheses.

```js
// call :: (a -> b) -> a -> b
const call = (f) => (x) => f(x)
```

The letters `a`, `b`, `c`, `d` are used to signify that the argument can be of any type. The following version of `map` takes a function that transforms a value of some type `a` into another type `b`, an array of values of type `a`, and returns an array of values of type `b`.

```js
// map :: (a -> b) -> [a] -> [b]
const map = (f) => (list) => list.map(f)
```

__Further reading__
* [Ramda's type signatures](https://github.com/ramda/ramda/wiki/Type-Signatures)
* [Mostly Adequate Guide](https://drboolean.gitbooks.io/mostly-adequate-guide/content/ch7.html#whats-your-type)
* [What is Hindley-Milner?](http://stackoverflow.com/a/399392/22425) on Stack Overflow

## Union type
A union type is the combination of two types together into another one.

JS doesn't have static types but let's say we invent a type `NumOrString` which is a sum of `String` and `Number`.

The `+` operator in JS works on strings and numbers so we can use this new type to describe its inputs and outputs:

```js
// add :: (NumOrString, NumOrString) -> NumOrString
const add = (a, b) => a + b

add(1, 2) // Returns number 3
add('Foo', 2) // Returns string "Foo2"
add('Foo', 'Bar') // Returns string "FooBar"
```

Union types are also known as algebraic types, tagged unions, or sum types.

There's a [couple](https://github.com/paldepind/union-type) [libraries](https://github.com/puffnfresh/daggy) in JS which help with defining and using union types.

## Product type

A **product** type combines types together in a way you're probably more familiar with:

```js
// point :: (Number, Number) -> {x: Number, y: Number}
const point = (x, y) => ({x: x, y: y})
```
It's called a product because the total possible values of the data structure is the product of the different values.

See also [Set theory](https://en.wikipedia.org/wiki/Set_theory).

## Option
Option is a [union type](#union-type) with two cases often called `Some` and `None`.

Option is useful for composing functions that might not return a value.

```js
// Naive definition

const Some = (v) => ({
  val: v,
  map (f) {
    return Some(f(this.val))
  },
  chain (f) {
    return f(this.val)
  }
})

const None = () => ({
  map (f) {
    return this
  },
  chain (f) {
    return this
  }
})

// maybeProp :: (String, {a}) -> Option a
const maybeProp = (key, obj) => typeof obj[key] === 'undefined' ? None() : Some(obj[key])
```
Use `chain` to sequence functions that return `Option`s
```js

// getItem :: Cart -> Option CartItem
const getItem = (cart) => maybeProp('item', cart)

// getPrice :: Item -> Option Number
const getPrice = (item) => maybeProp('price', item)

// getNestedPrice :: cart -> Option a
const getNestedPrice = (cart) => getItem(obj).chain(getPrice)

getNestedPrice({}) // None()
getNestedPrice({item: {foo: 1}}) // None()
getNestedPrice({item: {price: 9.99}}) // Some(9.99)
```

`Option` is also known as `Maybe`. `Some` is sometimes called `Just`. `None` is sometimes called `Nothing`.

## Functional Programming Libraries in JavaScript

* [Ramda](https://github.com/ramda/ramda)
* [Folktale](http://folktalejs.org)
* [lodash](https://github.com/lodash/lodash)
* [Underscore.js](https://github.com/jashkenas/underscore)
* [Lazy.js](https://github.com/dtao/lazy.js)
* [maryamyriameliamurphies.js](https://github.com/sjsyrek/maryamyriameliamurphies.js)
* [Haskell in ES6](https://github.com/casualjavascript/haskell-in-es6)
-->

---

__Nota:__ Este repositorio ha sido un éxito gracias a las [contribuciones de los colaboradores](https://github.com/rmariuzzo/jerga-programacion-funcional/graphs/contributors)!
