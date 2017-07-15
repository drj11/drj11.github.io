# The Philosophy of Forth

Forth is a family of programming languages
all sharing a similar theme, form, and philosophy.
They grew out of Chuck Moore's original Forth programming system,
created in the 1970s.

I'm creating a Forth system, so some of the philosophy is
rubbing off on me.
As I see it, the philosophy of Forth is:

- Implicit notation;
- The dictionary: a threaded list of words that is extensible;
- Chumminess with the implementation;
- Uniformity.

The use of "chumminess" here is a reference to Ritchie declaring
that some particular C struct hack was
"unwarranted chumminess with the C implementation"](http://c-faq.com/struct/structhack.html)

## Implicit Notation

I'm covering two points here with "Implicit Notation":
- the stack;
- the (lack of) syntax.

If you've heard anything about Forth,
you've probably heard that it is a "stack based language".
It's the thing that makes it different from other languages.

The second point is syntax.
There is a general lack of it.
And I don't just mean the syntax is small,
it's almost non-existant.

Let's do these separately.

### The Stack

The stack is data structure that holds a varying number of items.
Each item is stored in a _cell_ (ANSI Forth terminology),
and a cell can store an integer (64-bit in my implementation).
The stack has a most accessible part, call the _Top of Stack_
and a least accessible part (all the cells below Top of Stack,
all the way down to the bottom).
Items are added onto the stack; the newest item becomes the Top
of Stack, and the previous Top of Stack now sits below it.
When an item is removed
(generally the Top of Stack or one near it), it is forgotten.

Traditionally the act of putting a new item on the stack is
called _pushing_,
and the act of removing the top item is
called _popping_ (_push_ and _pop_ respectively).
In a typical Forth program,
items get pushed onto and popped from the stack all the time.
But curiously, the terms `push` and `pop`
don't appear in the Forth language, but
you're bound to see them in discussion.

The stack allows operations in Forth to have implicit inputs:
they find their inputs on the stack.
Outputs are implicit too: Outputs are pushed onto the stack.
Generally operations in Forth work by removing some items from
the stack, and placing a result on the Top of Stack.

For example, `+` is the plus operator.
When executed it pops the top two items from the stack,
performs an integer addition,
and pushes the result onto the stack,
making it the new Top of Stack.

Numbers, when they appear in a program,
push themselves onto the stack.
So to add two numbers together:

```
2 3 +
```

To add three numbers together, you can use either

```
1 2 3 + +
```

or

```
1 2 + 3 +
```

They add the same three numbers up in a different order.
Of course with integer addition we get the same answer,
but that might not be true of other operations.

### Swap, Drop, Over

There are all sorts of cunning stack reordering operations
which are useful all the time.
`SWAP` swaps the order of the top two items, effectively it
removes the item just below Top of Stack and pushes it back onto
the stack.
`DROP` pops the Top of Stack and forgets it.
`OVER` takes a copy of the item just below Top of Stack and
pushes that copy onto the stack.
And so on and so forth.

For more on this, go and seek "Reverse Polish Notation".

This is not the only way to have implicit operators,
but the stack is very flexible and powerful.
And time honoured.

The implicit nature of the operators leads to very terse code.
Functions don't need to say where their inputs come from,
or how many inputs they take, or how many results they push.

It also does your head in a bit.

### Syntax

The syntax is so implicit that its
almost non-existent.
Consider the definition of this new word:

    : select
        IF
            SWAP
        THEN
        DROP
    ;

(this word selects either the 2nd or the 3rd item on the stack,
according to the value of the Top of Stack)

`:` (pronounced "colon") introduces the definition of a new word.
In this case the new word's name is `select`.
The definition consists of a sequence of words up to the
terminating `;` (pronounced semicolon), which is itself a word.

`IF` works in a slightly reverse polish way.
It pops the Top of Stack and if it is 0
(a representation of false)
the words between `IF` and `THEN` are skipped
and control then resumes after `THEN`.
If the Top of Stack is any non-0 value
(the canonical value for true is "binary all 1s",
but in this case any non-0 value will do)
then execution proceeds in the normal way, with the words
immediately following `IF`.
In this case executing a `SWAP`.
Nothing happens when we get to `THEN`,
we ignore the `THEN`, then carry on with the following words.

So the action of `select` is to remove the Top of Stack and
either drop the new Top of Stack (if the item removed was 0), or
swap before dropping which removes the new 2nd item.
Pictorially:

    ( a b 0 -- a )
    ( a b nz -- b )

In this notation `( ... )` represents execution of the word `select`.
The part before `--` represents the (top portion of the) stack
before execution;
the part after `--` represents the (top portion of the) stack
after execution.
In both cases the most accessible (Top of Stack) part of the
stack is on the right.

This is a fairly standard notation in Forth.
And extremly useful.

### Nothing is special

But that's just a distraction from the point I want to make.
Which is, it looks like `IF` and `THEN` and `;` are special,
and they're understood by the compiler.
But that's really not the case.
Even `:` isn't particularly special.

In Forth (spiritually, traditionally, and in my implemention)
a compiled word is a sequence of cells,
and each cell represents another word to execute.
The final cell in a sequence is a word
(called `EXIT` in my implementation because
the 1983 standard strongly suggests that name)
that transfers control to the calling word.

### Interpreter mode

The normal mode of Forth (interpreter mode),
is to scan a word from the input,
convert it from a string
to its compilation address which represents its compiled form,
and execute it.
Numbers, like `128`, are handled slightly specially in that they
deposit themselves on the stack.

To compile a new word, which is what `:` does,
we change the mode so that instead of executing each word,
we compile its compilation address into a fresh list (a list
associated with the the newly named word,
`select` in the above example).
The way numbers are handled also changes slightly,
instead of leaving them on the stack,
they are compiled into
a sequence of words that will later
leave the same number on the stack.

### Stop compiling, IMMEDIATELY

To stop compilation, which is part of what `;` does,
there is one further additional wrinkle.
Some words are marked as Immediate
(the Forth word `IMMEDIATE` does this).
`;` is marked as Immediate.
Forth will execute an Immediate word
regardless of which mode it is in.

That means that `;`, because it is mark as Immediate,
can have an effect on the compilation process.
In particular, `;` arranges for the word `EXIT` to be
compiled onto the end of the current word,
and it also "exits the compiler" by
changing the mode back to the interpreter state.

This means that the compiler does not need to
have special knowledge of `;`.
The Immediate flag is all we need to finish compilation.
Without the Immediate mechanism the compiler would
need special knowledge of `;`,
or some other way to terminate compilation.

### Other uses of IMMEDIATE

`IF` is also an immediate word.
It compiles to a conditional branch,
a word (called `ZEROBRANCH` in my implementation) that
removes the Top of Stack and tests it,
if it 0 (false) then it arranges that the next word
to be executed is the one following `THEN`.
It doesn't know where the word following `THEN` is yet,
so it leaves a forward reference token on the stack.

`THEN` is also an immediate word.
`THEN` expects to find on the stack a forward reference token
left by `IF`.
It removes this token and
compiles into the location implied by the token,
a branch that will
cause the `IF` to branch to the word immediately following `THEN`.

### Syntax implied by semantics

Notice that the compiler did not have to get involved in
making the `IF` `THEN` "syntax" work.
The "syntax" of `IF` and `THEN` is entirely implicit
in the definitions of those words
and their behaviour when executed as Immediate words.

There are control structures for loops
(`DO` `LOOP` and `BEGIN` `WHILE` `REPEAT`),
and they work in a similar way.
The compiler has no special knowledge of them,
they execute as Immediate words
in a similar way to `IF` and `THEN`.
Each one deposits enough information on the stack
so that the next one finds it and patches up a branch.

The structures are not explicit
(as in, specified by a grammar),
but implicit in the behaviour of the individual words.
