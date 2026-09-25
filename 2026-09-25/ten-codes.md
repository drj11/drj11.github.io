[brutal]: #date "2026-09-25"
[brutal]: #author "drj"
[brutal]: #title "10 Lines of Code that Changed My World"

Roel Nieskins prompts us with his
[10 Lines of Code that Changed My World](https://pixelambacht.nl/2026/ten-lines-of-code/).

I responded on Fediverse with a bare post with just my 10.
Here they're expanded with commentary.

## WHSMITH

Just like Roel my first entrant is the silly little BASIC program
you can type into almost any computer of the time (early 1980s)
in WHSmith.

    10 PRINT "DRJ IS SKILL"
    20 GOTO 10

And, i don't know if i ever typed exactly this program in, or
just similar ones.  But this is the form in which this short program
has crystallised.  An endless loop declaring one boy's power over
the humble computer.  Except that on ZX81 it stops when it gets to
the bottom of the screen.

## Atari ST

Fast forward a few years when i saved up to buy an Atari ST, and
started dabbling in C.  Probably with a copy of Lattice C bootlegged
in the sixth form common room.

I remember finding this, or similar, when looking at the assembly
output of the C compiler:

    LEA (A1, A1.W*2), A1

What does it do? Well, `LEA` is Load Effective Address, for computing
addresses.  I admit at the time i was a little mystified as to why
one would need to compute an address rather than just _use_ one and
load from it (the reason is pointer arithmetic in C by the way).

I tracked it down to a `* 3` in the C code.  Although you're supposed
to put addresses in 68000 A registers, that's just like a guideline,
and of course you can put any data you like in there.  This
`LEA` instruction effectively computes A1 + 2 * A1 and stores it in A1.
Well, A1 + 2*A1 is 3*A1.  Doesn't matter if A1 is just a number and not
a real address.

I had two breakthroughs from seeing this:

  - maybe the compiler is cleverer than me, or better at picking some
    instructions;
  - it doesn't matter what the instructions are _intended_ to be used
  for, it matters what they _do_. The purpose of an instruction is what it
  does.


## The Unix years

As an undergrad at University i was dying to get onto a Unix system,
but you could only do that as a grad student or a final year undergrad
with permission.  As soon as i was in that final year i got the
necessary permission from my maths tutor, with the flimsy pretence
that i need Unix for 10 MB Lisp images (i did do some of the
computational part in Lisp).

Which led me to `/bin/vi` and learning from a file called `vitut.txt`
i think. And this vi/ex command:

    :g/^/m0

which i now know is also in Linda Lamb's Learning the vi Editor
(it astonishes me now that i never bothered requesting this from
any of the 4 well-stocked libraries i had access to and that surely
would have carried it).

What does it do?  Looping over every line of the file in `vi`, it moves
that line to after line 0, that is it becomes the new first line.
Because of the sequencing, it reverses the order of lines in the file.
Neat. But hardly ever useful.

Lamb might like to write it `: g /^/ m 0` to separate out the parts.
`g` is the global command that applies the command following to every
line that matches the RE.  `/^/` matches any line that has a beginning,
which is of course every line.


## Danger

This one is dangerous and should not be tried at home, and definitely
not tried on the shared Unix server, but at least if you do try it,
try it just before tea break so everyone that can take their enforced
screen-time break in the staff tea room.

It's the quite well known Unix shell fork bomb:

    :(){ :|:& };:

A fork bomb create many processes rapidly (each new one
creates two more), and in a typical Unix server will fill up
the process table and crash.

Instructive in how few safeguards Unix might have.

But also, i had a burgeoning interest in `/bin/sh` and decoding
this little snippet of shell involves some deep and arcane
knowledge and possibly much studying of the man page.


## Unix, once more

Have you ever wanted `uniq` but didn't have your input sorted? Or
didn't want to sort it?

Well, that's

    progthingy | awk '!a[$0]--'

I think the canonical version of this, which i probably found
in the AWK FAQ uses `++` instead of `--` but `--` works and is a tiny
bit easier to type.

Again, like the shell forkbomb, the real insight is the trips
to the man page to decode the meaning carefully.

Every input line is indexed into the `a[]` array and has its key value
decremented (incremented in the FAQ version).  The initial value is
blank which when inverted by `!` becomes `1` and the line matches
and gets printed.  Any subsequent line that matches exactly won't
print because its key value in the array will already be non-zero.


## The Unix Programming Environment

This is another very short part of a Unix pipeline:

    progthing | sed 9q

`sed` is a stream editor, and this program `9q` quits on line 9.
The default action of `sed` is to copy its input to its output,
so this program quits after 9 lines of input have been copied.

This was one of those moments where i knew `sed`, or so i thought,
but i just hadn't thought about using it like this.

It's given as an example in the excellent Unix text,
The Unix Programming Environment.  Which was already aging a bit
by the time i read it in the 1990s.  Around this example they talk
about using existing utilities, and how that in principle
one could imagine a utility _dedicated_ to displaying the first N
lines of a file, but why would anyone go to the trouble of creating
that, when `sed` exists?

Of course by the time i am reading this in the 1990s, every system
has `head`, the utility they are railing against, and everyone
has forgotten you can use `sed 9q`.  Even though, as the authors
point out, it's shorter than `head -n 9` or even `head -9`.

So now in 2026 i use `sed 9q` not because i am ignorant of `head`
but to talk the smallest possible stand against the deluge of
software on Unix that doesn't embody the Unix philosophy.


## Python

I once had the pleasure of meeting esteemed Python guru
Raymond Hettinger and we got to chatting about, i dunno, basic
Python builtins and i bemoaned that i loved `zip()` which takes
2 lists and forms a list of pairs, but where was `unzip()` which
should take a list of pairs and return 2 lists?

    zip(*s)

Like a swift kick to the head, zip* is unzip.
And matrix transpose, where it
featured in Norvig's Infrequently Asked Questions.


## FORTH

A language i had been exposed to as a child with no success,
but a fair amount of intrigue.  Easy to crash and on a ZX81
that meant reloading from tape.

Fast forward to the 21st century and i had the silly idea to
implement my own FORTH in 64-bit x86 assembler.  Part way
though that project i am browsing various things and in
someone else's small x86 Forth i see:

    SBB AX, AX

Subtract with Borrow.  What is the use of substracting AX from itself?
This is always 0? No, it's 0 when the carry flag is clear, but
when the carry flag is set, the result is -1; that is "all 1s".

So it's useful to copy the carry flag into the AX register,
which happens a lot in FORTH because comparison operations
might be implemented via machine language instructions that
leave the result in the carry flag, but in FORTH a numeric
value needs placing on the stack.  There is also a certain
FORTH tradition of making the TRUE value be all 1s so that
bitwise operations can be used it like a mask.  I think i
also used it for a branchless ABS.

The equivalent opcode in Z80 machine code is

    SBC A, A

(or just `SBC A`). It's used in the keyboard scanner of the ZX80
for exactly the same purpose.

## GO FORTH

I had more or less implemented my FORTH when i was poking around
the FAQs or somesuch.  And came across the only half rhetorical
question, "how do you implement coroutines? and can you do it only one
line?".  Answer:

    : CO R> R> SWAP >R >R ;

`CO` is now a FORTH word which suspends execution of the current
word and resumes its caller.  Effectively making a coroutine.
The caller can now resume the fresh coroutine where it left off
by calling `CO` again.

Of course this worked on my FORTH even though i never knew this
trick and only discovered it after implemented FORTH.

## 10PRINT

So apparently every other nerd on the internet knew this since 1982,
but not me.  I was one of that day's 10,000 when in _2023_ i came across
the book 10PRINT by Nick Montfort et al.  A book of essays
considering the text of the maze program made famous by its
appearance in the Commodore 64 manual:

    10 PRINT CHR$(205.5+RND(1)); : GOTO 10

In essence only a little more complicated than my first "DRJ IS SKILL"
program, but a true wonder that it produces a real maze.

It's a beautiful program, and i recommend the book too.

# END
