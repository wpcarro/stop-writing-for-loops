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


## What is this talk?

* Practical functional programming
* Introduce jargon
* ...and then avoid jargon
* Python to contrast with Javascript
* Technical in nature

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

```
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

```
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

```
function fn(...args) { ... }
```

#### Python	

```
def fn(*args):
	...
```


### Destructuring

#### Javascript

```
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
&nbsp;

&nbsp;

&nbsp;


## Synopsis
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

&nbsp;

&nbsp;

&nbsp;

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



## Problem with the for-loop



&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;




## Overthrowing the for-loop

### filter(..)

<Filter-Content-Goes-Here>


&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;



## map(..)

![](https://cdn-images-1.medium.com/max/1280/1*MCkcOF6jUjP6J1GLzRuopw.gif =500x)

### iterator(..)

![](https://cdn-images-1.medium.com/max/1280/1*T93TuoVkdNryicN8wmRpYA.jpeg =500x)




&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;




### reduce(..)

* Most flexible
* String -> Number
* Array -> String
* Array -> Object
* Object -> Array
* Array -> Function
* Array -> RegEx


### String -> Number

```
const string = 'The wheels on the bus go round and round.';

const countLetter = letter => string => 
  string.split('').reduce((ct, ltr) =>
    (ltr === letter) ? ct + 1 : ct, 0)

```

```
const string = 'ich bin ein berliner'

const countIs = countLetter('i')
const countEs = countLetter('e')

countIs(string)
// => 4

countEs(string)
// => 3
```

### Array to String

```
const join = joint => collection.reduce((a, b, i) =>
  (i === 0) ? b : a + joint + b, '')
```

```
const lyrics = ['like', 'a', 'rolling', 'stone'];

join(' ')(lyrics);
// => 'like a rolling stone'
```

### Array -> Object

```
const rToO = array => array.reduce((object, el, i) => 
  Object.assign(object, {[i]: el}), {})
```

```
rToO(['red', 'green', 'blue'])
// => {0: 'red', 1: 'green', 2: 'blue'}
```

### Object -> Array

```
const oToR = object => Object.keys(object).reduce(
  (array, k) => array.concat(object[k]), []);
```

```
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



* Array -> String
* Array -> Object
* Object -> Array
* Array -> Function *
* Array -> RegEx



### iterator(..)

![](https://cdn-images-1.medium.com/max/1280/1*T93TuoVkdNryicN8wmRpYA.jpeg =500x)


### reducer(..)

![](https://cdn-images-1.medium.com/max/2000/1*KW7fE-NkOGgThv79GYwz-w.jpeg =300x)


&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;

## Functors

* Something with a `map` function
* Map must follow that `map` contract
* Arrays!
* Object literals... unfortunately not. We can fix that

&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;

## Function Pilates

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

## Currying

* function arity
* allows for great composability

### Implementation

```
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


&nbsp;

&nbsp;

&nbsp;

---
&nbsp;

&nbsp;

&nbsp;



## Monads
* Promises are monads
* “Iterables” in Python are monads
* How do you "lift" the value out?
* 2 options...

```
// Async simulation
const doSomething = () => new Promise((resolve, reject) => {
  const payload = {
    data: 'I am the payload'
  }
  
  setTimeout(() => resolve(payload), 3000)
})
```

## Mutations

* Why are functional programmers obsessed with mutations?
* Programs can be reduced to three basic things:
	* inputs
	* outputs
	* side effects
* If we place a constraint on ourselves we may be more productive as a result.


```
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

## Making an Object a functor in JS

```
// Non-mutative (good)
const mappify = objLiteral => {
  const newObjLiteral = Object.assign({}, objLiteral);

  const map = iterator =>
    Object.keys(objLiteral).reduce((result, k) => {
      result[k] = iterator(objLiteral[k], k, objLiteral);
      return result;
    }, {});

  Object.defineProperty(newObjLiteral, 'map', {
    value: map,
    configurable: true,
    enumerable: false,
    writeable: true,
  });

  return newObjLiteral;
}

```

```

// Non-mutative (good)
const iteraterify = objLiteral => {
  const newObjLiteral = Object.assign({}, objLiteral);
  
  const it = function* values() {
    for (const k of Object.keys(objLiteral)) {
      yield [k, objLiteral[k]];
    }
  };

  Object.defineProperty(newObjLiteral, Symbol.iterator, {
    value: values,
    configurable: true,
    enumerable: false,
    writeable: true,
  });

  return newObjLiteral;
}


let o = mappify({a: 1, b: 2, c: 3});
```

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


### Arrays

```
for (const x of [1, 2, 3]) { ... }
```

### Generators

```
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
Nope.
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


```
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
