# Validating Pseudocode / Debugging with the Triangle Process

## Learning Goals

- Identify a process to validate pseudocode

## Introduction

Hopefully upon your review of the pseudocode you found the bug in it. Did you
see it? Don't worry if it didn't leap out for you. Our process will help you
spot the issue. Let's analyze our process for validating our pseudocode, the
6<sup>th</sup> step in the Flatiron Programming Process.

## Identify a Process to Validate Pseudocode

Here's the pseudocode again:

```text
Procedure DivideBaguetteEvenly(baguette, n):
  baguette_length = measure(baguette)
  even_length = baguette_length / n
  collection = []

  while baguette_length > even_length:
    piece, rest = cut_bread(baguette, even_length)
    collection.add(piece)

    baguette = rest
    baguette_length = measure(baguette)

  even_pieces = collection
  return even_pieces
```

We're going to walk through our pseudocode using a state table.

### Introducing State Tables

State tables are tables that keep track of the inputs, outputs, local
variables, and conditions in a bounded or fenced-off "bloc" of code or
pseudocode.

Because the blocs are fenced off, sometimes we have to provide input that would
be available if the bloc were part of a larger program. It's OK to do this
because we'll assume that other code does its job properly.

So for `DivideBaguetteEvenly`, we can create the following table:

|Variable|State|State|State|State|State|State|State|State|State|
|--------|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|Input: `baguette`|||||||||||
|Input: _n_|||||||||||
|Output: even_pieces|||||||||||
|Local: `baguette_length`|||||||||||
|Local: `even_length`|||||||||||
|Local: `collection`|||||||||||
|Cond: `baguette_length > even_length`|||||||||
|Local: `piece`|||||||||||
|Local: `rest`|||||||||||

We'll "plug in" some data for `DivideBaguetteEvenly` use and then we'll step
through, line by line and update the "state" or the "values" of the variables
as we work through the _Procedure_'s function. We'll check our output against our
expectations and make sure our logic is right.

### Filling in the State Table

Lets play through our _Procedure_ like so:

`DivideBaguetteEvenly( 60cm baguette, 3 people)`

We would expect that:

* Each person gets a 20cm length piece.
* The baguette is length 0 at the end (we used the whole thing)

Work through the code, filling out the table:

|Variable|State|State|State|State|State|State|State|State|State|
|--------|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|Input: `baguette`|60cm baguette|40cm baguette|20cm baguette||||||||
|Input: _n_|3||||||||||
|Output: even_pieces|[20cm piece,20cm piece]||||||||||
|Local: `baguette_length`|60|40|20||||||||
|Local: `even_length`|20||||||||||
|Local: `collection`|[]|[20cm piece,20cm piece]|||||||||
|Cond: `baguette_length > even_length`|T|T|**F**|
|Local: `piece`|20cm piece|20cm piece|||||||||
|Local: `rest`|40cm baguette|20cm baguette|||||||||

**Output**: 2 pieces. This is not what we predicted and is also incorrect.

Try testing the table with: `DivideBaguetteEvenly( 60cm baguette, 1 people)`.
That's certainly wrong output as well.

### Introducing the Triangle Process of Debugging

Whenever we debug we should always examine a single code "bloc" (a procedure, a
function, a method, etc.). We don't want to confuse ourselves.

Within a bloc we can find out if it's the source of the problem by verifying
our inputs, outputs, and the implementation. These three topics are the
vertices of the "triangle" method.

_Image here..._

For a given bloc ask:

* Were its inputs correct?
* Were its outputs correct?
* Was the implementation correct

For our _Procedure_:

* Our inputs were correct.
* Our output was incorrect
* What went wrong in our implementation?

Let's make some hypotheses about what a "working" method would have for a 60cm
baguette divided among 3 people:

* 3 pieces
* 0cm baguette at the end

The state table tells us clearly:

* 2 pieces were returned
* Our baguette was 20cm at the end.

Our bug is we should have used `>=`. Update your pseudocode and test it with a
table to verify that the fix works.

## Conclusion

Validating our pseudocode before implementing it as code is a vital skill to
develop. When engineers are seen "white boarding" they are playing through
pseudocode. The ability to craft focused pseudocode that solves problems is the
essential focus of the "whiteboard" interview used in engineering teams.

The ability to demonstrate, with state tables, that your pseudocode works is a
strong signal to other developers of your ability to think clearly about
programming.

The final step (and, to be honest, it's also expected in whiteboard interviews)
is to take these ideas and turn them into code in a particular language. Let's
take our `baguette_length` dividing _Procedure_ and turn it into Ruby code.
