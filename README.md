# Jex
An experimental language with a fresh look at functional OOP.

Jex (*one* in Gypsy) is an experimental programming language that explores some fresh ideas around functional and object-oriented programming. In my 10 or so years of professional software development, I've spent many long hours daydreaming about new patterns and paradigms for programming, and Jex is my first attempt at making something out of those various ideas.

## Disclaimers <sup>please<sup>be<sup>nice</sup></sup></sup>

* This is an experimental language. Do not use or it will harm your babies.
* I do have a Ph.D. in computer science but in an unrelated field. I'm not up-to-date with current programming language research; so I essentially have no idea what I'm doing.
* The ideas presented here may be described in other forms in prior art. I'm not aware that this is the case (as I haven't looked) but I'd be keen to know of any similar efforts.
* I'm doing this 100% for fun, and I will most probably abandon this project in a week or so.

# Hyper typing

On the spectrum of weak vs strong typing, Jex is a *very* strongly typed language, and even turns it up to eleven.

**As a rule, primitive types should never appear in any type definitions**.

Idiomatic Jex code builds type definitions that are as precise as possible, given the application domain. For example:

```
class NonNegativeInteger extends Integer

class NonNegativeFloat extends Float

class Millimeters extends NonNegativeFloat

class DegreesCelcius extends Float

class ArrayIndex extends NonNegativeInteger

...
```

Representing concrete application-domain values with raw primitive types is an anti-pattern and a source of numerous bugs. Jex solves this from two opposite angles:
* Jex makes defining new types really easy, reducing the overhead of having to define them.
* Jex makes it really hard to reuse primite types thanks to *Type unicity by scope* (see below).

# Type unicity by scope

Jex's most radical idea is a very strong (crippling, even) limitation on how types can be used:

**Jex dissallows having more that one value of a type in a scope.**

Crazy right? I know. This limitation may seem radical and hard to work with (and it some ways, it is) but it also brings a suprising amout of very useful benefits, as will hopefully become clear.

# Unification of variables and types

Jex's second-most radical idea is that, for better or worse, it removes one concept that pretty much all other programming languages have:

**Jex does not have variables. Values are always referred to through their type.**

Again, pretty crazy. 

# (Co-)domain identical functions exception

# Orderless argument lists

# Orderless currying 

# Implicit self

# Closed scoping




