# this keyword
Usage and behavior of this keyword in function

## Constructor function
Used to set properties in constructor function. Constructor function implicitly sets the `this` to object and sets the properties and return the `this` object. Constructor function is used with `new` keyword
```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

# prototype inheritance
Person.prototype.sayName = function() {
    console.log(`My name is ${this.name}`);
}

let p = new Person('Thang', 3)
# {name: 'Thang', age: 3}
```

## Dynamic scoping 
`this` has context dependency by how it is called. 
```javascript
p.sayName() # this gets sets to the calling object when called as method
# "My name is Thang"

let sayName = p.sayName
sayName() # this becomes global object - name could be undefined
```

## Setting Context of this
Three ways to set the context of `this` explicitly
- `bind` - returns a new function that has the `this` bounded to the object passed in
```javascript
let boundedSayName = sayName.bind(p1, "arg")
boundedSayName()
# "My name is Thang"
```
- `call` - sets the this object as first argument, and the rest of the arguments to the sayName function gets passed as regular argument parameters.
```javascript
let p2 = new Person('Tom', 4)
# {name:'Tom', age:4}
sayName.call(p2)
# "My name is Tom"
```

- `apply` - similar to `call`, but the arguments to the function gets passed as an array
```javascript
sayName.call(ps, [arg2, arg2])
```

***

# ES6

## Template String
```javascript
    `My name is ${p1.name}`
    # "My name is Thang"
```

## Default arguments
```javascript
function addDefault(x, y=5) {
    return x + y
}

addDefault(4) # return 9
addDefault(4,0) # return 4
```

## Rest and spread operator
- `rest` function definition that allows for variable amount of arguments
```javascript
    function betterAdd(zero, ...values) {
        var result = values.reduce((acc, v) => {
            return acc + v
        });
        return result;
    }
    
    betterAdd(0, 1) # 1
    betterAdd(0,2,3) # 5
```

- `spread` used to unwind the arrays into various arguments when calling functions
```javascript
let values = [0,2,4]
betterAdd(...values, 10) # 16
```

## Arrow Functions
has different scoping rules than regularly defined `function` definition - more lexical scoping
```javascript
    function Person(name) {
        this.name = name
    }

    // This gets called by setTimeout without `this` set
    Person.prototype.sayNameTimed = function() {
        setTimeout(function() {
            console.log(`My name is ${this.name}`) 
        },1000)
    }

    var p1 = new Person("Sean")

    p1.sayNameTimed() // My name is `undefined`

    // using self to set this
    Person.prototype.sayNameTimed = function() {
        self = this;
        setTimeout(function() {
            console.log(`My name is ${self.name}`)
        }, 1000);
    }

    // using bind to hard bind
    Person.prototype.sayNameTimed = function() {
        setTimeout(function() {
            console.log(`My name is ${this.name}`)
        }.bind(this), 1000)
    }

    // using arrow function to automatically use enclosing scope at definition
    Person.prototype.sayNameTimed = function() {
        setTimeout(() => {
            console.log(`My name is ${this.name}`)
        }, 1000)
    }

    p1.sayNameTimed() // My name is `Sean`
```

## Destructuring
- allows to map elements to variables quickly
```javascript
var arr = [1,2,3,4]
var [a, ,b] = arr // a, b gets mapped to first and third element respectively
```

- quickly swaps value between variables
```javascript
// a => 1 b=> 3
[a, b] = [b, a]
// a => 3 b=> 1
```

- when local variable name is the same as object property name, quickly destructure like so
```javascript
var p1 = {name: "thang", test:"b-"}
var {name} = p1
name // "thang"
```

- destructure nested sturctures
```javascript
let numbers = [[1,2,3],[4,5,6]]
let [[x,y,z], fourAndAbove] = numbers
console.log(x,y,z, fourAndAbove) // 1 2 3 [4, 5, 6]
```

## Export / Importing
- export default - exports works on all types
- Note that babel transpiling is needed to transpile es6 module system for node - look at simple node config repo
```javascript
// file one
export default function() {
    console.log("file one")
}

// file two
import mylibrary from './file-one'
```

- export multiple
```javascript
//file one
export function library() {
    console.log("file one library")
}

export function test() {
    console.log("file one test)
}

// file two
import {library, test} from './file-one'
```

## Classes
- syntactical sugar for constructor function, helps point method defined in class to prototype but still based on prototype inheritance
```javascript
class Person {
    constructor(name) {
        this.name = name // assign name to instance
    }

    // method definition doesn't need function keyword
    sayName() {
        console.log(`My name is ${this.name}`);
    }
}

let p1 = new Person("Thang")
p1.sayName() // My name is Thang

var sayName = p1.sayName // assigned function doesn't get automatically binded
sayName() // My name is undefined

var sayName = p1.sayName.bind(p1) // still have to manually bind if instance is needed
sayName() // My name is Thang
```

- Inheritance is a lil easier to write
```javascript
class Creature {
    constructor(name) {
        this.name = name;
    }

    sayName() {
        console.log(`My name is ${this.name}`)
    }
}

class Person extends Creature {
    constructor(name, age) {
        super(name) // sets parent constructor
        this.age = age
    }

    sayAge() {
        console.log(`My age is ${this.age}`)
    }
}

var p1 = new Person("Thang", 1)
console.log(p1.name) // Thang
console.log(p1.age) // 1
p1.sayAge() // My age is 1
p1.sayName() // My name is Thang
console.log(p1.sayName === Creature.prototype.sayName) // true
```