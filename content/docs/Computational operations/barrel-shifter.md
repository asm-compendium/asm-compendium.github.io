---
weight: 1
title: Barrel Shifter
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
The inline barrel shifter feature, also known as the constant shift feature, allows to
optionally shift the Rn operand for most instructions before it is processed by the
main instruction. This is a hallmark feature of ARM processors and is widely used in
optimization by programmers and compilers alike.

When an instruction includes a shift to its Rn operand, this is evaluated first.

![](/images/shift.png "Schematic view of an instruction using the barrel shifter.")

For example `add r1, r2, r3 ror #0x10` should be interpreted as `r1 = r2 + ror(r3, 0x10)`.

#### Comparison between using separate bit-shifting instructions and using the barrelshifter.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample a"}
sample_a:
    ror r3, #0x10
    lsr r4, #8
    add r1, r2, r3
    orr r1, r4
    bx lr
```
Discrete rotate and shift instructions.
  </div>
  <div class="box">

```verilog {filename="sample b"}
sample_b:
    add r1, r2, r3, ror #0x10
    orr r1, r1, r4, lsr #8
    bx lr
```
Rotate and shift instructions using the barrel shifter.
  </div>
</div>

The example above depicts the same calculation performed with and without the use of the
barrelshifter. Shifting an operand does not add any cycles, thus it can be used to
eliminate the cycles used by shift instructions. The performance impact can be seen in the table below.
#### Benchmark results of bit-shifting examples
| Figure                  | sample a  | sample b  |
|-------------------------|----------------------|----------------------|
| **Instructions executed**| 4                    | 2                    |
| **LSU count**            | 0                    | 0                    |
| **CPI count**            | 0                    | 0                    |
| **Fold count**           | (-) 0                | (-) 0                |
| **Cycle count**          | 4                    | 2                    |

