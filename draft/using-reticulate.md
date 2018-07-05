    install.package("reticulate")

    library("reticulate")
    py_run_string("x=[1,2,3]")

This is fun.
(_Python reticulatus_ is a species of Python,
and I'm sure this inspired the library name)

So we can make a vector in R:

    v = c(2,3,5)

and we can access it, via the Python global variable `r`, from
Python:

    py_run_string("print(r.v)")
    py_run_string("print(r.v[0])")

Several things to note:
- The R vector has become a Python list;
- In Python the numbers are floats;
- Python has 0-based indexing, even when accessing an R object.

An R vector isn't exactly the same as a Python list,
but they are similar enough.
That an R vector becomes a Python list follows
the Principle of Least Astonishment.
This principle will re-appear later.

Naturally, Python objects can be accessed from R,
this time via the R global variable `py`:

    py_run_string("l = [3, 5, 8]")
    py$l
