<!-- omit in toc -->
# Ami Syntax

<!-- omit in toc -->
## Table of Contents
- [Comments](#comments)
  - [Block Comments](#block-comments)
  - [Inline Comments](#inline-comments)
- [Variables](#variables)
  - [Declaring Variables](#declaring-variables)
  - [References and Clones](#references-and-clones)
- [Loops](#loops)

## Comments

### Block Comments

Everything inside the `/*` and `*/` are considered comment text
```js
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
```js
// This is an inline comment

var foo = true; // any code must come before the double-slashes
```


## Variables

### Declaring Variables

There is only one (1) way to declare a variable in ami: the `var` keyword. Variables must end with a semicolon.
```js
var foo = true;
```

You cannot do multi-declare single-var statements.
```js
// invalid
var foo = true, bar = false;

var foo = true,
    bar = false;

// valid
var foo = true; var bar = false;

var foo = true;
var bar = false;
```

### References and Clones

- `ref(T)` creates a sharable pointer that allows for mutations
    - `ref(T).pointer` will return the raw 64bit integer memory address of the pointer
    - `ObjAt(rawPointer)` will return a reference to the object at the specificed memory address (if exists and is a valid obj start prefix)
    - Referencing a reference is a no-op, returning the inputed reference
    - `ref(T) === ref(ref(T)) // true`
- `clone(T)` creates a deep-clone of the object
- Functions wrap all arguments in `clone(T)` unless already wrapped in a `ref(T)`
    ```js
    doSomething(foo, bar);
    // is parsed as
    doSomething(clone(foo), clone(bar));


    doSomethingElse(foo, ref(bar));
    // is parsed as
    doSomethingElse(clone(foo), ref(bar));
    ```


## Loops
