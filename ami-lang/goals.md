# Ami's Goals & Non-Goals

- __Goal__: Something to achieve or strive for in the language
- __Non-Goal__: Something to avoid or prevent in the language

## Goals
- [] VM based system
  - Ami code is compiled to a simpler bytecode and run on a pre-defined VM runtime
  - Like Erlang/Elixir or modern JS
- [] 64bit systems or greater only
- [] Safety First
  - [] No nulls
  - [] No globals
  - [] No undefinded values
  - [] No undefinded behavior
  - [] No shadowing
- [] Pure functions by default
  - (see details in syntax document)
- [] Error bubbling
  - Errors bubble up from the source to the first `catch` block they encounter
  - If not being caught, the program will use the default error handler at the root level
    - This will display an error message to the user and then exit the program
- [] All operations overloadable
  - all operands `+, -, *, /, ^` can be overloaded by the first operated upon object
    - `+` => `.add()` and `.toAdditionFormat()`
    - `-` => `.subtract()` and `.toSubtractionFormat()`
    - `*` => `.multiply()` and `.toMultiplicationFormat()`
    - `/` => `.divide()` and `.toDivisionFormat()`
    - `^` => `.raiseExponent()` and `.toExponentialFormat()`
    - etc...
  - Eg:
    ```ts
    var vector1 = Vector(1,2,3);
    var vector2 = Vector(2,3,4);

    var result = vector1 + vector2;
    // is compiled as
    var result = vector1.add(vector2.toAdditionFormat());
    ```
- [] All keywords can have additional aliases defined for localization
  - This should allow the entire language to be translated to all/most languages in the world
  - Even custom ones
- [] Strict structural typing
  - All variables must have a statically defined type
  - Compiler will warn


## Future Goals
- Define custom keywords and operators
  - Ami will not come with built-in binary operators, but someone could write a libary to add them in!
- TypeScript style type inference
  - Only require static typing when it can't be deduced


## Maybes
- Compile time macros
  - Might cost more than it's worth
  - Might encourage bad-patterns/abuse (see C++ templates)


## Non-Goals
- Maximum Execution Speed
  - Ami is not trying to be the fastest language to do any task in.
  - Just be fast enough
- Fit every use-case
  - Ami isn't trying to be fit for every area of programming.
  - Some areas like embedded systems are absolutely not part of the ami intended use areas.
