# The Beauty of ZEROBRANCH

Forth has a number of flow control constructs:
`IF`, `WHILE`, and `UNTIL`.

## What IF..?

`IF` is similar to the `if` construct from many languages:

    IF
        code when true
        ...
    ELSE
        code when false
        ...
    THEN

`IF` marks the beginning of the `IF` construct,
and `THEN` marks the end
(note that the position of `THEN` differs from Basic,
where it separates the condition from the "when true" code).
In Forth, `IF` expects the condition that is to be tested
to be already evaluated with the result left on the stack
(again, contrast with Basic where
the code for the condition to be tested is
between `if` and `then`).

Forth implements a convention of treating 0 (all bits zero)
as false,
and any other non-zero value as true.
`IF` pops the value at the Top of Stack and tests it;
if true, the code between `IF` and `ELSE` runs,
and then execution continues after `THEN`;
if false, the code between `ELSE` and `THEN` runs,
and then execution continues after `THEN`.

When translated into threaded code,
the action of `IF` is implemented by a conditional branch:
either execution continues at the next instruction,
or execution branches to the instruction following `ELSE`.
This behaviour is implemented in the Forth primitive
that is called `ZEROBRANCH` in my implementation
(I suspect I have some dim deep memory of
this primitive being called `0BRANCH` in
some version of Forth that I investigated as a child,
possible Sinclair ZX81 FORTH).

`ZEROBRANCH` tests the Top of Stack and branches if it is 0.
The branch destination is stored in the following cell.
That's sufficient to implement the `IF` part of
`IF ... ELSE ... THEN`.
The `ELSE` part is even simpler,
it is an unconditional branch
to the instructions following `THEN`.
The `THEN` part doesn't result in any compiled code,
though both `ELSE` and `THEN` have
additional work to do at compile time.

The `ELSE` part is optional,
so let's consider compilation of the slightly simpler
`IF ... THEN` constructs.
We'll add `ELSE` later for extra fun and profit.

The branch for `IF` is a forward branch,
this means that when the compiler is compiling `IF`
it doesn't know what the destination of the branch should be.
The compiler does know that it will
have to compile `ZEROBRANCH` followed by a branch destination.
So it compiles `ZEROBRANCH` followed by a dummy cell.
It leaves a marker (on the stack as it happens)
so that it can find the dummy cell later.
The marker is the address of the dummy cell.

Later the compiler sees the `THEN`, and
it now knows the destination of the previous branch.
The destination is the current compilation point.
A branch to the current compilation point
is written into the cell identified by the marker
(which is found at the Top of Stack, and is removed).
This overwrites the dummy value previously found in this cell.

### What ELSE?

We can add `ELSE` to this scheme quite easily.
When the compiler compiles `ELSE` it needs to do two things:
1) fill in the dummy cell left by `IF`; and,
2) leave a dummy cell and a marker that `THEN` will fill in later.
There are some fiddly details to get right
because `ELSE` has to juggle the previous marker left by `IF`
and its own freshly created marker.
But fundamentally compiling `ELSE` fills in the marker left by `IF`,
and replaces it with one of its own
that is later filled in by `THEN`.

Note that in adding `ELSE` to this scheme,
we didn't need to modify the behaviour of `THEN`.
`THEN` simply finds a marker on the Top of Stack and fills it in.
It does know or care whether the marker is
left by a previous `IF` or a previous `ELSE`.

## UNTIL

A loop is a branch that goes backwards.
If the branch is conditional in some way then
we have a way to finish the looping and carry on.
Forth's simplest looping is `BEGIN ... UNTIL`
and is based around a single backwards branch.

    BEGIN
        body of loop
        ...
        including code that ...
        leaves a condition to be tested on Top of Stack
    UNTIL

`BEGIN` simply marks the beginning of the loop.
`UNTIL` behaves very like `IF`.
It removes the Top of Stack and tests it.
If the value is 0 (false) then we go round the loop again;
execution branches to the code immediately following `BEGIN`.
If the value is non zero (true) then we exit the loop;
execution continues with the code following `UNTIL`.

