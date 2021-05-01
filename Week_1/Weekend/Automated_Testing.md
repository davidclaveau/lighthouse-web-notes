# Automated Testing

* Automated testing is the practice of writing code to programmatically test the actual code we want to write

* Automated tessting offers several benefits from manual testing
  * It saves testing time (by not having to perform manual tests over and over)
  * It saves debugging time (catching bugs earlier)
  * It makes it easier to program (only keeping what we're working on in our heads)
  * Easier to come to a program after some time
  * Easier to work together (tests inform your teammates)
  * It acts as documentation (reading tests teaches you about the code)
  * Improves the quality of our code (code that's easy to test usually means better code)

* There's Unit Testing, INtegration Testing and Functional Testing. More to read [here](https://codeutopia.net/blog/2015/04/11/what-are-unit-testing-integration-testing-and-functional-testing/).

## Unit Testing

* Is the **practice of testing small pieces of code, typically individual functions, alone and isolated**. If your test uses some external resource, like the network or database, it's not a unit test.
* In a way, unit tests are the backbone of testing
  * Same methods you use for unti testing are also applicable to the other types of testing
* Unit tests are also great for preventing regressions - bugs that occur repeatedly.

##  Integration Testing

* Integration testing is used to **test how parts of the system work tegether, through an integration of the parts**.
* Unit testing would not talk to a real database, but integration testing would.
* Because of the added complexity, integration test are usually slower  than unit testing.
  * They may also require additional set up or configuration, such as setting up a test database.

## Functional Testing

* Also sometimes called E2E testing, or browser testing, they all refer to the same thing
* Functional testing is defined as **the testing of complete functionality of some application.**
  * In practice, this means a program to simulate using a browser to click around on pages to test the application
* Very complex and difficult to write and maintain, so you have far fewer functional tests.
  * They also run very slowly, as they simulate real user interaction, so page load times become a factor.

## The Happy Path

* The Happy Path is a path through a system where everything works, the data is correct, the system stays up, and users are well-behaved.
* We often test the happy path first, and, if all goes well, assume the system has no bugs.
* This means we have to get off the happy path, try things that no one would (or should) do, and see what happens.