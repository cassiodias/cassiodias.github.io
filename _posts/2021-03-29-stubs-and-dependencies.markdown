---
layout: post
title:  "STUBS AND DEPENDENCIES"
date:   2021-03-28 15:59
categories: coding, test, stub
---
# STUBS AND DEPENDENCIES

<p align="center">
    <img style="width: 250px; height: 250px;" src="https://f4.bcbits.com/img/a2792880258_10.jpg">
</p>

The stubs, band from Poland.

## Dependencies can hurt our tests

The book [Refactoring Test Code by Gerard Meszaros](https://www.amazon.co.uk/xUnit-Test-Patterns-Refactoring-Signature/dp/0131495054) (2007) is a classic pattern reference for unit testing. It defines patterns for things you fake in your tests in at least five ways.

> “Replace a component that the system under test (SUT) depends on with a much lighter-weight implementation.”

The dependency on outside modules or functions can and will make it harder to write tests and make it repeatable. We call those external things that we rely on in our code as **dependencies**. These could include things like time, async execution, file system, network, or simply be using something that is tricky to configure or might be time-consuming executing.

## Types of dependencies

**Outgoing dependencies:** dependencies that represent an exit point of our unit of work. For example: calling a logger, saving something to a database, sending an email, notifying an API, etc. It’s the representation of an exit point or the end of a specific logical flow.

**Incoming Dependencies:** dependencies that are not exit points. They don’t represent a requirement on the eventual behavior of the unit of work. They are merely there to provide test-specific data or behavior into the code. For example a DB query’s result, a file’s content, an API response, etc. These are all pieces of data that flow inward into the code as a result of a previous operation.

Today in many places we hear the word “mock” as a catch-all term for both stubs and mocks. “We’ll mock this out”, “we have a mock database” and they really can create confusion. In fact, there are differences [between stub and mock](https://martinfowler.com/articles/mocksArentStubs.html#:~:text=Final%20Thoughts-,Mocks%20Aren't%20Stubs,mimic%20real%20objects%20for%20testing.&text=What's%20often%20not%20realized%2C%20however,a%20different%20style%20of%20testing.). We should use the right term to remove assumptions about what the other person is referring to.

With these types of dependencies in mind, let’s have a look at how [XUnit Test Patterns defines](http://xunitpatterns.com/Mocks,%20Fakes,%20Stubs%20and%20Dummies.html) the different patterns:

|Pattern	|Purpose	|Has Behavior	|Injects [indirect inputs](http://xunitpatterns.com/indirect%20input.html) into SUT	|Handles [indirect outputs](http://xunitpatterns.com/indirect%20output.html) of SUT	|Values provided by test(er)	|Examples	|
|---	|---	|---	|---	|---	|---	|---	|
|[Test Double](http://xunitpatterns.com/Test%20Double.html)	|Generic name for family	|	|	|	|	|	|
|[Dummy Object](http://xunitpatterns.com/Dummy%20Object.html)	|Attribute or Method Parameter	|no	|no, never called	|no, never called	|no	|Null, "Ignored String", new Object()	|
|[Test Stub](http://xunitpatterns.com/Test%20Stub.html)	|Verify indirect inputs of SUT	|yes	|yes	|ignores them	|inputs	|	|
|[Test Spy](http://xunitpatterns.com/Test%20Spy.html)	|Verify indirect outputs of SUT	|yes	|optional	|captures them for later verification	|inputs (optional)	|	|
|[Mock Object](http://xunitpatterns.com/Mock%20Object.html)	|Verify indirect outputs of SUT	|yes	|optional	|verifies correctness against expectations	|outputs & inputs (optional)	|	|
|[Fake Object](http://xunitpatterns.com/Fake%20Object.html)	|Run (unrunnable) tests (faster)	|yes	|no	|uses them	|none	|In-memory database emulator	|
|[Temporary Test Stub](http://xunitpatterns.com/Test%20Stub.html#Temporary%20Test%20Stub)* ** (see Test Stub)*	|Stand in for procedural code not yet written	|yes	|no	|uses them	|none	|In-memory database emulator	|

## Approaching dependencies with Stubs

### Reasons to use stubs

> Stubs provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test. Stubs may also record information about calls, such as an email gateway stub that remembers the messages it 'sent', or maybe only how many messages it 'sent' - Martin Fowler**.**


Let’s use this piece of code: a password verifier that can’t work on weekends.

```
const moment = require('moment');
const SUNDAY = 0, SATURDAY = 6;

const verifyPassword = (input, rules) => {
    const dayOfWeek = moment().day();
    if ([SATURDAY, SUNDAY].includes(dayOfWeek)) {
       throw Error("It's the weekend!");
    }
    //more code goes here...
    //return list of errors found..
    return [];
};
```

The code has a direct dependency on *moment.js*, which is a very common date/time utility for JS.

How does this direct usage of a time-related library affect our unit tests? The unfortunate issue here is that this direct dependency forces our tests, given no direct way to affect date and time inside our application under test, to also take into account the correct date and time.

```
const moment = require('moment');
const {verifyPassword} = require("./file.js");
const SUNDAY = 0, SATURDAY = 6, MONDAY = 2;

describe('verifier', () => {
     const TODAY = moment().day();
     //test is always executed, but might not do anything
     test('on weekends, throws exceptions', () => {
         if ([SATURDAY, SUNDAY].includes(TODAY)) {
             expect(()=> verifyPassword('anything',[]))
                 .toThrowError("It's the weekend!");
         }
     });
 });
```

**The tests never even execute unless it’s the weekend!** Of course, we could add a negative IF to run it on weekdays, however, the test is not consistent. Every time I run, it should be the exact same that had run before. Also, the values being used don’t change and the asserts don’t change.

If no code has changed then the test should provide the same result. Basically here code coverage reports will be smaller over weekends.

### Approaches to stubbing

**Common functional approaches: **

* Function as parameter
* Factory functions
* Constructor functions

**Common object-oriented approaches: **

* Constructor injection
* Common interface as parameter

### **Stubbing out time with parameter injection**

Two benefits of control the time based on the above example:

1. able to remove the variability from our tests.
2. able to simulate easily any time related scenario we’d like to test.

```
 const verifyPassword = (input, rules, currentDay) => {
     if ([SATURDAY, SUNDAY].includes(currentDay)) {
         throw Error("It's the weekend!");
     }
     //more code goes here...
     //return list of errors found..
     return [];
 };
 
// Test:
 const SUNDAY = 0, SATURDAY = 6, MONDAY = 2;
 describe('dummy object', () => {
     test('on weekends, throws exceptions', () => {
         expect(()=> verifyPassword('anything',[],SUNDAY ))
             .toThrowError("It's the weekend!");
     });
 });
```

By adding the currentDay parameter, we’re essentially giving the control overtime to the caller of the function (the test). What we’re injecting is formally called a “dummy” – it’s just a piece of data with no behavior. But we can just call it a **stub**.

It’s a simple refactoring, but quite effective. it provides a couple of nice benefits other than just consistency in the test:

* We can now easily simulate any day we want
* The code under test is not responsible for managing time imports, so it has one less reason to change if we ever use a different time library.

### **Quick recap**

* **Dependencies:** things that make our test & code maintainability difficult since we cannot **control** them from our tests, such as time, file system, network, random values, and more.
* **Control:** ability to instruct a dependency on how to behave. Whoever is *creating* the dependencies is said to be in control over them since they have the ability to configure them before they are used in the code under test.
* **Inversion** **of Control:** designing the code to remove the responsibility of creating the dependency internally and externalising it instead.
* **Dependency Injection: the** act of sending a dependency through the design interface to be used internally by a piece of code.

Adding a parameter did solve the dependency issue at the function level, however, now every caller needs to know about how to handle dates in some way. So, here we have some techniques to handle that.

## Functional Injection Techniques

JS enables two major styles of programming: functional and object-oriented. For that reason alone, it would be wise to learn both approaches so that you apply whichever works best for the team you’re working with. Either way, the pattern remains largely the same, we just translate them to different styles.

### Function injection

```
const verifyPassword = (input, rules, getDayFn) => {
     const dayOfWeek = getDayFn();
     if ([SATURDAY, SUNDAY].includes(dayOfWeek)) {
         throw Error("It's the weekend!");
     }
     //more code goes here...
     //return list of errors found..
     return [];
 };
// Test
describe('dummy function', () => {
     test('on weekends, throws exceptions', () => {
         const alwaysSunday = () => SUNDAY;
         expect(()=> verifyPassword('anything',[], alwaysSunday))
             .toThrowError("It's the weekend!");
 });
```

There’s very little difference from the previous test but using a function as a parameter is a valid way to do the injection. In other scenarios, it’s also a great way to enable special behavior such as simulating special cases or exceptions into your code under test.

### Factory functions injection

Functions that return other functions, pre-configured with some context are called factory functions. In our case, the context can be the list of rules and the current day function.
This basically turns the factory function into the “arrange” part in the test, and calling the returned function the “act” part of the test.

```
const SUNDAY = 0, … FRIDAY = 5, SATURDAY = 6;

const makeVerifier = (rules, dayOfWeekFn) => {
   return function (input) {
       if ([SATURDAY, SUNDAY].includes(dayOfWeekFn())) {
           throw new Error("It's the weekend!");
       }
       //more code goes here..
   };
}; 
// Test
describe('verifier', () => {
   test('factory method: on weekends, throws exceptions', () => {
      const alwaysSunday = () => SUNDAY;
       const verifyPassword = makeVerifier([], alwaysSunday);
      expect(()=> verifyPassword('anything'))
           .toThrow("It's the weekend!");
 });
```

### Constructor Functions

Constructor functions are slightly more “OO” of achieving the same result of a factory function, but they return something to an object with methods that we can trigger.

```
 const Verifier = function(rules, dayOfWeekFn) {
   this.verify = function (input) {
       if ([SATURDAY, SUNDAY].includes(dayOfWeekFn())) {
           throw new Error("It's the weekend!");
       }
       //more code goes here..
   };
};
// Test
test("constructor function: on weekends, throws exception", () => {
   const alwaysSunday = () => SUNDAY;
   const verifier = new Verifier([], alwaysSunday);
   expect(()=> verifier.verify('anything'))
       .toThrow("It's the weekend!");
});
```

## OO injection techniques

### Constructor Injection

This is a design where we can inject dependencies through the constructor of a class. This is a pretty common approach if you have experience with AngularJS or Spring with Java/Groovy/Kotlin.
It can remove repetition from clients who would only need to configure our class once and then reuse the configured class multiple times.

```
class PasswordVerifier {
   constructor(rules, dayOfWeekFn) {
       this.rules = rules;
       this.dayOfWeek = dayOfWeekFn;
   }
   verify(input) {
       if ([SATURDAY, SUNDAY].includes(this.dayOfWeek())) {
           throw new Error("It's the weekend!");
       }
       const errors = [];
       //more code goes here..
       return errors;
   };
}
// Test 
test('class constructor: on weekends, throws exception', () => {
   const alwaysSunday = () => SUNDAY;
   const verifier = new PasswordVerifier([], alwaysSunday);

   expect(()=> verifier.verify('anything'))
       .toThrow("It's the weekend!");
});
```

It looks and feels a lot like the above “constructor function”. This is a more class-oriented design that many people would feel more comfortable with, coming from an object-oriented background. It also is more verbose. In fact, we get more and more verbose the more “object-oriented” we make things - it’s part of the OO game.

### Extracting a common interface

We can take things one step further and if we’re using Typescript or a strongly typed language such as Java we can start using interfaces to define roles that our dependencies do, creating a contract that both real objects and fake objects will have to follow.

First, in a new time-provider-interface file (typescript) we’ll define our new interface:

```
export interface TimeProviderInterface {
    getDay(): number;
}
```

Second, we define a ‘real’ time provider that implements our interface in our production code like this:

```
import * as moment from "moment";
import {TimeProviderInterface} from "./time-provider-interface";
 
export class RealTimeProvider implements TimeProviderInterface {
    getDay(): number {
        return moment().day();
    }
}
```

Third, we define the constructor to our PasswordVerifier to take a dependency on our new TimeProviderInterface, instead of having a parameter type of a RealTimeProvider. We’re abstracting the ‘role’ of a time provider and declaring that **we don’t care** what is the object being passed, as long as it implements the interface.

```
import {TimeProviderInterface} from "./time-provider-interface";
export const SUNDAY = 0, SATURDAY=6;
 
export class PasswordVerifier {

    private _timeProvider: TimeProviderInterface;
 
    constructor(rules: any[], timeProvider: TimeProviderInterface) {
        this._timeProvider = timeProvider;
    }
 
    verify(input: string):string[] {
        const isWeekened = [SUNDAY, SATURDAY]
            .filter(x => x === this._timeProvider.getDay())
            .length > 0;
 
        if (isWeekened) {
            throw new Error("It's the weekend!")
        }
        // more logic goes here
        return [];
    }
}
```

Now, by having an interface to define what something looks like, we can stub it in our own tests. It’s going to look a lot like the previous test’s code, but it will have one strong difference: it will be compiler-checked.

```
import {TimeProviderInterface} from "./time-provider-interface";
 
class FakeTimeProvider implements TimeProviderInterface {
    fakeDay: number;
    getDay(): number {
        return this.fakeDay;
    }
}
// Test
describe('password verifier with interfaces', () => {
    test('on weekends, throws exceptions', () => {
        const stubTimeProvider = new FakeTimeProvider();
        stubTimeProvider.fakeDay = SUNDAY;
        const verifier = new PasswordVerifier([], stubTimeProvider);
 
        expect(()=> verifier.verify('anything'))
            .toThrow("It's the weekend!");
    });
});
```

## Wrapping up!

> Congratulations if you read till here 😀.

So, those are some examples from functional design to strongly typed, object-oriented design. Which one is best for your team and your project? There is no single answer. Again, the injection remains largely the same, it just uses a different vocabulary or language features to be enabled.

Eventually, it is the injection-ability that enables us to simulate things that would be practically impossible to test in real life, and that’s where the idea of stubs shines the most.
