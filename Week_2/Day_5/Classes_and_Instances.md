# Classes and Instances

* In Object Oriented Programming (OOP), **classes** are the *blueprints* (templates) that we use to create **instances** of objects

## Class

* To declare a class, you would use the `class` keyword with the name of the class. For example, the `Pizza` class:

```js
class Pizza {

}
```
* Class names should always be a **noun** and the **first letter should be capitalized.**

* To create a new object from a `class`, we would use the `new` keyword:

```js
let pizza1 = new Pizza();
let pizza2 = new Pizza();
```

* When you use a class to create an object, it's an **instance** of that class. So `pizza1` and `pizza2` are *instances* of the `Pizza` class.

* Note that these are not the same, however

```js
pizza1 === pizza2 // false
```

* If we wanted every pizza to start with cheese, and then allow other toppings to be added, that would look like this:

```js
class Pizza {

  constructor() {
    this.toppings = ["cheese"];
  }

  addTopping(topping) {
    this.toppings.push(topping);
  }

}
```

* To add properties to a class, we can simply use `this` keyword, followed by the property name, then assign it a value;

* The pizza object will now be able to add toppings with the `addTopping` method.

---

## Introduction to `constructor`

* A `constructor` is used to set up the default state for new instances; i.e., setting up default values for any new object's properties. 

* Every class can have a **single constructor** method that will get called when an instance of that class is created.
  * We can setup any default value to the constructor and therefore any instanced object
  * Because it's a method, we can also **pass in values** to the constructor method.

```js
class Pizza {

  constructor(size, crust) { // Pass in values size and crust
    this.size = size; // Assign them using `this`
    this.crust = crust;
    this.toppings = ["cheese"];
  }

  addTopping(topping) {
    this.toppings.push(topping);
  }

}
```

* This allows us to specify certain values of the constructor when instantiating an object:

```js
let pizza = new Pizza('large', 'thin');
```

## Dependency Injection

* The process of passing an object the information it needs when we create it. For example, our LHL cryptocurrency example:

```js
t1 = new Withdrawal(50.25, myAccount); // <-- myAccount
t1.commit();
```

 * This allows the deposit and withdrawal objects to not rely on any global or outerscoped data.

 * Additionally, transactions are no longer tied to only a single account. We can have these transaction records work with any account.