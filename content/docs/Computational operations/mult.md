---
weight: 2
title: Constant time multiplication instructions
type: docs
# prev: docs/
# next: docs/folder/
---
<style>
  .side-by-side {
    display: flex;
    gap: 10px;
    padding-top: 20px;
    padding-bottom: 10px;
  }
  .box {
    flex: 1;
    border: none;
    box-sizing: border-box;
  }
  @media (max-width: 400px) {
            .side-by-side {
                flex-direction: column;
            }
        }
</style>
Cortex M4 processors have access to a large variety of specialized multiplication instructions. And in contrast to the M3, all these instructions are constant time.

Due to the Digital Signal Processing (DSP) extension, some of these multiplication instructions are quite specific.
An example of such an instruction is `smulwt`, multiplying the lower 16 bits of one register with all bits of another register and discarding the lower 16 bits of the result. Unfortunately, there is no modular multiplication instruction but an exhaustive list of multiplication instructions can be found in the Architecture Reference Manual LINK.

Two generally useful instructions are `umull` and `umlal`. `umull` performs an unsigned 32-bit multiplication with a 64-bit result. `umlal` performs an unsigned 32-bit multiplication and adds its result to a 64-bit number. This is especially impressive as `umlal` takes one cycle and the addition alone would take longer. The code blocks below show a C implementation of the Pythagorean Theorem \(A^2 + B^2 = C^2\) where `A` and `B` are unsigned 32-bit integers and \(C^2\) is an unsigned 64-bit integer to avoid overflow.

<div class="side-by-side">
  <div class="box">

```C {filename="pythagorean.c"}
#include <stdint.h>

uint64_t pythagorean(uint32_t a, uint32_t b) {
    return (a * (uint64_t)a) + (b * (uint64_t)b);
}
```
  </div>
  <div class="box">

```verilog {filename="pythagorean.s"}
pythagorean:
    mov     r3, r0
    umull   r0, r1, r1, r1
    umlal   r0, r1, r3, r3
    bx      lr
```
  </div>
</div>

`pythagorean.s` shows an assembly implementation that would just take three cycles, ignoring the function call overhead. Implementing large multiplications in constant time without these instructions, such as would be necessary on an M3, is much slower. While there are some tricks one can use ([BearSSL constant-time multiplication](https://www.bearssl.org/ctmul.html)
), these come with compromises in precision or are not constant time in edge cases. A naive constant time implementation based on the 64-bit multiplication function from the ARM Cortex-M0 runtime ABI is used for comparison. It works by splitting `A` and `B` in 16-bit parts, multiplying and accumulating those results.

| Version                 | pythagorean.s | Naive assembly implementation |
|-------------------------|-----------------------------------------------------|-------|
| **Instructions executed**| 3                                                   | 56    |
| **LSU count**            | 0                                                   | 0     |
| **CPI count**            | 0                                                   | 0     |
| **Fold count**           | (-) 0                                               | (-) 0 |
| **Cycle count**          | 3                                                   | 56    |


