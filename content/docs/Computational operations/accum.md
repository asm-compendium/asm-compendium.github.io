---
weight: 3
title: Unsigned Accumulate Long
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

In a situation where many 32-bit numbers are added and accumulated as a 64-bit number, multiply accumulate instructions are also useful. The traditional way to add a 32-bit value to a 64-bit value is to use `adds` to add the 32-bit value to the least significant bits of the 64-bit value and `adc` to add the carry to the most significant bits (`accumulate_array_a.s`) as there is no specialized instruction to do this. We can turn `umlal` into such an operation by using a multiplication of 1; improving performance and size, at the cost of using an extra register (`accumulate_array_b.s`). The DSP extension also gives access to `umaal`, Unsigned Multiply Accumulate Accumulate Long, which is the ideal instruction to start this accumulation problem.

####  A comparison between accumulation in a traditional way and accumulation using the accumulation part of multiply accumulate instructions.
<div class="side-by-side">
  <div class="box">

```verilog {filename="accumulate_array_a.s"}
accumulate_array_a:
    ldm sp, {r2-r5} // Load 4 values
    mov r0, #0 //Init low bits
    mov r1, #0 //Init high bits
    
    adds r0, r3 //add r3 to r0
    adc  r1, #0 //add carry to r1
    adds r0, r4 //add r4 to r0
    adc  r1, #0 //add carry to r1
    adds r0, r5 //add r4 to r0
    adc  r1, #0 //add carry to r1
    bx lr
```
  </div>
  <div class="box">

```verilog {filename="accumulate_array_b.s"}
accumulate_array_b:
    ldm sp, {r0-r3} // Load 4 values
    mov r4, #1 // Set multiplier to 1
    
    umaal.w r0, r1, r2, r4
           // r0, r1 = r0 + r1 + r2*1
    umlal.w r0, r1, r3, r4
           // r0, r1 += r3*1
    bx lr
```
  </div>
</div>



The table below shows the benchmarks of these functions. The loading of values was included to make the example more readable, when ignoring the cycles used by `ldm` and thus only looking at setup and accumulation, sample a would take 8 cycles and sample b only 3 cycles.
#### Performance comparison of accumulating using carry and accumulating using multiplication instructions.
| Example: | sample a | sample b |
|-------------------------|------------------------------------------|------------------------------------------|
| **Instructions executed**| 9                                        | 4                                        |
| **LSU count**            | 5                                        | 5                                        |
| **CPI count**            | 0                                        | 0                                        |
| **Fold count**           | (-) 0                                    | (-) 0                                    |
| **Cycle count**          | 14                                       | 9                                        |


