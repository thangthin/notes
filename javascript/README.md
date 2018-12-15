# this keyword
Used to set properties in constructor function. Constructor function implicitly sets the ```this``` to object and sets the properties and return the ```this``` object. Constructor function is used with ```new keyword```
```
function Person(name, age) {
    this.name = name;
    this.age = age;
}

let p = new Person('Thang', 3)
# {name: 'Thang', age: 3}
```
