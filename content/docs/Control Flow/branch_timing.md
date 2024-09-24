---
weight: 1
title: Branch timing
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


Branches are one of the instruction types that do **not** always take one cycle. The only type of branch that does take one cycle is a conditional branch that is not taken. Branches that are taken take two cycles and branching to an unaligned wide instruction takes three cycles. Examples of starting a loop on an unaligned instruction can be seen in the examples below. The table shows the performance impact, while both functions jump twice, and a CPI count of two is expected, sample a uses 3 additional CPI cycles due to starting the loop with an unaligned wide instruction.


#### Examples of unaligned branching to wide and narrow instructions.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
power:
    mov r2, r0
    mov r0, #1     
    loop:
        mul r0, r2 //02fb00f0
        sub r1, #1
        cmp r1, #0
        bne loop
    bx lr
```
Unaligned loop starting with a wide (32-bit) sinstruction $r0 = r0^{(r1)}$.
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
multiply:
    mov r2, r0
    mov r0, #1     
    loop:
        add r0, r2 //1044
        sub r1, #1
        cmp r1, #0
        bne loop
    bx lr
```
Unaligned loop starting with a narrow (16-bit) instruction $r0 = r0*r1$.
  </div>
</div>


#### Performance comparison of unaligned loops. Note that 2 and 3 were passed as arguments.
| Example                | sample a | sample b |
|------------------------|----------|----------|
| **Instructions executed** | 14        | 14        |
| **LSU count**             | 0        | 0        |
| **CPI count**             | 5        | 2        |
| **Fold count**            | (-) 0    | (-) 0    |
| **Cycle count**           | 19        | 16        |