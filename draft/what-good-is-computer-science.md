[brutal]: #title "What good is Computer Science"
[brutal]: #author "David Jones"
[brutal]: #date

This is very much a personal reflection,
inspired by http://blog.alicegoldfuss.com/foot-candles/

Unlike Alice I took a fairly traditional route
into Computer Science and programming roles:
I did an undergraduate maths degree (3 years) and a
post-graduate diploma in computer science (1 year).

The diploma was marketed as a conversion course,
and was a sort-of rapid pace version of the 3-year undergraduate
course.

From memory we did:

ML, C, Modula-3, Prolog, PostScript, MIPS DEC assembly;

Regular Languages and Automota, Complexity, Computability;

Data Structures & Algorithms;
Numerical Methods;
Operating Systems;

Databases, Graphics, Artificial Intelligence;

Networking, Computer Architecture, Hardware Organisation;

Ethics, Formal Verification;

There was also some sort of business / industry track
which was often invited speakers talking about their business
or how to start one.

Lots of these courses came in "introduction" and "further"
versions.

If you want to consider yourself as having a Computer Science
education, what should it look like?

The short version of this article is that I think most of those
courses are relevant and necessary, but for an absolutely
minimal version they could all be a shorter.

## Programming Languages

I think you need to be exposed to
more than one computer programming language.
I think ML, which I admit has weaknesses as well as strengths,
is a good choice for this sort of course because:
a) not likely to be encountered before,
so will immediately become an additional string to your bow;
and, b) functional programming is the kind of intellectual
pursuit that "having an education" is supposed to imply.

Knowing more than one programming languages can enable you to
compare and contrast languages,
and it's really the minimum that you need
in order to explore the notion that
different languages might be best for different things.

## Theory

Regular Languages and Automata is a good place to introduce theory.
Regular Expressions are common
(they're used in `grep`, `ed`, a bunch of editors,
and most reasonable modern languages seem to have some sort of
support for them: `import re` in Python),
so I think that's a good place to start.
It's good to get the idea that Regular Expressions aren't just a
thing that lots of programming languages have, there is some
sort of formalism to it.

I also like that Regular Languages can be used to introduce the
idea of limits to power.
An example from Regular Languages is using the Pumping Lemma
to show that a regular expression cannot be used to balance
parentheses.

Complexity models how much space and time computational processes
need as they grow.
It's the O(n²) time and O(n log n) space stuff.
This is good and interesting theory,
but really I like it because
the theory so often has practical applications.
The practice meets the theory often enough that it is
practically useful to know
the difference between an O(n) and an O(n²) algorithm.
And if that's all you know, that's fine.

Computability — as in Turing Machines, Lambda Calculus, Minksy
Register Machines — is a fun topic, but all you really need to is
that there is no theoretically complete way to tell if your
Docker container is just slow or if it is stuck and never going
to start-up properly.
All very well in theory, but in practice,
you just kill it after 30 seconds anyway.

# Data Structures and Algorithms

I admit to having a certain fondness for this.
You do need to know what a linked list is.
You do not need to be able to reverse a binary tree on a
whiteboard.

It is useful to learn about one or two interesting and cool data
structure or algorithms, not particularly because you'll
definitely need Dijkstra's all-pairs shortest path algorithm in
real life, but to give you the idea that there might be
situations in your problem solving career where you need to go:
"aha, perhaps I need An Algorithm", and you can go looking for
one in the literature (don't try to make one up).
