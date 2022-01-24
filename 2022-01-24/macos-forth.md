[brutal]: #title "Implementing FORTH on macOS"
[brutal]: #author "David Jones"
[brutal]: #date "2022-01-24"

_SixtyForth_

A few years ago i
[implemented FORTH for 64-bit Intel architecture (AMD64)](https://gitlab.com/drj11/sixtyforth).
This was on Linux and i had deliberately not spent much time
thinking “portably”.
Most obviously not in terms of operating systems,
but also CPU architecture, and wordsize.

After buying a second-hand 2015 Mac Book Pro in
the second March of the long year of the pandemic
i would every now and then tinker with SixtyForth to
try and get it working on macOS.

With a bit of a push over the weekend i managed to get a basic
implementation going: it reaches the interactive prompt, but
the nano-style line editor does not work.

What is involved?
Well, the 2015 mac predates the switch to
Apple Silicon so it is a 64-bit Intel architecture.
That's a good start.
macOS implements a largely POSIX subsystem,
so that too is a good start.

Many blog articles on the very low-level details,
even ones that are using assembly,
assume you are writing assembly _and_ C, or
are targetting the C runtime (so you may have a conventional
`main()`, or are willing to link to the C library).

There are not many hints for people who eschew C entirely.


## Building

Building is the first stumbling block.
I use `nasm` for the assembler.
macOS and Linux use different object file formats.
Linux uses ELF, so i use

    nasm -w+error -g -f elf64 -o 64th.o -l ignore/listing 64th.asm

but macOS used Mach-O, so i need `-f macho64`

    nasm -w+error -g -f macho64 -o 64th.o -l ignore/listing 64th.asm

The source for SixtyForth is currently two files: one `64th.asm`
file, and one `rc.4` file.
`64th.asm` implements a fairly minimal interpreter, and
`rc.4` (run commands FORTH) contains the rest of the
implementation, written in itself.
The `.asm` file is compiled using `nasm` (above),
the `rc.4` file is “compiled” using `objcopy` making an object
file that has a byte-copy of the `rc.4` text file.
As part of FORTH startup the `rc.4` file is passed to `EVALUATE`
as a single string.

On macOS i couldn't wrangle `objcopy` to do this.
So the `rc.4` file is now compiled using `nasm` and a `INCBIN`
directive.
The end result is the same, a `.o` file that has a byte-bopy of
the text file.
The extra file is a little bit more annoying, but in a way it’s
a more portable toolchain because we already use `nasm` and we
remove `objcopy` as a dependency.

The linker step also changes. Relatively straightforward on
Linux:

    ld -g -o 64th 64th.o rc.o

On macOS:

    ld -L$(xcode-select -p)/SDKs/MacOSX.sdk/usr/lib -o 64th -arch x86_64 -segprot __DICT rwx rwx -macosx_version_min 10.13 -lSystem -no_pie -e _start asmrc.o 64th.o

Of note:

- `-e` option can be used to select the entrypoint;
- `-arch` must be explicit (good idea anyway);
- `-L` option seems over-the-top? `xcrun` didn’t seem to help,
  but i have a hard time believing that i have to link to
  `-lSystem` like that;
- the `-segprot __DICT rwx rwx` selects a non-default memory
  protection for a custom segment, more on that next.


## Sections and self-modifying code

The sections have to have different names when using the Mach-O format:
_SEGNAME,sectname_.
Conventionally both parts start with `__`, for example:
`__DICT,__data`.
In Mach-O all the sections with the same SEGNAME (segment name)
are joined together in a _segment_.

By default in macOS a segment is not both writable and
executable.
Data segments are writable and not executable;
code segments are executable and not writable.

Which eventually leads to the line in the linker:
`-segprot __DICT rwx rwx`.

SixtyForth requires that memory is both writable and executable
in order to implement the class-defining word `DOES>`.
At compile time `DOES>` pokes machine code into the word
containing it, and arranges at run time for the most recently
defined word to execute that machine code.

A future port to Apple Silicon (ARM based, extremely not x86-64)
would require a different strategy as
it is not possible to have a segment that
is created by the linker that is both writable and executable.
It is possible at runtime by using `mmap()`.

Currently in SixtyForth `DOES>` is only used in the definition
of `CONSTANT`.
Alternative strategies could be:

- Implement `DOES>` so that it does not compile machine code
  (not really in the spirit of FORTH and very slightly less
  efficient);
- Put the modifiable dictionary in a suitable JIT `mmap()`
  segment;
- Implement `CONSTANT` (or the run-time part of it) in assembly;
- Eliminate the use of `CONSTANT` in `rc.4`.

Some of these strategies combine.
It is possible to `mmap()` a
segment and link the dictionary to it
so that new definitions may use `DOES>`.
There is a separate question as to wether it is desirable to do
this before or after the `rc.4` code wants to use `CONSTANT`.


## System Call numbers and structures

The syscall numbers change between Linux and macOS;
for example `write()` is syscall 1 on Linux and syscall
0x2000004 on macOS.
For system calls that take in-memory structures, like the
`termios()` family, the in-memory structures are different.

Because both systems implement the Sys V AMD64 (aka x86-64) ABI
the rest of the calling conventions do not change.
Same registers and same `syscall` instruction.

SixtyForth only makes about 6 different system calls
(read, write, mmap, exit, open, ioctl).

syscall numbers are taken from
[https://sigsegv.pl/osx-bsd-syscalls/](https://sigsegv.pl/osx-bsd-syscalls/)
with the not-very-well documented caveat that
you have to add 0x2000000 to all the syscalls.

The flags (for `mmap()`) and the structs for `ioctl()` are
a bit more tricky.
Sure you can poke around header files and you get stuff like:

    #define TIOCGWINSZ      _IOR('t', 104, struct winsize)  /* get window size */

For `TIOCGWINZ` (get winsize). Hmm.
I suppose i can go and find the defintion of `_IOR` and guess
the size of `struct winsize`, but in practice it was easier to
write a C program to print out the value.
Which in this case is 0x40087468.
Incidentally it's nice to see traditions living on: that 
`'t'` in the macro is 0x74 ASCII which becomes
the hex-digits 74 in that long number.
The idea of encoding the subsystem in the `ioctl()` command
and using _t_ for terminal goes back to the first implementation
of `ioctl()` in
Unix Version 7.

It’s similar for the structs.
You can go to the header files again,
but i still need to know the _exact memory format_ for
`tcflag_t` and how `cc_t c_cc[NCCS]` is packed.
On Linux there is an extra wrinkle in that often the C library
interface and the syscall interface use a different struct and
there is glue code to shuffle data around.

Best thing to do is create a buffer, make the syscall, and
hexdump the buffer to work out the exact memory layout with
whatever struct definitions you have available as a hint.
This is one of the reasons that `DUMP` is implemented in assembly.

It may seem dodgy or underhand to obtain knowledge in this way,
but the knowledge obtained _absolutely cannot change_ because
it forms part of the ABI with the operating system.
For as long as the OS guarantees that old binaries will work,
it cannot change the length or layout of structs and it cannot
change flag values.


## argc, argv, envp

On Linux at program start $rsp contains the address of `argc`,
which is immediately followed in memory by the the argv vector
(essentially `argv = &argc + 8 bytes`) and then the envp vector.
It so happens that SixtyForth doesn’t use $rsp so it is
unchanged by the time that the `rc.4` code wants to inspect the
program arguments.

On macOS argc, argv, envp are _as if passed as arguments_.
The initial register state has them in $rdi, $rsi, $rdx
(thanks `lldb`).
Those registers _are_ used by SixtyForth and because we need
those values later they have to be specially saved.
I added code to the `_start` entrypoint to save them in a block
of memory that can be recalled with the FORTH word `iplsave`.


## The Unified Future

So far all of this work has been done on a branch.
What will a unified future look like?

In `.asm` we can have macros for the syscall numbers and
the `SECTION` directives.
The FORTH side of the `iplsave` mechanism can be the same,
but the assembly that implements it will have to be different;
so i guess that can be a macro too.

For the FORTH side of system calls we can use symbols with
different numeric values for syscall numbers, flags, and struct
offsets.
Where tho?

Options:

- define numeric values in assembly, one file per OS;
- at runtime in FORTH by using not-yet-defined LINUX? MACOS? words;
- a separate per-OS FORTH file that is included;
- edit the rc.4 file with text-hackery prior to compiling it.

In principle i prefer a separate `.4` file.
In practice it will mean splitting the `rc.4` file up as well.
Probably a good thing to be honest:

- portable rc.4
- os-variant.4
- mildly os-dependent rc.4

Might be a good idea to start splitting `rc.4` anyway.
At the moment it contains a few hundred lines to implement the
nano-like line editor which is more like an app that is
implemented in SixtyForth, and big enough to be in its own file.

# END
