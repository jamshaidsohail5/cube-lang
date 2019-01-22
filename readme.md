<img align="left" height="100" src="https://upload.wikimedia.org/wikipedia/commons/c/c3/Lambda-letter-lowercase-symbol-Garamond.svg">

# The Cube Programming Language

**Cube** is a modern, high-level language that combines functional and object-oriented programming. The language's core philosophy is to support efficient code, by enabling developers to write short intuitive programs with clear and readable syntax.

## Inspiration

Cube is primarily inspired by [pseudocode](https://www.youtube.com/watch?v=gcQMBK53UjI), a fundamental concept in Computer Science for both education and software design, that is used to explain how algorithms work, independently of any specific programming language.

Most computer programs describe algorithms, which are sets of instructions that are executed to provide the solution to a specific problem. In software development, clear programs that are simple to modify and test are highly valued by the programming community, compared to more complex programs that may solve the same problem.

What makes pseudocode so powerful is the idea that any computer program, even a highly complex one, can be described using a combination of simple mathematical notation and basic operations. Programs written in pseudocode are easier to understand because they use conventional words to describe instructions (such as `if`, `while`, `do` and `end`) and avoid complex grammar or unfamiliar symbols.

Compared to programming languages that have more complex syntax, this is easier for humans to read, but more challenging for machines to process. Cube shares its readable coding style with other similarly-inspired languages, including [Basic](https://en.wikipedia.org/wiki/BASIC), [Lua](https://en.wikipedia.org/wiki/Lua_(programming_language)), [Ruby](https://github.com/ThibaultJanBeyer/cheatsheets/blob/master/Ruby-Cheatsheet.md) and [SQL](https://en.wikipedia.org/wiki/Select_(SQL)).

A second source of inspiration is [dependency grammar](https://en.wikipedia.org/wiki/Dependency_grammar), a framework used in Computational Linguistics research for a wide variety of natural language processing tasks, such as translating one language into another. Cube uses an extensible form of dependency grammar that allows programmers to easily create new keywords or language definitions.

This simplifies the creation of domain specific languages, and enables [polyglot programming](https://www.graalvm.org/docs/reference-manual/polyglot), which combines the best features of different languages (such as functional programming and SQL over collections) into easily readable programs.

## Integrated Language Features

Common programming tasks often involve searching or updating data structures such as sets, lists and graphs. However, many programming languages do not include high-level support for both functional programing and the ability to easily modify set-based data structures. To solve these problems, programmers may have to use software libraries outside of their language, or write longer code using loops or lower-level iteration.

Cube provides language support for frequently occurring operations over basic data structures. For an example of how integrated language features help with code readability, consider trying to write a function that implements a simple mathematical algorithm for vector normalization.

In **Java**, we can use loops to write a function to normalize an array of probabilities in-place.

```java
public static void normalizeLogArray(double[] array) {
  double max = Double.NEGATIVE_INFINITY;
  for (double d : array) {
    if (d > max) max = d;
  }

  double expSum = 0;
  for (double d: array) {
    expSum += Math.exp(d - max);
  }
  double logExpSum = max + Math.log(expSum);

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

[![pseudocode](https://img.youtube.com/vi/gcQMBK53UjI/0.jpg)](https://www.youtube.com/watch?v=gcQMBK53UjI "pseudocode")

*Introduction to Algorithms*, by Joseph Dugan ([video](https://www.youtube.com/watch?v=gcQMBK53UjI)).
