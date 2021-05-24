# Algorithms

* Algorithms is a set of instructions or steps for accomplishing a specific task
  * It can be as simple as logging to the console, summing two numbers, or the algorithms Google uses to find your location or give directions.

## Algorithmic Complexity

* **Algorithmic complexity** is all about how slow or how fast a particular algoritm is
  * You can't use *time* to calculate the algorithmic complexity, as time is too unreliable due to OS or device
  * We therefore count the elementary operations - that is, how many operation steps it takes to resolve the task
  * We can count the operations and call it the *running time*

```js
number1 + number2;
```

* This has a running time of `1`, as does this:

```js
console.log(someString);
```

* This has `4` operation:

```js
let result = 0;
result += number1;
result += number2;
console.log(result);
```

* When there are things that are being looped, the algorithm can have a more abstract number of operations

* For example, this snippet of code:

```js
let result = 0;

for (let i = 0; i < array.length; i++) {
  let number = array[i];
  result += number;
}

console.log(result);
```

* Consider the actions that are being performed:
  * `n` is the number of times the array has to loop
  * So if there are three operations in the loop, that means we perform `n` three times
  * `n + 1` is because the loop has to check to make sure it doesn't have to run again

```js
let result = 0; // 1

for (
  let i = 0; // 1
  i < array.length; // n + 1
  i++ // n
) {
  let number = array[i]; // n
  result += number; // n
}

console.log(result); // 1
```

* This means we get `1 + 1 + 1` and `3 * n` and `(n + 1)`
  * Therefore: `3 + (3 * n) + n + 1`
  * Which can be shown as `4n + 4`

---

## Linear Search versus Binary Search

* If you're looking for a value in a database, and you start from the beginning and look at each value, you will find the value but it will take you an exorbitant amount of time. This is considered a **Linear Search**

* Another way to do this is to look through the database by guessing directly in the middle of the dataset. If the database tells you it's higher or lower than your initial guess, then you can remove the section of the dataset it's not in and guess the value in the other subset of data (in the middle again) Continue this process until you correctly find what you're looking for. This is called **Binary Search**.

* Here's a step-by-step description of using binary search to play the guessing game:
  1. Let `min = 1`, and `max = n`.
  2. Guess the average of `max` and `min` rounded down so that it is an integer.
  3. If you guessed the number, stop. You found it!
  4. If the guess was too low, set `min` to be one larger than the guess.
  5. If the guess was too high, set `max` to be one smaller than the guess.
  6. Go back to step two.

* Every time we double the scale of our `n` dataset, our number of guesses only goes up by 1. This is a **base-2 logarithm** of `n`, and this is the inverse of exponential;

---

## Quadratic Time

* Quadratic algorithms can be created by nesting loops which is common when comparing every item in an array to every other item in that array.

* What we can do to accomplish quadratic algorithms is to have the array sorted from smallest to largest

* If we're checking the array to see if two values added together equal a specified number, we can do the following:
  * We start with the first value in the array and add to the last index
  * If the value returned is too large, the we try with the second largest number in the array (second to last value in array)
  * If that value is too small, then we check the second largest value with the second smallest value in the array (second value in array)
  * Continue until we're out of values to compare

* Any `for` loops nested within each other will create exponential operations

```js
function arrayContainsSum(array, sum) {

  for (
    let i = 0; // 1
    i < array.length; // n + 1
    i++ // n
    ) {
    const element1 = array[i]; // n

    for (
      let ii = 0; // n
      ii < array.length; //n + n^2
      ii++ // n^2
      ) {

      const element2 = array[ii]; // n^2

      if (element1 + element2 === sum) { // n^2
        return true;
      }
    }
  }
  return false; // 1
}
```

* Whereas if we use our quadratic algorithm, we significiantly reduce operations:

```js
function arrayContainsSum(array, sum) {
  let i = 0; // 1
  let ii = array.length-1; // 1

  while (i <= ii) { // n + 1

    const element1 = array[i]; // n
    const element2 = array[ii]; // n
    const currentSum = element1 + element2; // n

    if (currentSum === sum) { // n
      return true;
    } else if(currentSum > sum) { // n
      ii--; // n
    } else {
      i++;
    }
  }
  return false; // 1
}
```

---

## Big O Notation

* Big O Notation, written as `O()` describes how many number of steps in an algorithm scales relative to its input.

* We only care about **arbitrarily large input**.
  * What does the run time of binary search look like when we give it an array of one million items?
  * So we calculate `n` in large numbers, such as `100,000` or `1,000,000`

* We drop the **non-dominant terms**.
  * When our algorithm had a running time of `(n^2+n)/2`, it was the `n^2` that was hurting us. So we'll just forget about everything else.
  * Same as the arbitrarly large inputs, we need to enter large numbers to make non-dominant terms seem useless.
    * With a graph of `1000n + n^2` and just `n^2`, when we use large values for `n` (like `2x10^9`) we see that the graphs are similar.
    * Therefore, we can just plot `n^2`
  * We also only use the **highest order term**
    * So plotting a value of `n^0 + n^2 + n^3 + n^4` we only care about `n^4`

* We drop **constant terms**.
  * If you graph `(n^3)/2` or `(n^3)*2`, it has pretty much the same curve as n^3, so let's just get rid of the constant `2`.
