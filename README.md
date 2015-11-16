# overflow-research
Research of Integer overflow to be incorporated into a Julia FAQ entry

## The Issue

Fixed-width integers represent a finite range and that fact can lead
to unexpected behavior if these numbers are thought of as their
infinite mathematical ideal.  *Overflow* occurs when an integer is
increased beyond its maximum representable value.  (Though decreasing
an integer below its minimum representable value is an *underflow*,
the term *overflow* will be used here to generically refer to either
situation.)

Two common ways for a computing language to respond to an integer
overflow are to:

  * throw an error
  * wraparound, returning the proper value modulo the integer range

Julia generally does wraparound on overflow.  This modulo arithmetic
approach is coherent and performant, but it may be unexpected and,
thus, can be a source of bugs.  Some would prefer that any overflows
call attention to themselves by throwing an error, giving the
developer or user an opportunity to respond appropriately.

## Performance Advantages of Wraparound

*lower overhead, better optimization and vectorization...*

## Isn't Wraparound on Overflow Dangerous?

*CWE-190 and friends...*

## Julia Overflow is less Dangerous than in C/C++

*checked array bounds and memory management...*

## Managing Overflow

*checked_ arithmetic functions, strategic checks outside of
 computational kernels...*

## Room for Improvement

*configurable overflow response, smart overflow detection...*

## References

  * [Understanding Integer Overflow in C/C++](http://llvm.org/pubs/2012-06-08-ICSE-UnderstandingIntegerOverflow.html)
  * [CWE-190: Integer Overflow or Wraparound](https://cwe.mitre.org/top25/#CWE-190)
  * [Safe Integer Arithmetic in C](http://blogs.msdn.com/b/michael_howard/archive/2006/02/02/523392.aspx)
  * [rust-lang RFC #560, Integer Overflow](https://github.com/rust-lang/rfcs/pull/560)
  

  
