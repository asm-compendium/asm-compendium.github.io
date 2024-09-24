---
weight: 5
title: SEL based conditional move
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


The `sel` instruction is meant to be used together with DSP instructions, to move selected bytes from two registers based on APSR GE flags set. These flags are located in bits[19:16] of the Program Status Register (PSR). Using the `msr` instruction, flags can be overwritten to all ones or zeros, Which means the `sel` instruction can be used to conditionally move between registers. As we are interested in setting all apsr\_ge bits at once and the other bits are read zero right ignored, there is no need to set the specific value as long as those bits[19:16] are set.



#### Comparison of tactics to perform a conditional move based on the sign bit of r0.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
    mov r1, #1
    mov r2, #2
    mov r3, #3
    mov r4, #4
        
    cmp r0, #0 //check sign 
    itt lt
    movge r1, r3
    movge r2, r4
    bx lr


```
Conditional execution using the `it` instruction
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
sample_b:
    asr r0, #31 // expand sign bit
    msr APSR_g, r0 // set all bits
    
    mov r1, #1
    mov r2, #2
    mov r3, #3
    mov r4, #4
    
    sel r1, r3, r1
    sel r2, r4, r2
    bx lr
```
Conditional execution using the `sel` instruction.
  </div>
</div>


While conditional moves can also be performed in constant time using the `it` instruction, using the `sel` instruction is faster in some situations. `it` instructions enable the conditional execution of four instructions. Whereas `sel` has an unlimited range as long as the flags are not overwritten. Thus when you need multiple blocks the difference already shrinks. This is exaggerated by loops due to the loop control overwriting the conditional flags, requiring the flags to be set in every loop iteration. The example shows a comparison between a conditional move based on the `sel` instruction and the `it` instruction. This is a minimal example for readability, the assembly in sample b is one cycle slower as the `it` instruction can be folded into the `cmp` in this case. However, this has been successfully implemented in BIKE, the authors of a recent [paper](https://eprint.iacr.org/2021/493) note that it provided about a 5\% increase in performance over their previous `it` instruction based code.