This article starts with a [post on
Twitter](https://mobile.twitter.com/PrincenAlice/status/1125446258829725697)
about a quirk in the Unix `read()` system call.

The issues are around the question:
"what happens if there is more input available that read() returns"?

To transplant that into the command line,
try this in your own command line shell:

Type `dd bs=2 count=1` followed by the ↵ key.
The terminal is now waiting for you to type in the input to the
`dd` command.
Type `lols` followed by the ↵ key.

Did you see a listing of the files in your directory?
I get something like this:

    ac1xdrj@owl:~/tmp$ dd count=1 bs=2
    lols
    lo1+0 records in
    1+0 records out
    2 bytes copied, 4.05288 s, 0.0 kB/s
    ac1xdrj@owl:~/tmp$ ls
    dl  metrics.4030284.csv

It looks like the `dd` command is somehow
passing the string `ls` to my shell and getting the shell to run it.
That's not quite what's going on, but we need to do a little bit
of unpacking to work it all out.

`dd` is an ancient program to read and copy data in various ways. 
I'm using it here to read 2 bytes from the input.
You can write a C program to do that if you like.
Compile and run this for the same effect:

    #include <unistd.h>

    int main(void) {
        char cs[2];
        read(0, cs, sizeof cs);
    }

Is `dd` (or the C program above) really sending data to the shell? No.

What is happening is that `dd` is reading exactly 2 bytes from
its `stdin`.
As it happens 5 are available (`l`, `o`, `l`, `s`, `␊`).
The Unix kernel returns 2 bytes to `dd`.
`dd` then exits.

The unread bytes that `dd` did not read are still available.
The next call to `read()` will return them.

In this case, the next call to `read()` is
the shell that originally invoked `dd`.
The shell calls `read()` for the next command.
It finds the input `ls␊` and treats that as a command to run.

It is true that when you typed `lols` the `dd` command was running,
so you might think of it as `dd`'s input, but it only really
becomes `dd`'s input if `dd` actually reads it.
Unread data are left in the kernel buffer for the next `read()` call.

It is perhaps not obvious that
in this case the shell and `dd` share an input, but they do.
When you type commands into a shell you are typing into its `stdin`, 
but when the shell invokes a command,
unless you have arranged for a different `stdin`,
the shell's `stdin` is shared with the command being invoked.

Mostly you don't notice any of these weird effects because:
Most programs read all of the input and don't leave input unread.
The interactive case is slightly unusual in that the shell and
the program being invoked share a `stdin`.

If you give a program a different `stdin`,
it won't be shared with the shell.
Try this example: `echo lols | dd bs=1 count=2`.
Here, the output of `echo` has been piped into the input of `dd`.
`dd` reads 2 bytes, copies them to its output, and exits.
That's exactly what it did before.
The difference here is that no other program shares the input to
`dd` so no other program reads the unread bytes.
In this case the unread bytes (`l`, `s`, `␊`) are tidied up by
the kernel when the last process that could read them exits.

The setup above is deliberately obscure:
a program that reads only a very small portion of the input.
There are other cases where a program might naturally read some
but not all of the input.
`head -n 1` copies the first line of its input to its output.
How much of its input does it read?
Consider: `cat big-long-file | head -n 1`.
In this case `head` does not need to read the entire input,
it would be a waste to read the entire input.

`cat thing` is a fairly innocuous example
(no, I don't want your Useless Use of Cat award).
Instead of `cat thing` we might have something that using both
CPU and network to provide input:
`curl https://some-large-compressed-object | head -n 1`.
In this case it might be very useful for `head` to
only read the first part of its input.
Also we might have a program that produces an infinite stream,
but we still only want part of it:
`yes 123456789 | head -n 1`

So, how much input does `head -n 1` read?
It reads enough of its input to read one line and then stops.
It can't know without reading its input how much it needs to read,
so it generally reads _some_ input and then
if it has already read enough it stops,
otherwise it carries on and read some more.
Generally an ordinary utility program like `head` will read
input in blocks of 4096 or 8192 bytes
(these are round numbers in binary and
fit well with disk and RAM sizes in a computer).

Consider: `yes 123456789 | ( head -n 1 ; head -n 1 )`.
Does this program terminate?
How many lines of output are written?
What is the output?
A caution here: the answers I got vary and are clearly going to
vary from system to system.

The `yes` program, by the way, repeatedly outputs its argument
on an infinite sequence of lines.

So the answers are: Yes, the program terminates;
Two lines are output (each invocation of `head` is responsible for one line);
The output is:

    123456789
    3456789

(maybe). The first line is definitely `123456789`,
the second line could be a variety of things.

What has happened here?
`yes` is producing an infinite sequence of lines,
each 10 bytes long (9 digits, plus a single ␊ byte).
The first `head` reads some input, extracts the first line from
it and writes that line to the output.
We know that is `123456789`.
The second `head` shares its input with the first head.
It reads some more input from where the first `head` left off.
We have a clue, the first byte of the second line is `3`.
This is consisent with the first `head` reading 8192 bytes:
819 lines of `123456789` followed by the first two bytes of the
820th line: `12`.
That leaves the second `head` to start right where that left off
and it reads starting from `3`.

We can use `strace` to see what `read()` system calls `head` is
making:

    $ yes 123456789 | ( strace -e read head -n 1 ; head -n 1 )
    read(4, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\t\2\0\0\0\0\0"..., 832) = 832
    read(4, "# Locale name alias data base.\n#"..., 4096) = 2995
    read(4, "", 4096)                       = 0
    read(0, "123456789\n123456789\n123456789\n12"..., 8192) = 8192
    123456789
    +++ exited with 0 +++
    3456789

(here we have used `strace -e read` so that `strace` only shows
the `read()` system calls)

We can see a bunch of `read()` calls on other file descriptors,
followed by `read(0, blah blah blah, 8192)`.
`head` is reading 8192 bytes from stdin (file descriptor 0).

The output after `+++` is produced by the second invocation of `head`.

If you've read Kernighan and Pike's «The UNIX Programming Environment»
you'll be aware that `sed q` is a replacement for `head -n 1`.
If we try the above using `sed` instead of `head`:

    $ yes 123456789 | ( sed q ; sed q )
    123456789
    789

The second line starts with a `7` implying that
the first invocation of `sed` reads 4096 bytes
(or some other number ending in 6).
A fact you can confirm using `strace`.

Note that the fact that `head` reads 8192 bytes and `sed` reads
4096 bytes is not really a hard fact.
It's just a partly visible consequence of the buffering
implementation that they both use.
So it may be that on your favourite Unix you get different results.
While I was writing this article,
I half-remember getting different results out of `head` and `sed`,
depending on exactly how I invoked them.
But I can't seem to reproduce that now.
