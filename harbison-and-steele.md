# C A Reference Manual

Prompted by conversations with samathy and matthew hughes
I am writing a review of a book about C.

Harbison and Steele's «C A Reference Manual».
The book is a classic and has been revised many times.
In this case I'm reviewing the fifth edition, published in 2002
(mostly because that's what was available
in The University of Sheffield library).

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
