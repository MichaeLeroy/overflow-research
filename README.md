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

## Isn't Integer Overflow Dangerous?

Yes, it can be, but the extent and severity of such dangers depend
upon the language being used.  C is prone to consequential integer
overflow bugs, and this issue has been well studied for C (*provide
citations--some from references below, work out mechanics*).  Consider
C and integer overflow to provide a context for a discussion of the
issue for Julia.

### Mechanisms, Contexts of Serious Integer Overflow Bugs

Untrapped Integer overflow is especially likely to cause serious
problems when it occurs in the context of:

  * Array Indexing
  * Object Size Calculations
  * Memory Allocation
  * Loop Indexing

A common problem with integer overflow in these contexts is that the
consequent corruption of size or location information leads to
inappropriate memory access or modification.

### Sources, Circumstances Prone to Integer Overflow

Some of the situations that may result in unintended integer overflow
are:

  * Casts to Integers
  * Credulous use of Untrustworthy Data
  * Counters and Accumulators of Open Ended Data Streams

In C, casts to integers can result in truncation or change of sign,
which have the characteristics of integer overflow.  Untrustworthy
input my provide false information about the sizes of objects or
provide objects of unexpected size, triggering overflow behavior.
Larger than expected or long-running data streams can cause counters or
accumulators to overflow.

## Julia is not C, and Integer Overflow is less Dangerous

Julia is not C.  The likelihood of unintended integer overflow is
somewhat reduced.  Even given an overflow, the mechanisms for serious
bugs are strongly mitigated in Julia Code.

By default Julia array accesses are bounds checked, s array access attempts and handles memory

management automatically, removing the most serious errors resulting
from integer overflow.  Also, loops are often constructed from ranges
or enumerations, rather than 

## Sometimes Overflow is Essential to Appropriate Program Behavior

*Hash calculations, bit manipulation*

## The Costs of Overflow Trapping

*Checking each integer operation for overflow may be burdensome.  In
 particular, the required conditionals my inhibit optimization and
 vectorization.*

## Managing Overflow

*checked_ arithmetic functions, strategic checks outside of
 computational kernels...*

## Room for Improvement

*configurable overflow response, smart overflow detection...*

## References

  * [Understanding Integer Overflow in C/C++](http://llvm.org/pubs/2012-06-08-ICSE-UnderstandingIntegerOverflow.html)
  * [CWE-190: Integer Overflow or Wraparound](https://cwe.mitre.org/top25/#CWE-190)
  * [Safe Integer Arithmetic in C](http://blogs.msdn.com/b/michael_howard/archive/2006/02/02/523392.aspx)
  * [rust-lang RFC #560, Integer Overflow](https://github.com/rust-lang/rfcs/blob/master/text/0560-integer-overflow.md)
  * [As if Infinite Ranged Integer Model](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=9299)

  
