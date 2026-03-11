[brutal]: #date "2026-03-11"
[brutal]: #author "drj"
[brutal]: #title "Some alternatives to vim"

Recently some slop vommits were made to the source code repo hosting _vim_,
the well-known editor based on _vi_ the well known _Visual Editor_ from Unix.

Depending on your viewpoint this is somewhere between world-shatteringly deplorable
to irrelevant.
For me, it is a minor annoyance. I use vi/vim on the daily and i do not want
to be associated with slop vommit tools.
But i’m also somewhat pragmatic and recognise that i probably already am.

The short version is that i have now compiled modern-ish version of both `vi` and
`nvi` and will be trying those.

I mostly switched to vim in the mid 1990s,
swiching away from _nvi_ because it had bugs over the execution of paste buffers
(did you know `@x` executes buffer _x_ as if it were typed into vi?).

But ever since then i’ve really been wanting to return to `nvi`, or maybe
even `vi`.
Like, i know `vim` does some cool things like you can use the arrow keys in
insert mode, but honestly i don’t use them that much.
And it is does a lot of things that lowkey annoy me. Anyway.

Fast forward to 2026 and `nvi` now works. Maybe.
I have spent some time (a not very productive day) investigating some alternatives
to vim:

- [_evi_](https://codeberg.org/NerdNextDoor/evi) a hard-fork of Vim from a point
  early enough to avoid Ai. I have no intention of using this, because for me
  if i am switching away, i may as well revert to `nvi` or `vi`. But it was
  a NerdNextDoor fediverse post which alerted me to the slop vommit issue.
- `emacs`. Haha. Emacs was actually the first Serious Editor i used that wasn’t
  entirely parochial (i used E and [ZED](https://archive.org/details/zed-reference-manual)
  before that, but they existed only
  on PHOENIX the university’s baroque 3084Q mainframe). And i know emacs pretty well.
  emacs 16. I will not be renewing my interesting in emacs.
- `nvi`. `nvi` is _new vi_, made to replace `vi`. But there are loads of different
  versions. On my Mac the _brew_ version installs but has display problems for
  non-ASCII characters, like þ. I found the Keith Bostic (author of `nvi`) source, but
  couldn’t work out how to compile it. I found repo.or.cz/nvi.git but it’s
  geoblocked (thanks to the UK’s oppressive digital regime). So what i’m actually
  using to type this blog article is:
- `nvi2` (it compiles a binary called `nvi`) which is [https://github.com/lichray/nvi2](https://github.com/lichray/nvi2) .
  Despite my trepidation at it requiring `cmake` and `ninja` to build (which i had
  to install from homebrew) the build instructions ran fine, trouble free, and
  relatively quickly.
- [_Heirloom vi_ at https://github.com/n-t-roff/heirloom-ex-vi](https://github.com/n-t-roff/heirloom-ex-vi)
  which i have now compiled (with `sh configure ; make`) and seems to be fine so far.
  Traditionalists will be delighted to note that the arrow keys do not work
  in insert mode. There is a note in the README about getting `PageUp` and `PageDown` keys
  to work tho.

The big change since the 1990s is that Unicode now works.

## Addendum on a `vi` bug

I discovered while writing blog post that...

`de` and `ye` put different texts into the paste buffer.
`ye` includes the character under the cursor, `de` does not (this last bit
seems like a bug, but the fact they are different is definitely a bug).

# END
