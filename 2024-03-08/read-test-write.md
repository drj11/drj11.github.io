[brutal]: #xdate "2024-04-08"
[brutal]: #author "David Jones"
[brutal]: #title "Testing read asymmetry write"

Some thoughts of the asymmetry of testing a library that reads a
file format versus one that writes a file format.
Mostly arising from my work on
[PyPNG](https://gitlab.com/drj11/pypng) which reads and writes
PNG files, but also informed by work on International Color
Consortium Profiles (ICCP) and TrueType Font files (TTF, OTF,
and so on).

Consider a library which reads and writes a particular file
format.
Like PNG for images, or AIFF for sounds files, or SVG for vector images.

As a rule of thumb, writing is easier than reading.
That’s because when writing you can make decisions and choose
options that simplify the things you can write.
Taking PNG as a more concrete example, you can decide to not write
interlaced images, only write 8-bit images, only write
single-channel greyscale images.
If you do all of those then you only need [about 24 lines of
Python to write a PNG image](https://gitlab.com/drj11/minpng).

Whereas when you’re reading, you have to cope with any input
PNG, and in the case of PNG there are quite a few formats.

Testing however, is somewhat the reverse. Almost like a dual.

Testing reading is relatively simple, just
find [a bunch of test images ](http://www.schaik.com/pngsuite/)
read them and check that you have the correct pixels.
How do you know you have the right pixels? Well, read the same
image in a different format, like GIF. Um, what if you don’t
have a GIF library? Maybe you can convert the images to
NetPBM which is simple enough to decode ad hoc, or
convert the images into Python literals.
FWIW, i don’t think PyPNG tests a single image like that.

Testing writing is simple in some aspects, it’s relatively easy
to test that you’ve written something. How do you know it’s in
the correct format? Read it in i suppose. Reading it in using
your own library seems like cheating, because maybe
you are making symmetric bad readings of the spec and you are
both writing out a PNG that isn’t compliant, and reading one
back in. So you need an external reader that can be trusted
to correctly interpret good PNG files and be reasonabily
diligent in rejecting badly formatted ones.

How do you generate input images for testing reading? Well, if
you can’t find them lying around, maybe you could make some.
By creating a library that writes them... What if you have a
library that writes images, but doesn’t write Adam7 interlace
images, say. How do you now create images in Adam7 interlace
format in order to test if you can read them? I suppose you’d
better add that feature to the writer.

# END

