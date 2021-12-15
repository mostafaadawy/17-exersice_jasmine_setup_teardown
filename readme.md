Instructions

This exercise includes a preconfigured Jasmine and TypeScript project. The only file you should need to modify is indexSpec.ts. Available scripts can be found in package.json.

Your job is to use the setup/teardown and organizational skills you have learned to better organize the specs. You should

    Mimic the file structure of the src folder and create individual files for each module you are importing
    Use parent and child suites to organize the specs so that it is clear what each pairing does and which utility it belongs to when viewing the results in the spec reporter.

Once your specs are organized, use information from the spec reporter to track down the error that is causing one of the specs to fail and correct it.
Suggested Plan of Attack

    Create a utilities folder in tests and separate the specs onto their respective files to match the structure of the src folder.
    Organize each test pair into a suite with a description. Create parent suites and name accordingly for types of utilities. 3 Place any data needed to run the tests into the appropriate describe blocks for scoping.
    Run the tests and locate the error.
    Fix the error in the appropriate file.
    Run tests again to ensure all tests pass.

NOTE: a popup of "Renderer Failure: tsconfig.json" may open when starting this project. It is safe to ignore this error. Error is due to the comments in tsconfig.json not being considered valid JSON; however, it is generally considered safe to place comments in JSON config files.


Setup and Teardown of Suites

These Jasmine features allow you to

    Connect to a database before a test
    Connect to a different database for specific tests
    Run only a specific test
    Skip one or more tests

beforeEach and afterEach

    beforeEach takes a callback function where we can tell the test to perform a task before each test is run.
    afterEach is used if there is a task to be run after each test is complete.

Example:

describe("", () => {
  beforeEach(function() {
    foo = 1;
  });

  it("", () => {
    expect(foo).toEqual(1);
    foo += 1;
  });

  it("", () => {
    expect(foo).toEqual(2);
  });
});

beforeEach runs before and afterEach runs after each test

beforeEach runs before each test and afterEach runs after each test
beforeAll and afterAll

    To perform an operation once before all the specs in a suite, use beforeAll
    To perform an operation once after all the specs in a suite, use afterAll.

beforeAll runs before the tests and afterAll runs after the tests

beforeAll runs before the tests and afterAll runs after the tests
Handling More Than One Suite

Jasmine gives us the ability to use set up and teardown for more than just one suite. Whatever action is performed as setup or teardown for the parent suite, all sub-suites will also have access to the repeated or one-time setup or teardown.

Example:

describe("A spec", function() {
  beforeEach(function() {
    foo = 0;
  });

  it("is just a function, so it can contain any code", function() {
    expect(foo).toEqual(1);
  });

  describe("nested inside a second describe", function() {
    var bar;

    it("can reference both scopes as needed", function() {
      expect(foo).toEqual(bar);
    });
  });
});

Handling Multiple Suites with beforeAll and afterAll

Handling Multiple Suites with beforeAll and afterAll
Skipping or Specifying Tests

    To skip a test or suite, add x in front of describe or it. This can be helpful to avoid a time-consuming test.
    To focus on one test or suite, add f in front of describe or it. This reduces clutter in the terminal.

Example:

xdescribe("A spec", function() {
    it("is just a function, so it can contain any code", ()=> {
        expect(foo).toEqual(1);
    });
});

fdescribe("A spec", function() {
    it("is just a function, so it can contain any code", ()=> {
        expect(foo).toEqual(1);
    });
});

Set Up and Tear Down in Practice

In this demonstration, setup and teardown are used to organize the tests perform some functions to better run our tests.

    Suites are set up to organize the tests with one parent suite and 2 child suites.
    There are an object and an array providing data to be used within the child suites.
    beforeAll();can be used to run some code before the specs run, and any log statements show up before the specs.

    afterAll(); allows functionality to be added after all of the specs in a suite have run. Log statements will show after the specs.
    beforeEach(); and afterEach(); will run before or after each one of the individual specs.
    fdescribe and fit allows jasmine to focus on one specific suite, skipping the others
    xdescribe and xit allows Jasmine to skip a specific suite or test, running all others.

;

To Complete This Exercise:
Step 1: Organize Test Folders

Create a utilities folder in tests and separate the specs onto their respective files to match the structure of the src folder.

