# Why Does Julia Wrap on Integer Overflow, Isn't that a Problem?

Fixed-width integers represent a finite range, which can lead to
unexpected behavior if these numbers are thought of as satisfying
their infinite mathematical ideal.  *Overflow* occurs when an
fixed-width integer operation yields a value that is outside of its
range.

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

## Isn't Wrapping Dangerous (learning from C)?

Yes, wrapped overflow can be a source of software errors, but
languages vary in the frequency and severity of such errors, so this
question deserves a careful examination.

Integer overflow bugs are perhaps most studied and best understood as
they occur in the C programming language.  Also, the concerns about
Julia's integer overflow behavior are often grounded in this C
experience.  So this response will first describe some of the pitfalls
of C overflow behavior to provide a context for an evaluation of the
issue for Julia.

*provide citations--from C references below, work out mechanics.*

### Mechanisms, Contexts of Serious Integer Overflow Bugs

Untrapped integer overflow is especially likely to cause serious
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

Unlike C, Julia objects are dynamically typed...(*overflow less prevalent*)

By default Julia array accesses are bounds checked and memory
management management is handled automatically, removing the most
serious errors resulting from integer overflow.  Also, loops are often
constructed from ranges or enumerations, rather than...

## What are the Advantages of Wrapped Overflow?

### In Some Cases, Wrapped Overflow is Exactly Correct

There are some computations in which wrapping upon overflow gives
exactly the right result...*cite Dietz results for prevalence and
types of intentional overflow*

### Wrapping is Liable to be More Efficient

  * Overflow wrap code is simpler than trap code and should run faster
  * The conditionals (or LLVM intrinsics calls) of trapped code are
    likely to hamper optimization.
  * Conditionals may also interfere with vectorization
  * It's difficult to benchmark wrapped versus trapped coding.  (*Some
    experience with C hints that the performance penalty of
    exhaustively trapped code may be several tens of percent, provide
    citation.*)

## How Can One Cope with Wrapped Overflow? 

  * `Base.checked_` arithmetic functions
  * use precondition and postcondition (domain and range) checks
    outside of computational kernels to provide safety with minimal
    performance impact.  (C experience indicates that such tests can
    be optimization friendly.)
  * Document intentional use of wrapped integer overflow behavior and
    provide tests of functions that trigger this overflow.
  * Use `Blind` when appropriate to provide truly "infinitely ranged"
    integers.  (Though performance may be an issue.)
  * Think carefully about overflow when writing code that will process
    untrustworthy input and code that will process long-lived or
    high-volume data streams.  Overflow is liable to occur in these
    situations.
  * Don't assume that `Int === Int64` if having the range of `Int64`
    is important to avoiding overflow behavior.  There are 32-bit
    architectures in use (e.g. embedded systems).

## Are there any Alternatives to Simple Trap or Wrap?

*This is a very speculative section.  Does it belong in a FAQ entry?*

  * Pragma to select appropriate overflow response (*a la Rust*)
  * Optimization friendly overflow trapping (*perhaps the performance
    penalty can be overcome by delayed trapping and trapping in
    aggregate to foster vectorization*)
  * Option of "As-If Infinitely Ranged" overflow response.


## References

*Not yet formalized.  Some of my sources.*

  * [Understanding Integer Overflow in C/C++](http://llvm.org/pubs/2012-06-08-ICSE-UnderstandingIntegerOverflow.html)
  * [CWE-190: Integer Overflow or Wraparound](https://cwe.mitre.org/top25/#CWE-190)
  * [Seacord on Integer Security](http://ptgmedia.pearsoncmg.com/images/0321335724/samplechapter/seacord_ch05.pdf)
  * [Safe Integer Arithmetic in C](http://blogs.msdn.com/b/michael_howard/archive/2006/02/02/523392.aspx)
  * [rust-lang RFC #560, Integer Overflow](https://github.com/rust-lang/rfcs/blob/master/text/0560-integer-overflow.md)
  * [As if Infinite Ranged Integer Model](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=9299)

  
