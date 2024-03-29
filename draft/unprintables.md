[brutal]: #title "unprintable characters"
[brutal]: #author "David Jones"
[brutal]: #date

ASCII is a very common system for encoding English language text.
There are 94 graphical symbols that represent the letters of the
alphabet (in both upper and lower case), numbers, punctuation,
and few other typographical symbols (parentheses, brackets,
asterisk, and so on).

The typewriter predates electronic computers,
and still lives on in the keyboard of our laptops.
ASCII more or less covers most of symbols you can type on a
typewriter.

In ASCII each one of these symbols is assigned a number
from 33 (which is « ! ») to 126 (which is « ~ »).
With an additional "space" symbol (code 32)
we can represent the text of most of English.
As long as we accept some compromises.

For example, we can't properly encode any article from
The New Yorker, as they regard the diæresis as part of their
identity, and ASCII only has the plain undecorated versions of
vowels and has no letters with any decorations.

How does this come about? Why pick codes from 32 to 126?
It turns out that codes 0 to 31 are _control codes_
that have some non-print effect.
For example code 8 is _backspace_, which has the effect of
moving the cursor backwards one space.
Typewriters typically have a backspace key,
often in more or less the place that is on modern keyboards.

7 bits in a binary counting system, can represent numbers from
0 to 127.
So each ASCII symbol can be encoding in 7 bits.
A byte is 8 bits (and has been for ages), why not use an 8 bit code?
two reasons:
1) the communications protocol in use might by a 7-bit channel.
Serial channels are often configured as 7-bit + parity;
2) it leaves the "upper 128" codes as free for a variety of
compatible extension codes.

Obviously these two reasons are in conflict, but there you go.

So let's talk about codes 0 to 31,
the unprintable control codes.

You've heard of some of these.
Backspace (code 8) I alread mentioned.
A newline is made up of two controls.
One, linefeed (LF), moves to the next line, keeping the same same
positions.
The second, carriage return (CR), moves to the beginning of the line.
Together a CR LF sequence will position the cursor at the
beginning a new line.

On Windows, inherited from its predecessor DOS, text files
use the two byte sequence CR LF to indicate a new line.
This means that we can display a file on a terminal by
transmitting each and every byte in sequence.

Unix takes a different approach.
A new line is just one byte in the file,
it's a linefeed (LF).
On Unix when we display a file on a terminal,
the program doing the displaying (for example, `cat`) transmits
each byte of the file to the terminal.
There is code in the Unix terminal drive that expands each LF
byte into a CR LF sequence.

This Unix approach simplifies application programs,
which only have to deal with one byte for a new line,
at the expense of having more complicated terminal device
drivers.


