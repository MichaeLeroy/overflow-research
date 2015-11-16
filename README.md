# overflow-research
Research of Integer overflow to be incorporated into a Julia FAQ entry

## The Issue

Fixed-width integers represent a finite range and that fact can lead
to unexpected behavior if these numbers are thought of as their
infinite mathematical ideal.  *Overflow* occurs when an integer is
increased beyond its maximum representable value.  Though decreasing
an integer below its minimum representable value is *underflow*, the
term *overflow* will be used here to generically refer to either
situation.
