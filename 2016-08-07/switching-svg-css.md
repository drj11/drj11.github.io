[brutal]: #title "Switching CSS in SVG"
[brutal]: #author "David Jones"
[brutal]: #date "2016-08-07"

SVG, stands for Scalable Vector Graphics,
is a way of making graphics from lines and curves and stuff
(as opposed to pixels, which is how JPEG and PNG and things work).

It's great.
I use Inkscape to maintain a scale drawing of my back garden in SVG:

<img src="https://drj11.github.io/house/rear-garden.svg">

That version is in colour.

For planning the garden,
i like to
print out the scale drawing
and draw on it with pen and pencil.
But for that, a black and white outline is more useful.

I'd like to be able to define a second CSS stylesheet,
and switch between the two.
Fun fact: you can style SVG using CSS, just like you can HTML.
Fun fun fact: this works because HTML and SVG are both XML based.

I didn't find a good way to do this, but I did find a way.

I can create an HTML page
which includes the SVG via an `OBJECT` tag.
I can then run some JavaScript
which fetches the DOM on the SVG object
and modifies the SVG to add a `style` element.

[Here is the vector outline
version](https://drj11.github.io/house/rear-garden-outline.html).

Notes:

- This is technically quite an obscure corner of the web.
I suspect that there are better approaches.
If you find one, please write it up.

- There is a Flash Of Unstyled Content (FOUC).
You sometimes see a flash of colour,
which is actually the original syle,
not unstyled content.

- The HTML document and the SVG each have DOMs,
and they are separate.
In particular, adding style to the HTML document
does not style the SVG document.

- SVG really is XML, so when adding a `style` tag,
you have to get the case right, and the namespace right.
