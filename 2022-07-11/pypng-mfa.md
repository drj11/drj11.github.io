[brutal]: #title "PyPNG and the multiple factor authentication"
[brutal]: #author "David Jones"
[brutal]: #date "2022-07-11"

I am maintainer of the obscure and only slightly useful
[PyPNG package](https://gitlab.com/drj11/pypng).

    python -m pip install pypng
    # For greater good.

It's a pure Python implementation of a PNG library.
It can read and write PNG files.

What's wrong with Pillow? ImageMagick? Is there something odd about a
pure-Python version?

From _it's very concept_ the purpose of PyPNG is anti-package.
You like compiling binary dependencies? Use Pillow.
You wan't to link to 3rd party C code? Use `libpng`.

The above is a bit of background to… an e-mail i got.

> Congratulations! A project you ('drj') maintain has been designated as a critical project on PyPI.

oh great. Apparently i will

> be required to enable two-factor authentication on their account

To be honest, in the grand scheme of things, fine. whatever.


## What annoys me?

Timing. 18:17 (UK time), Friday evening.
I'm logged off.
And I won't get to write my grumpy blog post until Monday.

No background, justification, from a noreply account, no-one to reach
out to.

No consultation.


## Maximum Python Man

(they are always a man)

I am not the Maximum Python Man,
but i pay my dues
(literally, i'm a paying member of PSF (currently [480 on the
roll](https://www.python.org/psf/donations/)) and usually of PyCon UK),
i vote (mostly),
i go to PyCon UK when i can and it's not infectious.
I have even given talks and help my buddies with their workshops.
I follow enough Python people that i usually hear about most things.

Maybe if you're Senior Python Fellow in charge of
making sure JP Morgan write another cheque and
PyPI is your world then this is a pretty big deal and
its benefits seem obvious and ineluctable.

I pretty much login in to PyPI only when i update PyPNG and that's
every 7 months on average.
I have no idea what my password is so if my browser doesn't know,
i'll reset via e-mail (this isn't a bad thing, i treat a lot of
websites that way).
If i have a new computer since my last upload or my API key is
missing for some other reason, i'll generate a new one.

I have 2FA in my life, for fairly important things.
Like e-mail, and my NHS records.

But PyPI? I use 3 different web-accessible version control systems, and
you're less important than those.

I woudn't be surprised if i make one more PyPI release and then tell
people to `pip install` from gitlab after that.

# END
