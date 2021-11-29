# Influences

## `JavaScript` & `TypeScript`:

JS and TS' importance cannot be understated. It's remarkable how a language designed and built in under 2 weeks has managed to become the most important programming language in the world. It's probable that more javascript code has been written than all other programming languages combined, and it's from that humongus library that Ami gleans several key tenants of what MUST be in the language and what MUST NOT.

### Ideas to Keep

- Scripting language
  - Ami will be interpretted (default for dev) or compiled ahead-of-time (default for production)
- Extendable
  - AST should be able to handle supersets, subsets, and language extensions
  - The language and it's ergonomics should be changable over time
- Package Management
  - Ami will have it's own `npm`-like service
- Modules
  - This took JS quie awhile to figure out, but CJS was nearly there and ESM brings the idea to the final destination. Modules should be easily defined and imported between other files and projects. (Incompatibilities between the two formats aside)

### Ideas to Avoid

- Non-optional/Untunable Garbage Collector
  - Ami must provide the ability to choose/replace the default GC
  - Ami might even allow optional manual memory freeing
- Backwards Compatibility
  - Backwards compatibility is a good thing, but JS' philosophy of NEVER breaking old code caused it to have to bend and deform in many important places and prevent tons of improvements to the core fundamentals
  - Ami will provide indefinate backwards compatibility, but in a radically different manner detailed further down
- Single Threaded
  - JS has eventually gained access to additional threads via Workers, but their ergonomics cripples their usefulness
- Anemic Standard Library
  - JS' standard libary is almost non-existant, and what is there is usually filled with bugs and oddities left to rot because of the infinite backwards compatibility problem
  - JS' does have a pseudo-standard libary provided by the NodeJS runtime which is much better, but it comes with it's own mistakes
- Missing fundamental primitives
  - JS only has `boolean`, `string`, `object`, or `double`. How this never got fixed is a mystery.
- No Types
  - This was fixed via Typescript, but while TS is now a must-use tech for JS, it can't be ran directly (Deno claims to run it directly, but as of writing this doc it sill transpiles all TS to JS before running).
- `null` & `undefined`
  - `null` is the mistake programmers keep on making over and over again
  - It was a bad idea in the 1960s when it was made for ALGOL, and it's still a bad idea now in the 2020s

## `Go`:

Go is a really good langauge. It nails just about every fundamental piece that goes into a language. Onto the things Ami will be stealing and leaving alone.

### Ideas to Keep

- Goroutines & Channels
  - aka Fibers/GreenThreads
  - Stop processing a current stack and continue with another while waiting on a signal to resume
- Gofmt
  - Definitive codestyle and formatter
- Structural Typing and Type Inference
  - TS also brought this to JS and everything is better with it

### Ideas to Avoid

- No Generic Types
  - I understand why the authors of Go didn't include support for generics in Go, but they were wrong. (Go Generics are still just a proposal at time of this writing)
- `nil`
  - See `null` in JS

## `WebAssembly`

WebAssembly is really neat, and Ami will almost certainly be compilable to it very early in it's life.

### Ideas to Keep

- Code runs against a stable Virtual Machine
  - Useful as a compile target for other languages
- Sandboxing
  - WASM runs all WASM code in it's own process giving great security guarantees
- Multiformat source
  - WASM has no primary format, only a textual representation for ease of viewing, and a binary representation for ease of running
- FFI/Javascript-interop
  - Being able to almost freely interact with JS from WASM is a must-have for WASM to even exist
  - Ami won't interact with JS but should present interfaces for other processes or modules to interact in a similar way

### Ideas to Avoid

- 600kb memory limit
- no SIMD

## `Java`

Java has a storied history, but results can't be denied. It's a very productive language wordwide, and with the additions of GraalVM and Kotlin, it's future is looking bright still.

### Ideas to Keep

- Multiple & Tunable Garbage Collectors
  - Java's GCs (notably the G1 and Z) are amazing feats of engineering
  - The Plan for Ami's first GC is to be like the Z garbage collector

### Ideas to Avoid

- Factory Factories
  - The java boilerplate is legendarily awful, and earns that rep easily
- Maven
  - Never has a package manager failed me more

## `Common Lisp`

I'm not going to pretend I like lisps, but there are still good things to steal from it for Ami.

### Ideas to Keep

- Meta-progamming
  - An idea truly ahead of it's time. Maybe even permanently.
  - The ability for a language to redefine and extend itself to add new capabilities is crazy powerful. Most other languages settle for simple macros
- REPL
  - Lisp REPLs are in a league of their own. The closest we have in other languages are jupyter notebooks and friends

### Ideas to Avoid

- Paren hell
- Functional programming only
