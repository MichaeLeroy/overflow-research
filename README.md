# Why Does Julia Wrap on Integer Overflow, Isn't that a Problem?

Fixed-width integers represent a finite range, which can lead to
unexpected behavior if these numbers are thought of as their infinite
mathematical ideal.  *Overflow* occurs when an fixed-width integer
operation yields a value that is outside of its range.

The two most common ways for a computing language to respond to an
integer overflow are to *trap* or to *wrap*.  If overflow is trapped,
the system throws an error (or more generally invokes a signal
handler) whenever overflow occurs.  When wrapping is used, the
otherwise unrepresentable overflowed value is converted, modulo the
integer's range, into a representable value.  Julia wraps `Signed` and
`Unsigned` integers upon
[overflow](http://docs.julialang.org/en/stable/manual/integers-and-floating-point-numbers/#overflow-behavior).

The balance of this FAQ entry addresses concerns over the safety of
wrapped overflow, describes some of its advantages, offers tips for
dealing with overflow, and looks at some alternatives to simple trap
or wrap.

## Isn't Wrapping Dangerous (the C Experience)?

Yes, it can be, but the extent and severity of such dangers depend
strongly upon the language being used.  C is prone to serious bugs
stemming from integer overflow, and this issue has been well studied
for C (*provide citations--some from references below, work out
mechanics*).  Consider C and integer overflow to provide a context for
an evaluation of the issue for Julia.

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

### Julia is not C, Wrapping is much less Dangerous.

Julia is not C.  The likelihood of unintended integer overflow is
somewhat reduced.  And, given an overflow, the mechanisms for serious
bugs are strongly mitigated in Julia Code.

Unlike C, Julia objects are dynamically typed...

By default Julia array accesses are bounds checked and memory
management management is handled automatically, removing the most
serious errors resulting from integer overflow.  Also, loops are often
constructed from ranges or enumerations, rather than

## What are the Advantages of Wrapped Overflow?

### In Some Cases, Wrapped Overflow is Exactly Correct

There are some computations in which wrapping upon overflow gives
exactly the right result...*cite Dietz results for prevalence and
types of intentional overflow*

### Wrapping is Liable to be More Efficient

*lower overhead, better optimization and vectorization...*

*Checking each integer operation for overflow may be burdensome.  In
 particular, the required conditionals my inhibit optimization and
 vectorization.*

## How Can One Cope with Wrapped Overflow? 

*checked_ arithmetic functions, strategic checks outside of
 computational kernels...*

## Are there any Alternatives to Simple Trap or Wrap?



## References

  * [Understanding Integer Overflow in C/C++](http://llvm.org/pubs/2012-06-08-ICSE-UnderstandingIntegerOverflow.html)
  * [CWE-190: Integer Overflow or Wraparound](https://cwe.mitre.org/top25/#CWE-190)
  * [Safe Integer Arithmetic in C](http://blogs.msdn.com/b/michael_howard/archive/2006/02/02/523392.aspx)
  * [rust-lang RFC #560, Integer Overflow](https://github.com/rust-lang/rfcs/blob/master/text/0560-integer-overflow.md)
  * [As if Infinite Ranged Integer Model](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=9299)

  
