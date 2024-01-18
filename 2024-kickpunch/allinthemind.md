[brutal]: #xdate "2024-01-18"
[brutal]: #author "David Jones"

<h1>Sunny Funny</h1>

I remembered i had a barely used, but very only, Acer netbook in
the attic. It turned out to be an Acer ZG5 from 2008 with a 1.6GHz CPU,
1GB of RAM, and a 160GB rotating disk (this configuration
isn’t listed on Wikiepdia, but whatever).

I had seen a few posts about people running Haiku (the new name
for BeOS) on this class of hardware.
BeOS, an obscure darling child of 1990s, should run reasonably
well on this early 21st Century hardware.
And it does.
After a false start of installing it on a partition which the
pre-existing grub couldn’t then boot into,
i installed it on the raw unpartitioned disk (i think).

A little poking around reveals that doing ordinary web things is
a disaster, but loading BeOS websites from the early 2000s and
before works fine.
What project could i try?

Then i came across
[this post from Nina Kalinina about "cling wrap
scanning"](https://typo.social/@nina_kali_nina@tech.lgbt/111456601630935995).
Using a pen and clear plastic film, you take a copy of an
existing art.
Then put the plastic over the screen (with tape or the magic of
cling film) and use an image editor to copy the art with pixels.

That would be a fun project to use the Haiku Netbook for.
Looking through my collection of videogame media, i even found
some art.
This amazing poster from [PaRappa the Rapper].

I traced over PaRappa using actual clingfilm, and it was
terrible. It sticks okay onto the netbook though.
But the WonderBrush art program even in fullscreen only lets me
use a small portion of the total screen for the editing area.
I solved this problem by tracing over Sunny Funny and taping
the art sideways (the artwork for Sunny is slightly narrower
than PaRappa).

What resolution? Something chunky like 320×200 perhaps? I
actually traced using 200×200 at 600% (i think).

Once i had basic outlines i removed the clear plastic pen copy,
and worked by eye.
WonderBrush isn’t really a true pixel editor, and even with the
"subpixel" box unchecked would "half-shade" pixels as a freehand
pen line was drawn with the mouse.
Also my mouse skills are bad (possibly exacerbated by an
authentic vintage mouse, and not a pro-gamer laserballs) and so
the actual shapes were not very accurate.

Digital art and pixel editing is not something i've done before,
so it took me many many hours of single pixel cleaning and
reshaping to get this:

Nice. But needs rotating, right? Simples.

Aha. Can rotate in WonderBrush but it resamples with some sort
of high-fidelity cubic filter.
Which for bilevel pixel art is a disaster.
Couldn’t work out how to fix that.
Seemingly can’t rotate in the image viewer.

Can install ImageMagick. Yes we can, but it seems not to work.
(I also tried both installing and compiling from source
ArtPaint, but that didn’t work either).

`git` works fine by the way.

So, netpbm?
