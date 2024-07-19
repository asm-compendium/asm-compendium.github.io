---
title: Barrel Shifter
type: docs
prev: docs/
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

The inline barrel shifter feature, also known as the constant shift feature, allows to optionally shift the Rn operand for most instructions before it is processed by the main instruction.

<div class="side-by-side">
  <div class="box">

```verilog {filename="sample a"}
   mov r0, #10
   mov r1, #12
   ror r1, #2
   add r0, r1, r1 
```
  </div>
  <div class="box">

```verilog {filename="sample b"}
   mov r0, #10
   mov r1, #12
   add r0, r1, r1, ror #2

```
  </div>
</div>

As this does not add any cycles to aritmatic instructions, it can be used to eliminate the cycles used by most shift instructions.
