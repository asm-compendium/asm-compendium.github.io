---
weight: 7
title: Counters in the FPU
type: docs
# prev: docs/
# next: docs/folder/
---

<style>
  .side-by-side {
    display: flex;
    gap: 10px;
    padding-top: 20px;
    padding-bottom: 20px;
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


When a counter is kept in the FPU, it can be modified and compared with FPU instructions. This comes with some overhead and limitations. It is only possible when the counter can be represented as a 32-bit float (25-bit signed int equivalent). Using a counter in the FPU is only faster when no CPU register values may be lost. While this is somewhat restrictive it has been used successfully in practice \cite{CHES:ACCEHHLNSWY21}. The code below shows an example of how this can be used, the performance comparison is shown in the following table.

#### Decrementing on the FPU. Note that the `vsub` can not be placed directly before `vcmp` without delays.
<div style="margin-top: -20px;"></div>
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
sample_a:   
  mov  r0, #10 // Loop count
  vmov s0, r0
  vcvt.f32.s32 s0, s0
  vmov.f32 s1, #1.0
  loop:
    vsub.f32 s0, s0, s1 
    add r2, #1
    // loop control
    vcmp.f32 s0, s1
    vmrs APSR_nzcv, FPSCR
    bge loop
  bx lr
```
Decrementing on the CPU. r0 needs to be spilled and recovered as we are out of registers.
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
sample_b:   
  mov r0, #10 // Loop count
  vmov s0, r0
  loop:
    add r2, #1
    // loop control
    vmov s1, r0
    vmov r0, s0
    subs r0, #1
    vmov s0, r0
    vmov r0, s1     
    bne loop
  bx lr

```
Example of looping ten times using a decrementing counter in the FPU and CPU. 
  </div>
</div>

#### Performance comparison of loop control based on a counter stored in a FPU or CPU register.
<div style="margin-top: -10px;"></div>

| Example:                |  sample a  | sample b |
|------------------------|------------------------------|------------------------------|
| **Instructions executed** | 52                          | 72                          |
| **LSU count**           | 0                            | 0                            |
| **CPI count**           | 9                            | 9                            |
| **Fold count**          | (-) 0                        | (-) 0                        |
| **Cycle count**         | 63                           | 81                           |
