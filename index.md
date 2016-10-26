&nbsp;

&nbsp;

&nbsp;

# Stop Writing for-loops*
![](https://cdn-images-1.medium.com/max/1280/1*eXtcDA7zohLE8GGm0W1UYQ.png)

#### _Functional Programming in Javascript_

William Carroll

&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;


## Synopsis
* Practical functional programming (abridged)
* Python to contrast with Javascript
* Jargon
* The problem with the for-loop
* Overthrowing the for-loop
	* `filter(..)`
	* `map(..)`
	* `reduce(..)`
* Function pilates
	* high-order functions
	* `curry(..)`
	* `compose(..)` & `pipe(..)`
* Mutations
* Monads
* Why functional programming?

REPL: [http://jsbin.com/nelipapahi/edit?html,js,console](http://jsbin.com/nelipapahi/edit?html,js,console)


&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;

## ES6
* Examples will be in ES6
* More concise
* More beautiful
* Write more logic with less code
* Striking similarities to Python


### Arrow Functions

#### Javascript

```javascript
function fn(a) {
  return a + 1
}

const fn = a => a + 1
```

#### Python

```
def fn(a):
	return a + 1

fn = lambda a: a + 1
```


### Spread Operator

#### Javascript

```javascript
function fn(a, b, c) {
  return a + b + c
}

fn(1, 2, 3) == fn(...[1, 2, 3])
```

#### Python

```
def fn(a, b, c):
	return a + b + c

fn(1, 2, 3) == fn(*[1, 2, 3])
```

### Rest Parameter

#### Javascript

```javascript
function fn(...args) { ... }
```

#### Python	

```
def fn(*args):
	...
```


### Destructuring

#### Javascript

```javascript
const [a, b, c] = [1, 2, 3]
// a => 1
// b => 2
// c => 3

const { name, age } = { name: 'William', age: 24 }
// name => 'William'
// age => 24
```

#### Python

```
>>> a, b, c = (1, 2, 3)
>>> a
1
>>> b
2
>>> c
3
```


&nbsp;

&nbsp;

&nbsp;

---

## What can FP in JS look like?

```javascript
const url = 'https://api.icndb.com/jokes/random';

compose(
  clog,
  path(['value', 'joke']),
  parseJSON,
  httpGet
)(url);

// => "There are no steroids in baseball. Just players Chuck Norris has breathed on."
```

![](https://cdn-images-1.medium.com/max/2000/1*XPIhV5x2T0HmT8lIcmSOOw.png =500x)

---
&nbsp;

&nbsp;

&nbsp;


## Jargon

* Functor
* Apply
* Applicative
* Monad
* Chain
* ChainRrec
* Bifunctor
* Extend
* Profunctor
* Traversable
* Foldable
* Semigroup
* Monoid
* Setoid

&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;


## Unpack the Jargon

* Fantasy-land spec
* [https://github.com/fantasyland/fantasy-land](https://github.com/fantasyland/fantasy-land)


&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;



## Problem with the for-loop

* Procedural
* Teaching an array how to expose its values

### ex. 1
```javascript
var sum = 0;

for (var i = 0; i < array.length; i++) {
  sum += array[i];
}
```

```javascript
array.reduce(add, 0)
```

### ex. 2
```javascript
var newArray = [];

for (var i = 0; i < array.length; i++) {
  var value = array[i];
  
  if (value % 2 == 0) {
    newArray.push(value);
  }
}
```

```javascript
array.filter(isEven)
```

### ex. 3
```javascript
for (var i = 0; i < array.length; i++) {
  array[i] = array[i] * 2;
}
```

```javascript
array.map(mult(2))
```



&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;




## Overthrowing the for-loop

### filter(..)

* Inputs: array and predicate
* Outputs: new array

```javascript
filter(predicate, array)
```

```javascript
const predicate = (element, index, array) => { ... }
```

```javascript
[1, null, 3, 4, null].filter((element, index, array) => element !== null)
// => [1, 3, 4]
```


### map(..)

* Inputs: array and iterator
* Outputs: new array (same no. of values as original)

```javascript
map(iterator, array)
```

```javascript
const iterator = (element, index, array) => { ... }
```

```javascript
[1, 2, 3, 4].map((element, index, array) => element * 2)
// => [2, 4, 6, 8]
```

### NOTE: array (et al) comprehensions

```python
>>> xs = [1, None, 3, 4]
>>> [x * 2 for x in xs if x != None]
[2, 6, 8]
```

```python
>>> d = {'a': 1, 'b': 2, 'c': 3, 'd': None}
>>> dict((k, v * 2) for k, v in d.iteritems() if v != None)
{'a': 2, 'b': 4, 'c': 6}
```

* python removed `reduce(..)` in Python3


### reduce(..)

* Inputs: array and reducer
* Outputs: new any

```javascript
reduce(reducer, initialValue, array)
```


```javascript
const reducer = (accumulator, element, index, array) => { ... }
```


##### Array -> Object

```javascript
const rToO = array => array.reduce((object, el, i) => 
  Object.assign(object, {[i]: el}), {})
```

```javascript
rToO(['red', 'green', 'blue'])
// => {0: 'red', 1: 'green', 2: 'blue'}
```

##### Object -> Array

```javascript
const oToR = object => Object.keys(object).reduce(
  (array, k) => array.concat(object[k]), []);
```

```javascript
oToR({a: 2, b: 4, c: 6, d: 8});
// => [2, 4, 6, 8]
```


&nbsp;

&nbsp;

&nbsp;

---

&nbsp;

&nbsp;

&nbsp;

## Function Pilates

### Decorators

#### memoize(..) and lru_cache(..)

```javascript
fib(1000)

// versus

fib(10000)
```

#### throttle(..) and debounce(..)

```javascript
document.addEventListener('scroll', throttle(scrollHandler))
```




### Currying

* function arity
* allows for great composability

```javascript
const add = a => b => c => a + b + c
```

```javascript
add(1)(2)(3)
// => 6

add(1, 2, 3)
// => 6
```


#### Implementation

```javascript
const curry = fn => {
  const parmsNeeded = fn.length
  let parmsReceived = []

  return function ref(...args) {
    if (parmsReceived.length + args.length >= parmsNeeded) {
      return fn(...parmsReceived, ...args)
    }
    parmsReceived = [...parmsReceived, ...args]
    
    return ref
  }
}
```

### compose(..)

> “There are only two hard parts of Computer Science: cache invalidation and naming things.” — Martin Fowler


```javascript
compose(
  x => x + 5
  x => x * 2,
  x => -x,
)(10);
// => -15
```

### Point-free?

```javascript
compose(
  add(5)   // <- curried
  mult(2), // <- curried
  negate,
)(10);
// => -15
```

### Bash

```bash
$ find . -type f -name '*.js' | xargs cat | wc -l
```


### With higher-order functions

```javascript
const fn = compose(
  reduce(add, 0),
  map(divide(_, 2)),
  filter(isEven)
);

fn([11, 40, 21, 2, 14, 16]);
// => 36
```

### Debugging

```javascript
const clog = (...args) => console.log(...args)
```

```javascript
const fn = compose(
  clog,              // 36
  reduce(add, 0),
  clog,              // [20, 1, 7, 8]
  map(divide(_, 2)),
  clog,              // [40, 2, 14, 16]
  filter(isEven)
);
```


### Implementation

```javascript
const pipe = (...fns) => (...args) =>
  fns.slice(1).reduce(
    (result, fn) => fn(result), fns[0](...args))
```

### Why compose?
* It's declarative
* Stop naming things
* Using commonly understood point-free functions reduces developers' congitive load



&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;



## Monads (lite)

* Monads are burritos
* Async monad (aka Promise)
* Maybe monad (does null checking for free)
* common interface
* How do you "lift" the value out?
* 2 options...

#### Identity Monad

```javascript
function IMonad(value) {
  this.value = value
}

IMonad.prototype.pprint = function() {
  console.log(this.value)
}

IMonad.prototype.map = function(fn) {
  return new IMonad(fn(this.value))
}
```


#### Maybe Monad
```javascript
function MaybeMonad(value) {
  this.value = value
}

MaybeMonad.prototype.pprint = function() {
  console.log(this.value)
}

MaybeMonad.prototype.map = function(fn) {
  if (this.value) {
    return new MaybeMonad(fn(this.value))
  }
  return this
}
```

#### Error Monad
```javascript
function ErrorMonad(value) {
  this.value = value
}

ErrorMonad.prototype.pprint = function() {
  console.log(this.value)
}

ErrorMonad.prototype.map = function(fn) {
  try {
    return new ErrorMonad(fn(this.value))
  }
  catch (err) {
    console.error(err)
    return new ErrorMonad(this.value)
  }
}
```

#### Promises
```javascript
// Async simulation
const doSomething = () => new Promise((resolve, reject) => {
  const payload = {
    data: 'I am the payload'
  }
  
  setTimeout(() => resolve(payload), 3000)
})
```

```javascript
// Option 1

doSomething()
.then(value => console.log(value))
// => {data: 'I am the payload'}


// Option 2

async function fn() {
  const value = await doSomething()
  
  console.log(value)
  // => {data: 'I am the payload'}
}
```

## Mutations

* Why are functional programmers obsessed with mutations?
* Programs can be reduced to three basic things:
	* inputs
	* outputs
	* side effects
* If we place a constraint on ourselves we may be more productive as a result.
* FP concepts as a framework
* Redux
* Time-travelling
* Elm architecture


&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;


## Why the Asterisk?
* for...of loop
* Arrays
* Data types that define a Symbol.iterator
* Don't have to write the logic to "lift" the values
* The data types themselves define how to "lift" their own values

## Making an Object a functor in JS

```javascript
// Non-mutative (good)
const mappify = objLiteral => {
  const newObjLiteral = Object.assign({}, objLiteral)

  const map = iterator =>
    Object.keys(objLiteral).reduce((result, k) => {
      result[k] = iterator(objLiteral[k], k, objLiteral)
      return result
    }, {})

  Object.defineProperty(newObjLiteral, 'map', {
    value: map,
    configurable: true,
    enumerable: false,
    writeable: true
  })

  return newObjLiteral
}

```

```javascript
// Non-mutative (good)
const iteraterify = objLiteral => {
  const proto = Object.assign({}, Object.prototype)
  
  function* values() {
    for (const k of Object.keys(objLiteral)) {
      yield [k, objLiteral[k]]
    }
  };

  proto[Symbol.iterator] = values

  return Object.create(proto)
}
```



### Arrays

```javascript
for (const x of [1, 2, 3]) { ... }
```

### Generators

```javascript
function* createSequenceGenerator() {
  yield 1
  yield 2
  yield 3
}

const seqGen = createSequenceGenerator()

for (const x of seqGen) { ... }
```

### Objects?

```
Natively: Nope.
```

```javascript
const me = {
  name: 'William',
  age: 24,
  city: 'New York'
}

for (const [k, v] of iteraterify(me)) {
  console.log(k, v)
}
```


&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;

## Challenge

* Create a function, `add(..)` that passes these tests.


```javascript
/* -- binary -- */
add(1, 2) == 3
// => true

add(1)(2) == 3
// => true


/* -- ternary -- */
add(1, 2, 3) == 6
// => true

add(1)(2)(3) == 6
// => true

add(1, 2)(3) == 6
// => true

add(1)(2, 3) == 6
// => true


/* -- 4-ary -- */
add(1, 2, 3, 4) == 10
// => true

add(1, 2, 3)(4) == 10
// => true

add(1, 2)(3, 4) == 10
// => true

add(1)(2, 3, 4) == 10
// => true
```
