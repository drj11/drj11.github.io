# C A Reference Manual

Prompted by conversations with samathy and matthew hughes
I am writing a review of a book about C.

Harbison and Steele's «C A Reference Manual».
The book is a classic and has been revised many times.
In this case I'm reviewing the fifth edition, published in 2002
(this appears to be the most recent edition,
but it is also whatmostly because that's what was available
The University of Sheffield library stocks).

Because this review is bound to say lots of negative and
nitpicky things
I'm going to open by saying that
this is a good book and I recommend it.
It is not a good book for beginners;
the reference manual style leaves little room for
gently easing you into difficult topics
or expanding upon subtle ones.
But if you can write a program in C then
this book would make an excellent choice.

Good books on C are rare, and this is a good book.
The authors know they're stuff,
and while they are not always as clear as possible on some topics,
they are assiduous and correct.

My chief criticisms of the book are:

- the order;
- the examples;
- too much focus on pre Standard C
- not enough focus on C99: topics: restrict and inttypes.h and
  designated initialisers.

The order of the book roughly follows the order of the C standard.

Practically, this means
the most abstruse and technical parts of C are covered first.
And I mean really obscure.
Like how preprocessor token concatenation used to work
before ANSI came along and standardised it.
This particular topic, of token pasting, allows me to briefly
preview two of other gripes: The examples chosen to illustrate
various language features; and, too much focus on pre-Standard C.

For token pasting,
the authors have an example of how it used to be done,
before Standard C.
Maybe in 1990 it would be useful to have a compare-and-contrast
between Traditional C and Standard C,
but this edition was published in 2002,
12 years after the first Standard for the C programming language
was published.
The example in this case is an example of how not to do it,
and it just clutters the book.

This ordering problem manifests most obviously in the fact that
`main()`, which is the only function that a (hosted) C program
_must_ provide and which defines the entry point to a C program,
doesn't appear until page 303.
Which is well after halfway in this 533 page book
(generously counting the appendices and the indexes).
For comparison, it's page 6 in Kernighan and Ritchie's even more
venerable classic «The C Programming Language».

Whilst reading I made a note that page 259,
which is the start of Chapter 8, «Statements»,
"would be a great place to start".
And it would be.

It's a reference work, I get that.
But even so, I think the order of the material is poor.
At times it's brutally rude (for example, to see an explanation
of the rules of pointer type compatibility before you see the
syntax for pointer expressions).

## Setting a bad example

As hinted at previously,
many of the examples are examples of how not to do things
(in that they show techniques that are either obscure
or invoke undefined behaviour),
or they are examples that apply to Traditional C
(either "how we did it in the old days" or
"why this traditional technique no longer works").

An example of this, chosen more or less at random,
is on page 212 after the discussion of array notation
there is a program that flattens, by casting,
a 2-dimensional array (`double matrix[SIZE][SIZE]`) to
a pointer (`(double *)matrix`) so that they can show
their "trick" of using a single loop.
It's bad code.
It's an obscure and unclear trick introduced because they were
too weak to avoid the siren call of premature performance
optimisation.

(the bizarre thing is at some level the authors know this,
they frequently signal the code with
"avoid in most situations" or
"you will rarely have good reason to do this")

If you have a copy of the book,
the exemplar example is on page 276 at the end of section 8.7 on
the switch statement.
It is too horrible to repeat here.

# Ancient C

They call it "Traditional C", I call it ancient.
It's the C we programmed before Standard C was published.
In 1990.
Actually a lot of it predates even that period.
Even before Standard C was published
a lot of compiler vendors were moving towards a C
that was more like Standard C
(partly because the vendors were basing their
compiler implementations on draft standards,
but also because the standard was standardising existing
common behavour of implementations).
So there was a light before the dawn.
(admittedly, there was also a shadow after dawn,
vendors dragging their heels and support for older systems
meant that we were still discussing in 1995
if we should write C for "pre ANSI" compilers).

In 2002 we should have been proposing that
programmers start using `restrict` a bit more,
not persuading them to use prototype function declarations.
Protoype function declarations are ones that include the type of
the formal arguments inside the parentheses
(for example `int floor(float x)` is a prototype,
`int floor(x) float x ; {` is the old-style "K & R" style
definition.
The old-style definitions were old in 1990
(since many compilers had already been supporting
the Standard C prototype syntax), never mind 2002.

So I don't really want to see (on page 286) "whether to use
prototypes in C programs is a tricky portability issue".
It's not.
Use prototypes.

There is a quite a lot of discussion of Traditional C in the book,
and I'd rather it were just removed.
You can always refer readers to earlier editions of the book if
they really have to maintain code that uses Traditional C
features.

# C1999

On the flip side the coverage of
the language changes
introduced with the 1999 version of the C Standard are
most welcome.
But.
But there isn't enough discussion of them.

I've already mentioned `restrict`
and it's quite a major new feature ("new" as in "new in 1999")
of the language.
It grants to C what every Fortran programmer already had,
the ability to declare that
the array a function writes the result into
is separate from the array the function reads from.
It's perfect for large matrix multiplies.

But `restrict` is hardly discussed in the book
and appears only once in the index.
This is a real shame, because we desparately need
material to use to train all the C programmers on
how to use `restrict` productively.
