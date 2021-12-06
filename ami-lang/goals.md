# Ami's Goals & Non-Goals

- __Goal__: Something to achieve or strive for in the language
- __Non-Goal__: Something to avoid or prevent in the language

## Goals
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
    - `+` => `.add`
    - `-` => `.subtract`
    - `*` => `.multiply`
    - `/` => `.divide`
    - `^` => `.toThePowerOf`
  - Eg:
    ```js
    var vector1 = Vector(1,2,3);
    var vector2 = Vector(2,3,4);

    var result = vector1 + vector2;
    // is parsed as
    var result = vector1.plus(vector2);
    ```
- [] All keywords can have additional aliases defined for localization
  - This should allow the entire language to be translated to all/most languages in the world


## Non-Goals
