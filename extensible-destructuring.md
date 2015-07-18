# Extensible Destructuring

## Rationale


Programmers use `Object`s for the purpose of `String` keyed hash maps. On some
occasions, Objects are not well suited as *map like* data structure and that is
the reason why alternative datastructures like ECMAScript 2015 `Map`, or
[ImmutableJS Map]( https://facebook.github.io/immutable-js/docs/#/Map
ImmutableJS Map) exist and get used.

Object destructuring has become part of ECMAScript 2015. 
http://www.ecma-international.org/ecma-262/6.0/#sec-destructuring-assignment

Currently programmers who use alternative *map like data structures* instead of
`Object` are out of luck. They can't do destructuring on these data structures
which results in very verbose code:

Compare

```es6
const {author: {name: {first, last}, birthdate}} = book;
```

With
```es6
const first = book.get('author').get('name').get('first')
const last = book.get('author').get('name').get('last')
const birthdate = book.get('author').get('birthdate')
```

## Specification

Let's add `Symbol.get` (`@@get`). Then add following to `Object.prototype`

```es6
Object.prototype[Symbol.get] = (key) => this[key]
```

Whenever object destructuring is used, `@@get` method is used to extract
value of keys.

Then destructuring example from our previous example would be equivalent to

```es6
const first = book[Symbol.get]('author')[Symbol.get]('name')[Symbol.get]('first')
const last = book[Symbol.get]('author')[Symbol.get]('name')[Symbol.get]('last')
const birthdate = book[Symbol.get]('author')[Symbol.get]('birthdate')
```

## Implementation by transpilers
Very straightforward. It should be very easy to implement in Babel.
