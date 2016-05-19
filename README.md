# Measurements

[![Build Status](https://travis-ci.org/giordano/Measurements.jl.svg?branch=master)](https://travis-ci.org/giordano/Measurements.jl) [![Appveyor Build Status on Windows](https://ci.appveyor.com/api/projects/status/u8mg5dlhyb1vjcpe?svg=true)](https://ci.appveyor.com/project/giordano/measurements-jl) [![Coverage Status](https://coveralls.io/repos/github/giordano/Measurements.jl/badge.svg?branch=master)](https://coveralls.io/github/giordano/Measurements.jl?branch=master) [![codecov.io](https://codecov.io/github/giordano/Measurements.jl/coverage.svg?branch=master)](https://codecov.io/github/giordano/Measurements.jl?branch=master)

Introduction
------------

This package defines a new data type, `Measurement`, that allows you to enter a
quantity with its uncertainty and
[propagate errors](https://en.wikipedia.org/wiki/Propagation_of_uncertainty)
when performing mathematical operations involving `Measurement` objects.

For those interested in the technical details of the package, `Measurement` is a
[composite](http://docs.julialang.org/en/stable/manual/types/#composite-types)
[parametric](http://docs.julialang.org/en/stable/manual/types/#man-parametric-types)
type, whose definition is:

``` julia
immutable Measurement{T<:Number}
    val::T # The value
    err::T # The uncertainty, assumed to be standard deviation
end
```

Users that want to hack into `Measurements.jl` should use objects with type that
is a subtype of `Number`.

Installation
------------

`Measurements.jl` is available for Julia 0.4 and later versions, and can be
installed with
[Julia built-in package manager](http://docs.julialang.org/en/stable/manual/packages/).
In a Julia session run the command

```julia
julia> Pkg.add("Measurements")
```

You may need to update your package list with `Pkg.update()` in order to get the
latest version of `Measurements.jl`.

Usage
-----

After installing the package, you can start using it with

```julia
using Measurements
```

Many mathematical operations are redefined to accept `Measurement` type.

Examples
--------

``` julia
using Measurements
a = Measurement(4.5, 0.1)
# => 4.5 ± 0.1
b = Measurement(3.8, 0.4)
# => 3.8 ± 0.4
2a + b
# => 12.8 ± 0.4472135954999579
a - 1.2b
# => -0.05999999999999961 ± 0.49030602688525043
l = Measurement(0.936, 1e-3);
T = Measurement(1.942, 4e-3);
P = 4pi^2*l/T^2
# => 9.797993213510699 ± 0.041697817535336676
c = Constant(4)
# => 4 ± 0
a*c
# => 18.0 ± 0.4
sind(Measurement(94, 1.2))
# => 0.9975640502598242 ± 0.0014609761696991563
```

`±` is defined as an alias for the `Measurement` constructor, so you can simply
define a new quantity with uncertainty with this syntax:

``` julia
using Measurements
x = 5.48 ± 0.67
# => 5.48 ± 0.67
y = 9.36 ± 1.02
# => 9.36 ± 1.02
log(2x^2 - 3.4y)
# =>  3.3406260917568824 ± 0.5344198747546611
atan2(y, x)
# => 1.0411291003154137 ± 0.07141014208254456
```

How Can I Help?
---------------

Have a look at the TODO list below, feel free to implement those features and
send a pull request.  In addition, you can instruct more mathematical functions
to accept `Measurement` type arguments.  Bug reports and wishlists are welcome
as well.

TODO
----

* Add pretty printing: optionally print only the relevant significant digits
* Add support for correlation, so that `x-x == zero(x)`, `x*x == x^2`, `tan(x)
  == sin(x)/cos(x)`, etc...
* Extend to generic functions, also those not taking `Measurement` type
  arguments.  This should be possible with a macro like `@macroname
  any_function(4.3 ± 0.4)`.  This calculates the value of `any_function(4.3)`
  and the approximated uncertainty using numerical derivatives or so, and
  finally construct the `Measurement` object `any_function(4.3) ± uncertainty`
* Other suggestions welcome `:-)`

License
-------

The `Measurements.jl` package is licensed under the MIT "Expat" License.  The
original author is Mosè Giordano.
