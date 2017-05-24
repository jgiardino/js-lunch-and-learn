# Classes

[MDN Class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

Class is an es6 concept that was introduced to JavaScript to make it feel more like other
classical languages.  We covered constructor functions & prototypes first because a class
is a "syntax sugar" over this concept.

Classical inheritance is an object-oriented inheritance model.  This was not introduced
into JavaScript.  A class is intended to be just a simpler syntax for thinking about
constructors and prototypes.



## Comparing constructors, classes, and other languages

In simplest form, it is pretty easy to see how a class and a constructor match up:

```JavaScript
// a typical constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// the same thing as a class
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

```

Quick note, both of the above can be defined as expressions:

```JavaScript
// an anonymous function assigned to a variable
var Person = function(name, age) {
  this.name = name;
  this.age = age;
};

// an anonymous class assigned to a variable
var Person = class {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

```

For a trivial case as above, the class requires more typing.  However, it looks very similar
to a class in other languages:

```Ruby
# a Ruby class
# notice: no braces, semicolons, etc
class Person
  def initialize(name, age)
    @name = name
    @age = age
  end
end
```

```Python
# a python class
# notice: indentation
class Person
  def __init__(name, age):
    self.name = name
    self.age = age
```

```php
class Person {
  var $name;
  var $age;
  function __construct($name, $age) {
    $this->name = $name;
    $this->age = $age;
  }
}
```

```Java
// Java is the most verbose
public class Person {
    String name;
    int age;
    public Person() {}
    public Person(String name) {
      name = name;
    }
    public Person(String name, int age) {
      name = name;
      age = age;
    }  
}
```

```Go
// go does not have classes, it follows a different paradigm.
// it defines methods which operate on a common type.
```

### Constructor

The constructor method of a class takes over the job of the constructor function inself.  It
creates & `initializes` the object created from the class, that is, it sets the defaults.

### Class Methods

You can add methods to your class as follows:

```JavaScript
class Person {
  constructor(name) {
    this.name = name || 'John Doe';
  }

  sayHi(person) {
    if(person && person.name) {
      return 'Hi, ' + person.name + '. My name is ' + this.name;
    }
    return 'Hi, my name is ' + this.name;
  }
}

var bob = new Person('Bob');
var jane = new Person('Jane');
bob.sayHi(jane);
```

Adding methods to a class like this is equivalent to adding them to the prototype of a
constructor:

```JavaScript
function Person(name) {
  this.name = name || 'John Doe';
}
Person.prototype.sayHi = function() {
  if(person && person.name) {
    return 'Hi, ' + person.name + '. My name is ' + this.name;
  }
  return 'Hi, my name is ' + this.name;
}

var bob = new Person('Bob');
var jane = new Person('Jane');
bob.sayHi(jane);
```

The class simply hides away the concept of a prototype, though it is still there. A
`new Person` from a class and a `new Person` from a constructor function will act the
same way & interact without issues.

Unique to classes, the body of a class is always executed in
[strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode).  
This enforces some modern semantics, such as converting mistakes into errors.

### Static methods

Static methods are methods that are not available to objects that are instances of the class
itself. This can also be done the previous way:

```JavaScript
class Bird {
  constructor(color) {
    this.color = color
  }
  // compares colors of birds
  static compare(first, second) {
    return first.color === second.color;
  }
}

var bird1 = new Bird('blue');
var bird2 = new Bird('red');
// individual birds cannot compare their colors,
// but we can attach this .compare() function to the
// Bird class as a static method.  Its like the class
// itself is responsible for understanding the properties
// of "birdness", and thus it can be called upon to help us
// know if the birds are the same color (since the birds themselves
// probably don't know their colors)
var isSameColor = Bird.compare(bird1, bird2);
console.log('same color?', isSameColor);
```

### Inheritance & Extending Classes

Classes simplify the concept of inheritance by using the `extend` keyword.
The idea is that a new class can extend a previous class, providing a mechanism
of code reuse.  

In practice, extending classes can actually create a rigid structure that is
difficult to deal with.  Rarely do we create simplistic things like 'Person' or
'Car'.  The concept of "Composition over Inheritance" is the preferred approach.
Essentially it encourages building new objects with previously existing components,
rather than a huge tree of inheritance (where you may not actually want everything
from a previous class).  

```JavaScript
class Machine {
  constructor(name) {
    console.log('created ', name);
  }
}

class Vehicle extends Machine {
  constructor(make, model, year) {
    super(make + ' ' + model);
    this.make = make;
    this.model = model;
    this.year = year;
  }
}

// from here, we split the tree
// Car & Motorcycle extend Vehicle in slightly different ways
class Car extends Vehicle {
  constructor(make, model, year, mpg) {
    mpg = mpg || 25;
    super(make, model, year, mpg);
    this.wheels = 4;
  }
}

class Motorcycle extends Vehicle {
  constructor(make, model, year, mpg) {
    mpg = mpg || 40;
    super(make, model, year, mpg);
    this.wheels = 2;
  }
}

// instantiate a couple
var car = new Car('Tesla', 'Model S', 2016);
var cycle = new Motorcycle('Honda', 'Shadow', 2017);

```


## Homework

1. Create a `Vehicle`, `Car`, and `Motorcyle` class as we did above.  Then do the following:

```JavaScript
// 1. Create Vehicle, Car, Motorcycle classes
// 2. Optionally pass make, model, year, etc.
// 3. When creating a new object of any of these 3 types, it should randomly generate a
//    maxSpeed for the object.
// 4. Vehicle, the base class for the other classes, should have a static-method that can be
//    used to compare max speeds of vehicle.
//    4.1  write .compare() in such a way that it takes 2 vehicles & will tell you the fastest.
//    It should return something like "[make] [model] is faster than [otherMake] [otherModel]"
//    example: "Tesla Model 3 is faster than Toyota Prius"
//    4.2  update this static method so that it can take any number of vehicles as arguments
//    4.3 update this static method so that it can optionally take an array of vehicls as an argument



var car1 = new Car();
var car2 = new Car();
var moto = new Motorcycle();

Vehicle.compare(car1, car2);          // Tesla Model 3 is faster than Toyota Prius.
Vehicle.compare(car1, car2, moto);    // Honda Shadow is the fastest.
Vehicle.compare([car1, car2, moto]);  // Volkswagon Beetle is the fastest.
```

2.  Lets do something a little bit more real world and create our own mini version of jQuery.  Use
the following template:

```JavaScript
// here we have 2 classes & a selecting function to emulate how jQuery works
// 1.  add the addClass function to both the Element & ElementList classes
//  - the ElementList should typically just loop its array of elements & call
//    the appropriate function on each Element
//  - the Element class should have the addClass method as well, but this one
//    should be responsible for actually adding the class to the underlying node.

// each element should be represented by an object like this:
class Element {
  constructor(elem) {
    this.elem = elem;
  }
}

// it is handy to be able to work with lists, so lets make a list class:
class ElementList {
  constructor(elems) {
    var elements = [];
    elems.forEach(function(elem) {
      elements.push(new Element(elem));
    });
    this.elements = elements;
  }
}

// now, we have the `$` function that does our selecting:
window.$ = function(selector) {
  var nodes = document.querySelectorAll(selector);
  return new ElementList(nodes);
};

// select (will return an elementList)
var foo = $('.foo');
// the elementList should be able to loop over
// each of its elements & call the element's addClass() method.
// (meaning the elementList should not add the classes directly!)
foo.addClass('bar');
```

3.  Add some more of the typical jQuery methods to your classes:

- addClass
- removeClass
- hasClass
- show
- hide  
- append
- prepend
- text
- html 
- on
- off
- find
- remove

If you aren't sure what these do, look it up on the jQuery API docs, for
example, [removeClass](https://api.jquery.com/removeclass/).  `.removeClass`
simply takes the string name of a class and, if it exists in the elements class list,
removes it.

Note as well that you can make helper methods/functions.  Even though 'getClasses' may not
be an exposed method, it might be very useful to you to have a function that will get the elements
classList & .split(' ') it into an array (hint).