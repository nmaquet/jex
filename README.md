# Jex
An experimental language with a fresh look at functional OOP.

Jex (*one* in Gypsy) is an experimental programming language that explores some fresh ideas around functional and object-oriented programming. In my 10 or so years of professional software development, I've spent many long hours daydreaming about new patterns and paradigms for programming, and Jex is my first attempt at making something out of those various ideas.

## Disclaimers <sup>please<sup>be<sup>nice</sup></sup></sup>

* This is an experimental language. Do not use or it will harm your babies.
* I do have a Ph.D. in computer science but in an unrelated field. I'm not up-to-date with current programming language research; so I essentially have no idea what I'm doing.
* The ideas presented here may be described in other forms in prior art. I'm not aware of this being the case (as I haven't looked) but I'd be keen to know of any similar efforts.
* I'm doing this 100% for fun, and I will most probably abandon this project in a week or so.

# Hyper typing

On the spectrum of weak vs strong typing, Jex is a *very* strongly typed language, and even turns it up to eleven.

**As a rule, primitive types should never appear in any type definitions**.

Idiomatic Jex code builds type definitions that are as precise as possible, given the application domain.
<br>For example (`<` means `extends`):

```
class NonNegativeInteger < Integer

class NonNegativeFloat < Float

class Millimeters < NonNegativeFloat

class DegreesCelcius < Float

class ArrayIndex < NonNegativeInteger

...
```

Representing concrete application-domain values with raw primitive types is an anti-pattern and a source of numerous bugs. Jex solves this from two opposite angles:
* Jex makes defining new types really easy, reducing the overhead of having to define them.
* Jex makes using unprecise types inconvenient thanks to *type unicity by scope* (see below).

# Type unicity by scope

Jex's most radical idea is a very strong (crippling, even) limitation on how types can be used:

**Jex disallows having more that one value of a type in a scope.**

Crazy right? I know. This limitation may seem radical and hard to work with (and it some ways, it is) but it also brings a suprising amout of very useful benefits, as will hopefully become clear.

# Unification of variables and types

Jex's second-most radical idea is that, for better or worse, it removes one concept that pretty much all other programming languages have:

**Jex does not have variables. Values are always referred to through their type.**

Again, pretty crazy. The goal here is to make code a lot more concise. Dynamically typed languages are a lot less verbose than statically typed ones (consider Java's `Widget widget = WidgetFactory.createWidget()`... *shudders*). The thing is, Jex types are so precise that doing away with variables altogether is relatively easy and makes code very concise. For example:

```
def getWidget(WidgetId, Connection): Widget
	new PreparedStatement("select * from widgets where id = ?")
	PreparedStatement.setLong(WidgetId.toLong)
	PreparedStatement.executeQuery()
	ResultSet.next()
	new Widget(ResultSet)
```

As seen above, Jex code is pretty concise. Also, an interesting consequence of not having variables is that *Jex does not have an assignment operator*. 

**But... but... what if I do need two values of the same type ?**

Obviously some methods will actually require more than value of the same type, so what gives? In these case, you can artificially introduce subtypes to act as surrogate variables. You can even define subtypes directly into method argument lists.

```
def haveSameFirstName(ClientA < Client, ClientB < Client): Boolean
	ClientA.firstName = ClientB.firstName

def plus(LeftInteger < Integer, RightInteger < Integer): Integer
	...
```

Easy enough. Note that since Jex does not have an assignment operator, it breaks free of C's `==` nonsense (and certainly of Javascript's `===` atrocity) and uses `=` to denote the equality test. Shout-out to all the Pascal lovers out there.

# Value consumption scoping rule

When a type appears both in the argument list and in the return type, *Jex consumes the input value from the scope and replaces it with the output value*. This is better understood with an example:

```
	def femaleDevelopersOver30(List[Person]): List[Person]
		List[Person].filter(_.sex = Female)
		List[Person].filter(_.age > Years(30))
		List[Person].filter(_.jobTitle = JobTitle("developer"))
```

There are four distinct values of type `List[Person]` in the method above, and each call to `filter` consumes and returns a new one. Also notice that Jex stole Scala's syntax for [anonymous functions](http://docs.scala-lang.org/tutorials/tour/anonymous-function-syntax.html).

# Implicit self

When unambiguous and appropriate (i.e. when it enhances readability) Jex allows the omission of the self value in method calls. The idiomatic way of writing `femaleDevelopersOver30` is actually:

```
def femaleDevelopersOver30(List[Person]): List[Person]
	filter(_.sex = Female)
	filter(_.age > Years(30))
	filter(_.jobTitle = JobTitle("developer"))
```

So pretty :heart:

A more idiomatic version of `getWidget` would be:

```
def getWidget(WidgetId, Connection): Widget
    new PreparedStatement("select * from widgets where id = ?")
    setLong(WidgetId.toLong)
    executeQuery()
    ResultSet.next()
    new Widget(ResultSet)
```

# Undordered argument lists

One of the coolest benefits of *type unicity by scope* is that method argument lists are unordered in Jex. Arguments can be passed in any order without issue.
This means you don't need to remember parameter order when learning a new API; cool stuff.

# Unordered currying 

A direct consequence of undordered argument lists is that currying can also be done in any order. None of that Haskell [`flip` nonsense](http://hackage.haskell.org/package/base-4.8.1.0/docs/Prelude.html#v:flip) in Jex!




