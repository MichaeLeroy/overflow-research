# overflow-research
Research of Integer overflow to be incorporated into a Julia FAQ entry

## The Issue

Fixed-width integers represent a finite range, which can lead
to unexpected behavior if these numbers are thought of as their
infinite mathematical ideal.  *Overflow* occurs when an integer is
increased beyond its maximum representable value.  

Several common ways for a computing language to respond to an integer
overflow are to:

  * have no defined behavior
  * throw an error
  * wraparound, returning the proper value modulo the integer range

Julia generally does wraparound on overflow.  This modulo arithmetic
approach is coherent and performant.  , but it may be unexpected and,
thus, can be a source of bugs.  Some would prefer that any overflows
call attention to themselves by throwing an error, giving the
developer or user an opportunity to respond appropriately.

## Performance Advantages of Wraparound

*lower overhead, better optimization and vectorization...*

## Isn't Wraparound on Overflow Dangerous?

Integer overflow can be a source of serious errors in C and related
languages, raising understandable concerns about the consequences of
integer overflow in general.

At the root of the most serious risks posed by integer overflow is the
key role that integer arithmetic plays in memory (and array)
allocation and access.  Overflow behavior can corrupt position and
size calculations, resulting in damaging buffer overflows.
Particularly troublesome are signed integer overflows, which are left
having undefined behavior according to C standards.

## Julia Overflow is less Dangerous than One Might Think.

Julia bounds checks array access attempts and handles memory
management automatically, removing the most serious errors resulting
from integer overflow.  Also, loops are often constructed from ranges
or enumerations, rather than 

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
  

  
