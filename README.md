# Papers
Proposals for standardisation
## P4296 Default-Deny + Provable-Whitelist Invalidation
Profile invalidation is one of the most critical (and most controversial) parts of the overall safety profiles effort. We’ll describe our “default-deny + provable-whitelist” proposal for implementing profile invalidation. Very briefly, we aim to be provably-safe from the outset, while reducing the amount of false positives by adding “we can prove it is safe” rules aiming specific provably-safe patterns in existing C++ code. We’re sure that in this way it is possible to achieve fewer false positives than with borrow-checker approaches, gradually approximating the “Holy Grail” of the safety profiles where all “good” code will be free from false positives. As an implementation detail of the provable-whitelist part of it, we want to exploit the existing C++ type system (including strict aliasing) to eliminate some of the common invalidations from the very beginning - and without the need to annotate.
## P3375 Reproducible floating-point results
The C++ standard, hereafter referred to as [C++], has very little to say about floating-point specification, which means that it is extremely hard to write floating-point expressions that, given the same inputs and rounding mode, will produce the same results on all implementations. This paper proposes the introduction of a new set of functions which specifies sufficient conformance with the provisions specified in ISO/IEC 60559:2020 (which is itself imported from IEEE Std 754-2019, unmodified except for editorial adjustment), hereafter referred to as [60559], to guarantee this reproducibility.
## P3864 Correctly rounded floating-point maths functions
Floating-point maths functions are rounded after calculation. The rounding mode used is part of the floating-point state which is maintained per-thread. This introduces two problems:

- Changing rounding modes, for example for calculations that require correct rounding in a set of optimized expression evaluations, is unergonomic.
- There is a burden on library writers to restore the floating-point state to the condition it was in when a library function was called.

This is not a new problem, and the C standard in Annex F reserves the prefix cr_ for functions fully matching the [60559:2020] mathematical operations; see 7.33.8 [Mathematics <math.h>]:

Function names that begin with cr_ are potentially reserved identifiers and may be added to the <math.h> header. The cr_ prefix is intended to indicate a correctly rounded version of the function.

This paper proposes adding five overload sets to the standard library for addition, subtraction, multiplication, division and square root calculation.

These functions guarantee that the operation will be carried out as if with infinite precision, and rounded using the roundTiesToEven rounding mode, as specified in [60559:2020]. The parameter type must satisfy the type trait std::numeric_­limits<T>::­is_iec559.

This solves problem 1 directly by providing a more ergonomic way of expressing intention: the client can explicitly state that they require correctly rounded calculation without having to ensure that the floating-point state is set appropriately.

This does not solve problem 2 directly, since this does not necessarily affect the floating-point state at all. However, it reduces the sources of error when correct rounding is important.
## P4212 ISO/IEC 60559 Floating-Point Support Annex for C++
This paper proposes a new normative Annex specifying support for floating-point arithmetic as defined by [559]. The C standard provides a similar annex, but it is not directly suitable for C++ due to differences in language semantics, constant evaluation, templates, and the standard library. This proposal integrates [559] semantics into C++ in a manner consistent with existing language rules and implementation practice.