├── node_modules
├── spec
│ ├── support
│ └── jasmine.json
├── src
│ ├── tests
│ │ ├── helpers
│ │ │ └── reporter.ts
│ │ ├── utilities
│ │ │ ├── arraysSpec.ts
│ │ │ ├── numbersSpec.ts
│ │ │ └── stringsSpec.ts
│ │ └── indexSpec.ts
│ ├── utilities
│ │ ├── arrays.ts
│ │ ├── numbers.ts
│ │ └── strings.ts
│ └── index.ts
├── package-lock.json
├── package.json
└── tsconfig.json

Step 3: Organize the Tests

Organize each test pair into a suite with a description. Create parent suites and name accordingly for types of utilities and place any data needed to run the tests into the appropriate describe blocks for scoping.
indexSpec.ts

import newArr from '../index';

describe('newArr should add a new item to the start of array', () => {
    const wordArr = ['cat', 'dog', 'rabbit', 'bird'];

    it('should make a new array containing dog', () => {
        expect(newArr(3, wordArr)).toContain('dog');
    });
    it('make a new array containing 3', () => {
        expect(newArr(3, wordArr)).toContain(3);
    });
});


arraySpec.ts

import arrays from '../../utilities/arrays';

describe('Tests for array utilities', () => {
    const numArr = [3, 4, 5, 6];
    const wordArr = ['cat', 'dog', 'rabbit', 'bird'];

    describe('function addArr adds numbers in array', () => {
        it('should add numbers in array and be truthy', () => {
            expect(arrays.addArr(numArr)).toBeTruthy();
        });
        it('should add numbers in array and be 19', () => {
            expect(arrays.addArr(numArr)).toBe(19);
        });
    })

    describe('function concatArr concatinates 2 arrays', () => {
        it('should concatinate 2 arrays to not equal the first', () => {
            expect(arrays.concatArr(numArr, wordArr)).not.toEqual(numArr);
        });
        it('should concatinate 2 arrays to not equal the second', () => {
            expect(arrays.concatArr(numArr, wordArr)).not.toEqual(wordArr);
        });
    })

    describe('function cut3 removes 3 item in array', () => {
        it('should contain 3 items except rabbit', () => {
            expect(arrays.cut3(wordArr)).toEqual(['cat', 'dog', 'bird']);
        });
        it('should not contain the third index rabbit', () => {
            expect(arrays.cut3(wordArr)).not.toContain('rabbit');
        });    
    });

    describe('function lgNum gets the largest number in array', () => {
        it('should have 6 be largest number', () => {
            expect(arrays.lgNum(numArr)).toEqual(6);
        });
        it('should not have a large number and be falsy', () => {
            expect(arrays.lgNum(wordArr)).toBeFalsy();
        });
    });
});


numbersSpec.ts

import numbers from '../../utilities/numbers';

describe('Tests for number utilities', () => {

    describe('function sum gets sum of two numbers', () => {
        it('should be a sum greater than 10', () => {
            expect(numbers.sum(3,10)).toBeGreaterThan(10);
        });
        it('should be a sum less than 10', () => {
            expect(numbers.sum(-3,10)).toBeLessThan(10);
        });
    });

    describe('function multiply multiplies 2 numbers', () => {
        it('should multiply 3 by 5 and be 15', () => {
            expect(numbers.multiply(3,5)).toBe(15);
        });
        it('should multiply 0 by 5 to be falsy', () => {
            expect(numbers.multiply(0,5)).toBeFalsy();
        });
    });
});


stringsSpec.ts

import strings from '../../utilities/strings';

describe('Tests for string utilities', () => {
    describe('function capitalize capitalizes a string', () => {
        it('should capitalize a string', () => {
            expect(strings.capitalize('a sentence')).toEqual('A Sentence');
        });
        it('should allow sentence to remain capitalized', () => {
            expect(strings.capitalize('A Sentence')).toEqual('A Sentence');
        });
    });
});

Step 3:: Find the Error

Run the tests to locate the error.

npm run test


Step 4: Fix the Error in arraysSpec.ts

        it('should add numbers in array and be 18', () => {
            expect(arrays.addArr(numArr)).toBe(18);
        });


Step 5: Run Tests Again to Ensure All Tests Pass:

npm run test

Thinking Like a Test Driven Developer

What's another case you could envision using beforeAll, afterAll, beforeEach, or afterEach?
Your reflection
Accumulated and dependent testes
Things to think about

They become incredibly common when working with databases or with functionality where you need data to reset between tests.
;