`UNTIL` is, like `IF`, implemented by `ZEROBRANCH`.
Compilation proceeds similarly to `IF ... THEN`.
It's a little bit simpler than compiling `IF`.
Compilation of `BEGIN` involves pushing a marker onto the stack.
This marker is the address of the current compilation point.
Compilation of `UNTIL` removes the marker
and uses it to compute the destination for
the `ZEROBRANCH` primitive that it compiles.

## BEGIN WHILE REPEAT

The next looping construct I discuss is
_one of my favourite looping constructs_
of all time, of any programming language.

    BEGIN
        first part of loop
        ...
        including code that ...
        leaves a condition to be tested on Top of Stack
    WHILE
        second part of loop
        ...
    REPEAT

The thing that _so insanely great_ about this loop is
that the test is in the middle of the loop.
The code in the first part of the loop is executed,
then a condition is left on Top of Stack.
`WHILE` tests this condition and
if it is true (non-zero) it continues execution of the second
part of the loop;
if it is false (zero) it terminates the loop by branching
to the code just following `REPEAT`.
`REPEAT` (if we reach it by executing the second part of the loop)
branches (unconditionally) back to the code following `BEGIN`.

Note that the "first part of loop" could be empty,
in which case the test is effectively at the beginning of the
loop.
Similarly, the "second part of loop" could be empty,
in which case the test is effectively at the end of the loop.
`BEGIN ... WHILE ... REPEAT`
does the work of both `while() { ... }` and `do { ... } while();`
from C.
All loop styles in one well dressed looping construct. Smart!

Note that the `WHILE` test is inverted compared to `UNTIL`.
`WHILE` continues looping when the Top of Stack is true;
`UNTIL` continues looping when the Top of Stack is false.
This is deliberately, and, cunningly, allows
`WHILE` to also be implemented using... `ZEROBRANCH`!

`WHILE` compiles to a `ZEROBRANCH` that (conditionally) branches
to the code following `REPEAT`.
This is a forward branch, so, like `IF`, `WHILE` must compile a
dummy `ZEROBRANCH` and leave a marker for `REPEAT` to find.

`REPEAT` now expects to find two markers on the stack,
one from `BEGIN` which is used to find the beginning of the loop,
and one from `WHILE` which is used to compute the forward branch
and fill in the dummy value.

Note that `BEGIN` is shared by two different loops:
`BEGIN ...  UNTIL` and `BEGIN ... WHILE ... REPEAT`.
In either case,
the behaviour of `BEGIN` is to leave a marker on the stack;
it doesn't need to know if it is being used in a
`BEGIN ...  UNTIL` loop or a `BEGIN ... WHILE ... REPEAT` loop.

## Safety

What happens if you make a mistake and try something like:

    IF
        4 *
    THEN
        8 *
    THEN

?

All hell breaks lose and
the Forth system probably loses control of itself.
In Forth systems, the programmer, not the system,
is responsible for ensuring that
the programmer does not violate any of the system safety rules.

One could add a measure of safety checking to the system.
For example, when compiling `IF`
the address marker that is left on the stack
could be followed by an `IF` token.
The compilation of `ELSE` or `THEN` could check that
it finds an expected `IF` token on the stack.
If it does not find the expected token,
it could abort safely.

Similar the other looping primitives could be enhanced with
more safety checks.
`:` and `;` could cooperate to ensure that the stack level
on entry to compilation equalled
the stack level on exit.

These checks come at an increase in complexity, and
an increase in code size.
Traditionally Forth systems performed only a minimal amount
of safety checking,
preferring lean systems that give the programmer control,
but require the programmer to assume a lot of responsibility.

As of writing my Forth implementation has very little safety checking,
but I will probably add more in future.

## ZEROBRANCH

I love the parsimony of the `ZEROBRANCH` primitive.
It's no coincidence that a single primitive
allows 3 different control structures to be implemented
fairly straightforwardly.

In designing this part of Forth Charles Moore shows great skill
in marrying reasonable language semantics with
efficient possible implementations.

This philosophy is in contrast to Lisp which is
"optimal language semantics that are implementable somehow";
and C which is
"any language semantics with simplest possible implementation".
