[brutal]: #title "using Go for Advent of Code"
[brutal]: #author "David Jones"

So I did a little bit of Advent of Code this year, for the first
time.

It's quite fun.

I took this as an opportunity to rekindle some Go.
Go is a great language and I really loved the opportunity
at SensibleCode.io to learn it and use it in anger.

Go is a good match for the my personal knowledge.
Go syntax is mostly C-like.
The syntax for types is reversed,
so the ordering of elements is more similar to Pascal than C,
but I mostly cope with that.
Go's type system is very similar to C's,
but mostly typesafe.

For the sort of programming puzzle problems that appear in
Advent of Code I would say that Go is pretty well suited.

Go has _maps_ (like perl's hashes, Python's dicts, or awk's
associative arrays), and these are incredibly useful.
Go's maps are homogeneously typed,
meaning that in the _key_ to _value_ mapping,
all the values have to be the same type
(so do all the keys, but they are quite often all _strings_).
In practice most of time even in more dynamic langauges like
Python a map (or dict in Python) will have the values be all the
same type, so it's not the restriction is not too bad.

Go has _slices_, which are basically stretchy vectors, and
again, these are incredible useful.
Again, like maps, Go's slices are homogeneously typed.
You can have a slice of ints or a slice of strings but you can't
have a slice of anything you like
(actually, you can, it's just a bit more awkward).

Go doesn't have sets, and had it had them then there were at
least a couple of times that I would have used them.
Instead you either have to use a `map[string]bool`
or `map[string]struct{}`.
Curiously the jury still seems to be out on which type
the journeyman Go programmer should use for a set,
but at least `map[string]struct{}` is more honest.
So, not having sets is a minor wrinkle, but that's all.

Go is pretty fast, and that was helpful for at least a couple of
the days of advent of code.
Day 24 involves a fairly exhaustive longest graph-traversal
scenario,
and using Go means that we can code it fairly naively and it
still finishes in reasonable time.
