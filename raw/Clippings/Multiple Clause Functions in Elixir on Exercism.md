---
title: "Multiple Clause Functions in Elixir on Exercism"
source: "https://exercism.org/tracks/elixir/concepts/multiple-clause-functions"
author:
published:
created: 2026-06-16
description: "Master Multiple Clause Functions in Elixir by solving 72 exercises, with support from our world-class team."
tags:
  - "clippings"
---
[Perk](https://exercism.org/adverts/e82a03d234574c52b9d4fc66df509cb3/redirect?impression_uuid=8431d7f0f42751f64af5b98e82558a1c)

[

Ship portfolio-worthy **audio projects** with the ElevenLabs API. Get **50% off** your first month of Creator.

Claim your discount

](https://exercism.org/adverts/e82a03d234574c52b9d4fc66df509cb3/redirect?impression_uuid=8431d7f0f42751f64af5b98e82558a1c)

## About Multiple Clause Functions

In Elixir, a single function can have multiple clauses. This is achieved by pattern matching the function's arguments and by using guards.

```elixir
# pattern matching the argument
def number(7) do
  "Awesome, that's my favorite"
end

# using a guard
def number(n) when is_integer(n) do
  "That's not my favorite"
end

def number(_n) do
  "That's not even a number!"
end
```
- Use [multiple function clauses](https://hexdocs.pm/elixir/modules-and-functions.html#function-definition) to extract control logic from functions.
- Clauses are attempted in order, from top to bottom of the source file until one succeeds.
- If none succeed, a `FunctionClauseError` is raised by the BEAM VM.
- If argument variables are not used either in the body of the function or in a guard, they should be prefixed with an `_` otherwise a warning is emitted by the compiler.
- Anonymous functions can also have multiple clauses.
	```elixir
	fn
	  13 -> "Awesome, that's my favorite"
	  _ -> "That's not my favorite"
	end
	```

Note that multiple clause functions should not be confused with function overloading that you might know from other programming languages. In Elixir, functions are identified by their name and arity only, not types of arguments (since there is no static typing). The function `number/1` from the example is considered to be a single function regardless of how many clauses it has.

[Edit via GitHub](https://github.com/exercism/elixir/tree/main/concepts/multiple-clause-functions)

### Learn Multiple Clause Functions

[![Icon for exercise called Guessing Game](https://assets.exercism.org/exercises/guessing-game.svg)](https://exercism.org/tracks/elixir/exercises/guessing-game)

[Guessing Game](https://exercism.org/tracks/elixir/exercises/guessing-game)

[

Learning Exercise

Learn about multiple clause functions, guards, and default arguments by implementing a simple game in which the player needs to guess a secret number.

](https://exercism.org/tracks/elixir/exercises/guessing-game)

![Practicing is locked](https://assets.exercism.org/assets/icons/lock-circle-ce2e3f3cd95b158ecabc5f97078aeb57d8cfade6.svg)