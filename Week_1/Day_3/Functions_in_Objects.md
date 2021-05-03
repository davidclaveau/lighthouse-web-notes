# Functions in Objects

Example object:

```javascript
const snack = {
  flavour: 'zesty',
  spiciness: 7,
  texture: 'crunchy',
  amount: 20,
  dippable: true
};
```

* We have the abilitiy to add functions, and functions **define behavior**

```js
const snack = {
  flavour: 'zesty',
  spiciness: 7,
  texture: 'crunchy',
  amount: 20,
  dippable: true,

  eatSome: function(amountEaten) {
    snack.amount = snack.amount - amountEaten;
    console.log(`there is ${snack.amount} left`)
  }

};

console.log(snack);
snack.eatSome(6); // there is 14 left
```

* While this works, we would actually want to avoid

```javascript
    snack.amount = snack.amount - amountEaten;
```

* This is because, at any point in our code, the `snack` variable may change and is outside of our control

* In JS, if you call a function inside an object, it will point to the object itself (to the left of the dot)
  * We can take advantage of `this` to update our code to ensure it's called correctly in all cases

```javascript
const snack = {
  flavour: 'zesty',
  spiciness: 7,
  texture: 'crunchy',
  amount: 20,
  dippable: true,

  eatSome: function(amountEaten) {
    this.amount = this.amount - amountEaten;
    console.log(`there is ${this.amount} left`)
  }

};

console.log(snack);
snack.eatSome(6); // there is 14 left
```

* Another example with other functions calling functions in the same object:


```js
const snack = {
  flavour: 'zesty',
  spiciness: 7,
  texture: 'crunchy',
  amount: 20,
  dippable: true,

  eatSome: function(amountEaten) {
    this.amount = this.amount - amountEaten;
    console.log(`there is ${this.amount} left`)
  },
  goOnADiet: function(amount) {
    const reducedAmount = amount / 2;
    this.eatSome(reducedAmount); // Call the eatSome function within the object
  },
  changeTexture: function(newTexture) {
    this.texture = newTexture; // Change the texture to the newTexture value
  }
};
```

## Notes on Looping

* We can use `for...of` to loop through the **values** in an array
* We can use `for...in` to loop through the indexes (indices) of an array
* We can also use `for...in` to loop through the keys of an object
  * If you're trying to loop through the data values of each key, you would have to use **square bracket** as dot notation will look for a string and not work (`undefined`).