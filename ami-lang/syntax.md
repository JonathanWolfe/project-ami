<!-- omit in toc -->
# Ami Syntax

<!-- omit in toc -->
## Table of Contents
- [Whitespace](#whitespace)
- [Comments](#comments)
  - [Block Comments](#block-comments)
  - [Inline Comments](#inline-comments)
- [Variables & Constants](#variables--constants)
  - [Declaring Variables](#declaring-variables)
  - [Declaring Constants](#declaring-constants)
- [References and Clones](#references-and-clones)
- [Functions](#functions)
- [Strings](#strings)
- [Numbers](#numbers)
  - [Notes on Numbers and Math](#notes-on-numbers-and-math)
- [Raw Bits/Bytes](#raw-bitsbytes)
- [Loops](#loops)

## Whitespace

- Ami does not consider whitespace significant except for:
  - comments
  - string contents
  - number literals
- Ami considers all zero-width utf-8 characters as whitespace
- Whitespace is not stored in the AST
- Ami's code formatter will insert ASCII spaces when it adds whitespace

## Comments

### Block Comments

Everything inside the `/*` and `*/` are considered comment text
```ts
/* This is a single-line block comment */

/*
 * This
 * is
 * a
 * multi-line block comment
 */
```

### Inline Comments

Everything after the `//` is considered comment text
```ts
// This is an inline comment

var foo = true; // any code must come before the double-slashes
```


## Variables & Constants

### Declaring Variables

Variables in Ami must be in either camelCase or snake_case, cannot start with a number, and cannot contain whitespace. PascalCase is reserved for types. UPPERCASE is reserved for constants. To declare a variable you use the `var` keyword. Variables must end with a semicolon.

```ts
var foo = true;
```

You cannot do multi-declare single-var statements.
```ts
// INVALID
var foo = true, bar = false;

var foo = true,
    bar = false;

var who AmI = "who who, who who";

// VALID
var foo = true; var bar = false;

var foo = true;
var bar = false;

var whoAmI = "who who, who who";
```

### Declaring Constants

Constants in Ami are true constants since they cannot be re-bound, re-declared, or mutated. It is managed by a special type of reference object, so all usage of a constant is pass-by-reference. Using `ref` or `clone` on a constant is a no-op returning the constant. Constants must be declared using the `const` keyword, be in all caps, and not start with a number. Constants must end with a semicolon.

```ts
const SETTINGS = {
    cwd: arguments?.cwd || process.cwd,
    targets: arguments?.targets || ['**/*'],
};
const VERBOSE_MODE = new Boolean(process.env.VERBOSE || false);
const PORT = 3000;

const VERBOSE_MODE = false; // INVALID: redeclartion
var VERBOSE_MODE = false; // INVALID: shadowing
SETTINGS.handleErrors = arguments.handleErrors; // INVALID: constant mutation
PORT += 100; // INVALID: constant mutation
```

Constants can be detected via their readonly property `_IS_CONST` (a boolean);

## References and Clones

Ami is a Last-Write-Wins lock-free language. Mutating a reference in one thread does not signal to the others that a mutation occured, and subsequent reads will return the new value in all threads.

- `ref(T)` creates a sharable pointer that allows for mutations
    - `ref(T).pointer` will return the raw 64bit integer memory address of the pointer
    - `ObjAt(rawPointer)` will return a reference to the object at the specificed memory address (if exists and is a valid obj start prefix)
    - Referencing a reference is a no-op, returning the inputed reference
    - `ref(T) === ref(ref(T)) // true`
- `clone(T)` creates a deep-clone of the object
    - Clones of a constant produce a mutable copy
- Functions wrap all arguments in `clone(T)` unless the argument is a constant or variable

## Functions

Functions in Ami are declared via the `function` keyword. Their arguments list is comma-separated and must be typed. Like variables they follow the `camelCase` or `snake_case` naming schemes. To end execution of a function, use the `return` keyword with a value. Functions are invoked by using parenthesis after the function's name.

```ts
function doSomething(start: Int, prefix: Ref<String>) {
    while(start < 10) {
        start += 1;
        debugger.info(prefix, start)
    }

    return start;
}

doSomething(3, 'Current Value')
```


## Strings

Strings are created by using two single or double quotes. Strings in Ami are always utf-8 encoded. Strings can contain interpolated values by using double curly bracket wrappers. Interpolated values can be any valid Ami value or expression and will be converted to a string by calling the resulting type's `.toString` method.

```ts
var simple = 'A simple string';
var double = "With double quotes";
var withValue = 'Hello from {{ country }}!';
var doubleValue = "Yours Truly, {{ getLastName() }}';
var math = '2 + 2 = {{ 2 + 2 }}';
```

## Numbers

Numbers in Ami are declared using the ASCII 0 through 9 characters in an unbroken sequence with an optional `-` at the beginning to denote sign. A comma or period is used to denote decimal splitting. Optional underscores may be used to separate numbers for easier reading (they have no syntactic effect, but are preserved in the AST). Ami does not allow scientific notation or other formats. Any character other than the previous mentioned will result in a syntax error.

```ts
var int: Int = 123;
var decimal: Decimal = 123.456;
var fancy: Decimal = -1_234_567,890;
var phone: Int = 555_555_5555;

var bad: Int = 5e27; // INVALID
var mistyped: Int = 100.50; // INVALID: assigning fractional to integer
```

### Notes on Numbers and Math
- Ami uses fixed-point arithmatic
- Ami does not use floating point arithmatic (IEE-754 or otherwise)
- Non-Fractionals are stored as 64bit signed ints by default
  - This can be shortend by adding an argument to the type in the declaration
    ```ts
    var tenDigits: Int<10> = 0;
    ```
  - Unsigned Integers can but created by using the `UInt` type
    ```ts
    var unsigned: UInt<10> = 00_0000_0000;
    ```
- Fractionals are stored as two 64bit ints by default
  - Both halves can be shortened by adding an argument to the type in the declaration
    ```ts
    var monetary: Decimal(64, 2) = 0.00;
    var oddButOkay: Decimal(3, 50) = 123.4567890;
    ```
  - Fractionals cannot be unsigned
- Trailing zeros are ignored when compiling but preserved in the AST


## Raw Bits/Bytes

Bits operate the exact same as unsigned integers except for allowing rollover which are tracked by a readonly property.

```ts
var sixtyFourBit: Bit = 0;
var eightBit: Bit<8> = 0000_0000;

debugger.info(eightBit.rollovers); // outputs: 0
```


## Loops

There are 2 looping constructs in ami: `while` and `for`.

### While Loops

The loop's body will be repeated until the conditional in the parens evaluates to true. Only conditional expressions or values are allowed in the parens, not assignments.

```ts
var done = false;

while (!done) {
  debugger.info('looped again');
}
```

### For Loops

- The loop's body will be repeated a specific number of times. 
- For loops over non-intergers cause a new reference to be created with the provided name. 
- When given a collection, the loop will operate in index order (eg: 0,1,2,3...)
- When given a record/dictionary, the loop will operate in assignment order
- You cannot loop over decimal numbers
- Custom types will need to expose the methods `.getNextCollectionItem()` or `.getNextRecordEntry()` to be interoperable with for loops

```ts
// Over collection
var names: Collection<String> = ['John', 'Jacob'];

for (var item, index of names) {
  debugger.info("item: {{ item }}, index: {{ index }}");
}

// Over range (inclusive)
for (var n of 1..10) {
  debugger.info("n = {{ n }}");
}

// Over records
var data: Record<Boolean | Int> = { foo: true, bar: 2 };

for (var key, value of data) {
  debugger.info("key: {{ key }}, value: {{ value }}");
}
```

## Collections

Collections in ami are list data structure of undetermined size. Items in the collection can be accessed by the `.atIndex` method. Attempts to access negative indexes will operate from the tail of the collection instead of the head. Attempts to access an unfilled collection slot will throw an error.

```ts
var items: Collection<Int> = [1, 2, 3];

debugger.info(items.atIndex(1));
debugger.info(items.atIndex(-1));
debugger.info(items.atIndex(10)); // ERROR

items.add(4);
debugger.info(items.atIndex(3));
debugger.info(items.length); // 4

items.remove(2);
debugger.info(items.length); // 3

items = items.filter((value, index) => Math.isOdd(value));
debugger.info(items.length); // 2
```