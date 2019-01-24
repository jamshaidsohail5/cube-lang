<img align="left" height="100" src="https://upload.wikimedia.org/wikipedia/commons/c/c3/Lambda-letter-lowercase-symbol-Garamond.svg">

# The Cube Programming Language

**Cube** is a modern, high-level language that combines functional and object-oriented programming with type safety. The language's core philosophy is to support efficient code, by enabling developers to write intuitive programs with clear and readable syntax. The first release aims to target the [JVM](https://en.wikipedia.org/wiki/Java_virtual_machine), followed by [LLVM](https://en.wikipedia.org/wiki/LLVM) and [WebAssembly](https://en.wikipedia.org/wiki/WebAssembly).

**Contents**

* [Examples](#examples)
* [Motivation](#motivation)
* [Integrated Language Features](#integrated-language-features)
* [Algorithms and Pseudocode](#algorithms-and-pseudocode)

## Examples

Although Cube isn't publicly available yet as we are still working on the compiler, this repository contains example Cube programs. Another example is the [Cube parser](https://github.com/cube-projects/cube-lang/tree/master/src/cube), also written in Cube. To get involved, email us at [info@cube-lang.org](mailto:info@cube-lang.org).

### Whitespace

Cube is a polygot language, and combines the best features of several programming languages into single programs. Because embedded languages such as SQL use flexible whitespace layout, Cube and does not require new lines or tabs to delimit statements or blocks.

The Cube parser ignores whitespace, and like [Lisp](https://en.wikipedia.org/wiki/Lisp_(programming_language)), a complete program will still parse correctly if formatted onto a single line. Although Cube does have a recommended  coding style, programmers have the flexibility to format code without being restricted by layout.

### Hello world

```lua
print('hola!')
```

### Functions

```lua
function square(x as int) = x * x

print(square(5))
```

### Unit tests

Cube provides a domain-specific language (DSL) for test-driven development. In this example, a `map` operation is used to make a unit test over a list more readable.

```lua
map (1, 2, 3, 4, 5) to square
should be (1, 4, 9, 16, 25)

square(50) should be between 2000 and 3000
```

### Variables

Cube has a strong type system that supports early error-detection, and both objects and functions are first-class citizens in the type system. Cube also supports type inference to improve readability. Variable and function types can be inferred by the compiler if not specified.

```lua
-- mutable
var y = 3
y = 5         -- updated

-- immutable
let x = 'red'
print(x)
x = 'green'   -- won't compile

-- strongly-typed
let z as double = 3.5
```

### Arrays

```lua
-- an array of ints
let x = [1, 2, 3, 4, 5]

-- create a new array
let y = new array[double](50)
print(y.length)
```

### Lists

```lua
let names = ('Alice', 'Bob')
names.add('Eve')

names.length should be 3
```

### Maps

```lua
let food = {
  'Monday' -> 'chicken',
  'Tuesday' -> 'pasta'
}

print(food('Monday'))
```

### Objects

This program defines a temperature class with a Celsius value that can be increased, or converted to Fahrenheit.

```lua
define temperature(celsius as double)
  function fahrenheit = celsius * 9 / 5 + 32

  function increaseTemperature(amount as double)
    if amount <= 0 then error '{amount} is not a valid temperature.'
    else celsius += amount
  end
end
```

### Negative unit testing

```lua
test 'convert celsius to fahrenheit'
  let x = new temperature(celsius = 5)
  x.fahrenheit should be 41
  x.increaseTemperature(-5) should error '-5 is not a valid temperature.'
end
```

### Metadata

A simple [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) endpoint with metadata annotations.

```lua
[path('webapp/hello')]
define restEndpoint

  [get]
  [produces(mediaType = 'text/plain')]
  function hello = 'hello world'

end
```

### SQL over in-memory collections

```sql
let buffer = connection.stats

select avg(time), max(time)
from time in buffer where time > 0
```

### JSON interoperability

```sql
let cities = {
  'Tokyo': {
      'tags': ['Asia', 'Japan']
  },
  'London': {
    'tags': ['Europe', 'UK']
  }
}

-- find tags for Tokyo
let tags =
  select c.tags from c in cities
  where c.key = 'Tokyo'

-- add travel
tags.add('Travel')
tags.length should be 3

-- convert to hashtags
let hashtags =
  select '#' + lowercase(tag)
  from c in cities join tag in c.tags
```

### Map

`map` is a higher-order function that transforms data using a mapping function. In the example below, we first define a function that accepts a single value, and then apply this function to map each number to its square. 

```lua
function square(v as int) = v * v

map [1, 2, 3, 4, 5] to square
```

If a higher-order function is not required, a `select` query can also be used to apply a mapping function directly to a data structure.

```lua
select square(x) from x in [1, 2, 3, 4, 5]
```

### Reduce

As well as `map`, `reduce` operations can also be used to solve data transformation problems. These are higher-order functions that use a combining operation to transform or aggregate data. Cube provides both recursive (*stateless*) and iterative (*stateful*) forms of reduction. 

In purely-functional programming, [reduce](https://en.wikipedia.org/wiki/Fold_(higher-order_function)) is a stateless function that applies a combining operation recursively. A simple example of this is using addition to reduce a list to its sum.

```lua
let a = (1, 2, 3, 4, 5)
let v = reduce a with +

v should be 15
```

Cube also provides a stateful form of reduction. Using intermediate state can be useful when processing consecutive elements of a data set, such as calculating the gap in days between dates in a list, or performing a compounding calculation.

The example code below compounds the positive values in an array, starting with an initial value of 1. In this reduce operation, the elements in the array are combined into a single value by multiplying each element with the result of the previous step.

```lua
let a = [1, 2, 3, -4, 5]

with v = 1
reduce v * e from e in a
where e > 0

v should be 30
```

### Higher-order functions with lambdas

```lua
function benchmark(codeBlock as () -> void) as interval
  let startTime = now
  codeBlock()
  output now - startTime
end
```

### Recursion and type inference

```lua
function factorial(n as int) = if n < 1 then 1 else n * factorial(n - 1)
```

### Loops

```lua
function cluster(data as dataset)
  for i = 1 to i < data.size, j = 1 to i
    let p = measure(data(i, j))
    if distance(p) > 0 then matrix(i, j) = log(p)
  end
end
```

### Iteration

```lua
for each c in connectionList -> print(c.timeout)
```

### Interfaces

Cube supports object-oriented programming, with interfaces, polymorphism and abstract types.

```lua
interface IConnection
  field timeout as long
  function close
end
```

### Set-based invocation

```lua
close each connection in connectionList
```

### Set-based update

```lua
update c in connectionList
set c.timeout = 1000
where c is not null
```

### Generics

```lua
define node[T]
  field value as T
  field children = new list[node[T]]
  function countNodes = sum(c.countNodes) from c in children
end
```

### Type constraints

```lua
function totalArea[T >= shape](shapes as list[T])
  output sum(s.area) from s in shapes
end
```

### Pattern matching

```lua
function describe(p as point)
  let name =
    match p
      when root or leaf then 'already visited'
      when (x, y) where x > 50 then 'larger than x-bound'
      when p is graphPoint or treePoint then p.name
      else 'not classified'
    end

  output 'Point is {name}.'
end
```

### Concurrent programming with agents

Parallel computing algorithms using low-level threads are challenging to implement and difficult to test. For concurrent and distributed programming, Cube provides an intuitive agent-based framework to simplify parallel computing tasks.

```lua
define hello as agent
  function receive 
    match when 'hello' then print('hi!')
    else print('sorry?')
  end
end 

test 'hello agent'
  let group = new agentGroup
  let agent = group.add[hello]

  send 'hello' to agent
  send 'hola!' to agent
  wait until group.queue is empty
end
```

### Exception handling

```lua
try
  loadFile('test/data')
catch
  when e is DataException then log.warn(e)
  else log.error(e)
finally
  log.info('completed')
end
```

## Motivation

### Pseudocode

Cube is primarily inspired by [pseudocode](https://www.youtube.com/watch?v=gcQMBK53UjI), a fundamental concept in Computer Science for both education and software design, that is used to explain how algorithms work, independently of any specific programming language.

Most computer programs describe algorithms, which are sets of instructions that are executed to provide the solution to a specific problem. In software development, clear programs that are simple to modify and test are highly valued by the programming community, compared to more complex programs that may solve the same problem.

What makes pseudocode so powerful is the idea that any computer program, even a highly complex one, can be described using a combination of simple mathematical notation and basic operations. Programs written in pseudocode are easier to understand because they use conventional words to describe instructions (such as `if`, `while`, `do` and `end`) and avoid complex grammar or unfamiliar symbols.

Compared to programming languages that have more complex syntax, this is easier for humans to read, but more challenging for machines to process. Cube shares its readable coding style with other similarly-inspired languages, including [Basic](https://en.wikipedia.org/wiki/BASIC), [Lua](https://en.wikipedia.org/wiki/Lua_(programming_language)), [Ruby](https://github.com/ThibaultJanBeyer/cheatsheets/blob/master/Ruby-Cheatsheet.md) and [SQL](https://en.wikipedia.org/wiki/Select_(SQL)).

### Dependency grammar

A second source of inspiration is [dependency grammar](https://en.wikipedia.org/wiki/Dependency_grammar), a framework used in Computational Linguistics research for a wide variety of natural language processing tasks, such as translating one language into another. Cube uses an extensible form of dependency grammar that allows programmers to easily create new keywords or language definitions.

This simplifies the creation of domain specific languages, and enables [polyglot programming](https://www.graalvm.org/docs/reference-manual/polyglot), which combines the best features of different languages (such as functional programming and SQL over collections) into easily readable programs.

## Integrated Language Features

Common programming tasks often involve searching or updating data structures such as sets, lists and graphs. However, many programming languages do not include high-level support for both functional programming and the ability to easily modify set-based data structures. To solve these problems, programmers may have to use software libraries outside of their language, or write longer code using loops or lower-level iteration.

### Example

Cube provides language support for frequently occurring operations over basic data structures. For an example of how integrated language features help with code readability, consider trying to write a function that implements a simple mathematical algorithm for vector normalization.

In **Java**, we can use loops to write a function to normalise an array of probabilities in-place.

```java
public static void normalizeLogArray(double[] array) {
  double max = Double.NEGATIVE_INFINITY;
  for (double d : array) {
    if (d > max) max = d;
  }

  double sumExp = 0;
  for (double d : array) {
    sumExp += Math.exp(d - max);
  }
  double logSumExp = max + Math.log(sumExp);

  if (Double.isInfinite(logExpSum)) {
    for (int i = 0; i < array.length; i++) {
      array[i] = 1.0 / array.length;
    }
  } else {
    for (int i = 0; i < array.length; i++) {
      array[i] = Math.exp(array[i] - logSumExp);
    }
  }
}
```

In **Cube**, the same function can be written in a more readable way, using integrated language support for set-based data updates.

```lua
function normalize(d as array[double])
  let v = max(d) + log(sum(exp(a - max(d)))) from a in d
  update a in d
  set a = if v is infinite then 1.0 / d.length else exp(a - v)
end
```

## Algorithms and Pseudocode

Joesph Dugan's *Introduction to Algorithms* provides a great basis for understanding how pseudocode is used ([video](https://www.youtube.com/watch?v=gcQMBK53UjI)).

[![pseudocode](https://img.youtube.com/vi/gcQMBK53UjI/0.jpg)](https://www.youtube.com/watch?v=gcQMBK53UjI "pseudocode")
