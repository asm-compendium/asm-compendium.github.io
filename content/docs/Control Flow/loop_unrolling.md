---
weight: 2
title: Loop unrolling
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


Loop unrolling is a common optimization technique that trades code size for speed. As established in the previous subsection, a conditional branch takes a single cycle, and jumping when the condition is met takes an additional cycle regardless of pipeline state. Sample a shows a loop that will count to five, compare and branch both take one cycle. Jumping back to the loop label four times will take an additional four cycles, which will be caught by the CPI counter as shown in the table. Note that loops performing few operations are a worst-case scenario, due to the ratio of loop-control operations and productive operations. The example is meant to illustrate the overhead each iteration will have.

#### Comparison of a loop performing 5 operations and a rolled out loop doing so.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
count_to_five:
    mov r0, #0   
    
    loop:
        add r0, #1
        cmp r0, #5
        bne loop
    bx lr
    
```
A loop performing 5 operations.
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
count_to_five:
    mov r0, #0
    
    add r0, #1
    add r0, #1
    add r0, #1
    add r0, #1
    add r0, #1
    bx lr
```
Rolled out loop, 5 sequential operations.
  </div>
</div>

#### Performance comparison of a loop and sequential operations.
| Example                | Sample A | Sample B |
|------------------------|----------|----------|
| **Instructions executed** | 16        | 6        |
| **LSU count**             | 0        | 0        |
| **CPI count**             | 4        | 0        |
| **Fold count**            | (-) 0    | (-) 0    |
| **Cycle count**           | 20        | 6        |