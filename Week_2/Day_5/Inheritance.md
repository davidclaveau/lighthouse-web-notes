# Inheritance

* As we start to create different types of objects, inevitably we start to see some code duplication.

* Consider the class `Student` and the class `Mentor`:

```js
class Student {
  // this constructor is identical to that of a mentor!
  constructor(name, quirkyFact) {
    this.name = name;
    this.quirkyFact = quirkyFact;
  }

  // here is a method that is specific to students
  enroll(cohort) {
    this.cohort = cohort;
  }

  // Identical! Smells of code duplication (WET)
  bio() {
    return `My name is ${this.name} and here's my quirky fact: ${this.quirkyFact}`;
  }
}

class Mentor {
  // this constructor is identical to that of a student!
  constructor(name, quirkyFact) {
    this.name = name;
    this.quirkyFact = quirkyFact;
  }

  // specific to mentors
  goOnShift() {
    this.onShift = true;
  }

  // specific to mentors
  goOffShift() {
    this.onShift = false;
  }

  // Identical! Smells of code duplication (WET)
  bio() {
    return `My name is ${this.name} and here's my quirky fact: ${this.quirkyFact}`;
  }
}
```

* We have the same `constructor`s and `bio` methods

* With **inheritance**, we can build a new class based on an existing class. In the scenario above, we can create a class called `Person` to bring in the similar methods.

```js
// This class represents all that is common between Student and Mentor
class Person {
  // Moved here b/c it was identical
  constructor(name, quirkyFact) {
    this.name = name;
    this.quirkyFact = quirkyFact;
  }

  // Moved here b/c it was identical
  bio() {
    return `My name is ${this.name} and here's my quirky fact: ${this.quirkyFact}`;
  }
}
```

```js
class Student extends Person {
  // Stays in Student class since it's specific to students only
  enroll(cohort) {
    this.cohort = cohort;
  }
}

class Mentor extends Person {
  // Specific to mentors
  goOnShift() {
    this.onShift = true;
  }

  // Specific to mentors
  goOffShift() {
    this.onShift = false;
  }
}
```

* `Student` and `Mentor` become **subclasses** of the `Person` class, since they are *extensions* of that class. `Person` is the **superclass** in this relationship.

---

## Method Overriding

* Sometimes you want a subclass to have similar, but different, behavior than a superclass.

* Like, having a mentor in the example above say "I am a mentor at LHLs..." before saying "My name is" in the `bio` method.

### Solution Part 1: Method Override

* We can simply override the method in the Mentor class:

```js
// Superclass
class Person {
  constructor(name, quirkyFact) {
    this.name = name;
    this.quirkyFact = quirkyFact;
  }

  bio() {
    return `My name is ${this.name} and here's my quirky fact: ${this.quirkyFact}`;
  }
}

// Subclass
class Mentor extends Person {
  // Completely re-define the bio method since it has more to say
  bio() {
    return `I'm a mentor at Lighthouse Labs. My name is ${this.name} and here's my quirky fact: ${this.quirkyFact}`;
  }
}

// The Student class doesn't need to define bio since it can just use the one from Person

// DRIVER CODE

const bob = new Mentor('Bob Ross', 'I like mountains way too much');
console.log(bob.bio());
```

* This shows how subclasses and superclasses give priority, but this clearly isn't a very DRY method.

### Solution Part 2: Use `super`

* OOP languages allow subclasses to have a reference on the parent class, and this is usually done via the `super` keyword, which JS supports too:

```js
// Super class
class Person {
  constructor(name, quirkyFact) {
    this.name = name;
    this.quirkyFact = quirkyFact;
  }

  bio() {
    return `My name is ${this.name} and here's my quirky fact: ${this.quirkyFact}`;
  }
}

class Mentor extends Person {
  // Mentor bios need to include a bit more info
  bio() {
    return `I'm a mentor at Lighthouse Labs. ${super.bio()}`;
  }
}

// DRIVER CODE

const bob = new Mentor('Bob Ross', 'I like mountains way too much');
console.log(bob.bio());
```
