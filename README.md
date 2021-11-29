# Project Ami

Building the Next Generation of the Internet

## What is it?

Project Ami is a suite of tools, specs, and languages that detail and define a new way of creating apps, pages, and documents on the Internet.

Currently there are 2 parts to Project Ami:
- **Ami**: A new programming language designed for the post-modern world
- **Juju Browser**: A replacement for web browsers enabling ami apps to be discovered and rendered on user machines securely and quickly

## Why?

Because HTML, CSS, and JavaScript all have a bunch of flaws due to them being primarily design for other tasks and then coopted for modern web app development. Project Ami takes the 30 years of ideas and experience we've had with these tools, improves upon them, and bring ideas from other languages and tools into the mix.

# Ami Lang

Ami lang builds off the syntax of the C family, but with radically different internals more in line with Go and Elixer with a touch of Lisp. If while reading this you find a portion of the document to be underspecified, please open an issue but continue reading the with working theory that it operates like Go would in the situation.

## Influences

[Read the dedicated doc](./ami-lang/influences.md)

## Goals & Non-Goals

[Read the dedicated doc](./ami-lang/goals.md)

## Syntax

[Read the dedicated doc](./ami-lang/syntax.md)

# JuJu Browser

JuJu will be a browser meant to replace applications likes Chrome, FireFox, and Safari. It will reach out to programs written in Ami, run them in a locked down process sandbox, and read from Ami's graphics handlers to render them via the GPU (Unknown yet if fallback to a cpu software renderer will be provided, or if we make it hardware rendering only).

JuJu is much less defined than Ami at this point in time.