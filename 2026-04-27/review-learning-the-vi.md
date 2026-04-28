[brutal]: #xdate "2026-04-27"
[brutal]: #author "drj"
[brutal]: #title "Learning the vi editor"

I have been browsing the `nvi` sources and Keith Bostic (principal author)
makes a couple of references to
_Learning the vi Editor_ by Linda Lamb.
I can’t remember if i knew this book in the 1990s when it came out and
when i was learning vi, but i wish i got a copy back then.

I’m reviewing the 5th edition, and for this book it matters.
After the 5th edition the book got renamed _Learning the vi and Vim editors_, got
a lot thicker, and added lots of distracting content about _Vim_.
_Vim_ is not _vi_.

The short version is that this edition is awesome and if you are serious
about _vi_ you should try and at least [read a copy](https://archive.org/details/learningvieditor00lamb_0).

It’s a small book at 206 pages, and, perhaps in recognition of that, features
a _Tarsius_ on the cover (what? not a _Lamb_?); it’s very cute.
The pace is good, a reasoned and gentle introduction to basic editing,
before ramping up to more advanced tasks, uses of _ex_ (the underling line editor),
and then spiralling out into orbit with scripting and automation
(and incidentally, a concise but very practical introduction to _shell scripting_).
As the book itself proclaims, you could stop after the first 2 (or 3) chapters and
be reasonably proficient with _vi_ on the daily.
The book is there and ready when you are curious to learn more.

It was this sort of book that laid out the backbone of what was to
become O’Reilly’s dominance in the technical book market for
practising Unix professionals.
Clear layout with impeccable technical experts and _writers_ making both
tutorial and reference content (both in one volume for vi).
Also physically well made and with a cute animal on the cover;
the copy i have now must be 30 years old, and
although it has the glued perfect binding, it
still lies flat without cracking.

There is a handy reference section at the end.
The 5th edition also comes with
a glossy 3-panel folded reference card which is
attached in a perforated insert at the back
(luckily my second hand copy still has it attached,
and i haven’t dared tear it out either).
The tear-out car seems to largely repeat the
reference section in the main body, but 
is slightly more accurate and comprehensive.

One minor niggle is that programmers might not
find the small and useful section about lisp-mode, indentation,
and ctags.
It is squirreled away at the back of the last chapter, and while
it is signposted earlier, it is easy to miss.
Programmers should skim the end of the last chapter after finishing chapter 2.

## A little more detail

Chapter 1 — The vi Text Editor — introduces `vi` as a Unix command:
how to start, how to quit, and a few troubleshooting checklists to consult
if things don’t seem to be working (the checklists continue in the rest of
the book and often contain hidden nuggets).

Chapter 2 — Simple Editing — is where the real work beings. Actually editing
files with vi. `h j k l` `c d i` all of that.
The pace is slow and careful, and illustrates many of the commands with
before- and after-diagrams showing how the cursor moves and text changes.

The next two chapters are Moving Around in a Hurry and Beyond the Basics;
while they are not _critical_ for editing using _vi_ i highly recommend them
(and they are shorter than the first 2 chapters).
They cover searching (without using regular expressions,
which are covered later), other movement commands (like `z` and `H` which
are some of my favourites), and marking and buffers.

You could stop there and declare yourself a competent _vi_ user, but
the book, like _vi_ itself, tempts you onwards to learn more powerful
techniques.

Chapter 5 is Introducing the _ex_ editor; _ex_ while technically a
different editor is also technically part of _vi_:
you can use _ex_ from _vi_ and you can use _vi_ from _ex_.
A lot of this chapter is moving blocks of text around and into
and from other files.
One part of _ex_, global replacement, is so powerful that its
treatment has the whole of the following chapter.

Chapter 6 — Global Replacement — is for the `g` and `s` commands
in _ex_; it also features _regular expressions_ fairly extensively.
The chapters are getting quite long now, and a lot of the
material here is definitely expert stuff.
One of the best bits is several pages of extended examples,
which are a very diverse set of small snippets that do
useful things.
Some of them are classics and some of them have been made
classics by this book.
My fave is `:v/./d` to delete all blank lines (my version
is actually one character shorter than the one given in the book).

Chapter 7 — Advanced Editing — is the final and longest chapter (over
30 pages of reference material follow though) and is
somewhat miscellaneous.
How to add custom commands and keyboard mappings to _vi_,
and how to script with _ex_ i consider as fairly esoteric; but
using Unix filters, like `!}fmt` to run the next paragraph through
the Unix utility `fmt`, is fairly fun and within ordinary competence.
This is also where the section for editing program source code appears.


## The Historical Perspective

The 5th edition was published in 1990 and i am reading the 1994 revision of that.
That’s over 30 years ago.
How much is still relevant?

Well, basically everything except about half of the last chapter,
very definitely still relevant.
Every now and then there is a bit of discussion of using slow (300 baud)
dial-up terminals, or types of terminal that were obsolete even in 1990.
That stuff is not so relevant.

A lot of the material for the more advanced sections is based around
document preparation using _troff_, the tradition Unix typesetting system
that was used to prepare this book.
It helps if you know a bit of _troff_ (which i do), but
i doubt there are many people still writing new documents
in _troff_ macro packages, [love letters
aside](http://www.schemamania.org/troff/for-the-love-of-troff.pdf).
So maybe the typical reader in the 21st century does not feel
motivated by these _troff_ sections, and also slightly
bewildered at seeing _troff_ syntax, which has very 1979 vibes.

I feel that parts of the book have been left behind.
People don’t maintain little address books using `vi`
in plain text files.
They don’t have `vi` scripts that convert _troff_ sources
to indexes.
The task of personalising the computing platform we use,
has been taking over by
scripts written in Perl or Python or
has been swallowed up by little apps and webpages.

One of the examples, more sketched than fully developed,
uses diff’s _ed_ mode (also compatible with _ex_)
to create a series of versions of a file.
Version control by-hand if you like.
This is a nice example of more powerful scripting using `vi` and
other Unix tools, but even in 1990 the overall motivation is weak:
The version control systems SCCS and RCS were available, although
possibly not available on every Unix system because of
licensing issues (perhaps that in fact motivates the idea of
managing versions using ad-hoc scripts: SCCS wasn’t installed
on your Unix).


## Closing notes

Probably still the best reference on _vi_ all in one place.
Users of more recent “clones” will probably be surprised by
what `vi` could do (and still can, if you can find it).
Also great if you like the way all Unix textbooks used to be
in Helvetica and Times New Roman.

# END
