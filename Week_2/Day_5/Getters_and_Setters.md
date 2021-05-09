# Getters and Setters

* Getts and setters are special methods that are used to get the value of a property or set the value

```js
class Pizza {

  // Code from before

  setSize(size) {
    this.size = size;
  }
  getSize() {
    return this.size;
  }

}


// DRIVER CODE
const pizza = new Pizza();
pizza.setSize('m');
pizza.getSize(); // m

console.log(pizza)
```

* Using getters and setters might seem somewhat pointless, but they are good for two reasons:
  * **Validating** data before assigning it to a property
  * **Computing** a value on the fly instead of simply pulling it out of the property

## Validating

* If our pizza was only allowed to be certain sizes, then a `setter` method called `setSize` would be useful to ensure no erroneous sizes were given:

```js
class Pizza {

  // ...

  // setSize now includes data validation
  setSize(size) {
    if (size === 's' || size === 'm' || size === 'l') {
      this.size = size;
    }
    // else we could throw an error, return false, etc.
    // We choose here to ignore all other values!
  }
}

// DRIVER CODE
let pizza = new Pizza();
pizza.setSize('s');
```

## Computer Value

* We could create a property to keep track of the price of pizza, but this would require keeping track of the price every time a topping or size changes.

* It's easier to compute the price of the pizza when it's needed:

```js
class Pizza {

  // ...

  getPrice() {
    const basePrice = 10;
    const toppingPrice = 2;
    return basePrice + (this.toppings.length * toppingPrice);
  }
}

// DRIVER CODE
let pizza = new Pizza();
pizza.getPrice();
```

## Easier Way to use `get` and `set`

* Because getters and setters are very common, JS classes have special `get` and `set` keywords that can be used to implement them more easily.

* The only thing to note is that we would need to change the property that we store the `size` into from the `setter`, otherwise we get an infinite loop of the setter being called.

```js
class Pizza {

  // ...

  // replace our custom getters / setters with these ones!
  get price() {
    const basePrice = 10;
    const toppingPrice = 2;
    return basePrice + this.toppings.length * toppingPrice;
  }

  set size(size) {
    if (size === 's' || size === 'm' || size === 'l') {
      this._size = size;  // preven infinite loop!
    }
  }
}
```

* This gives nicer syntax overall:

```js
let pizza = new Pizza();

pizza.price;      // instead of getPrice()
pizza.size = 's'; // instead of setSize(size)
```