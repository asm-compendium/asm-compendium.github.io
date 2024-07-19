---
title: Barrel Shifter
type: docs
prev: docs/
# next: docs/folder/
---

Moving a `ror` operation from it's own instruction into the add instruction by using the inline barrel shifter. 

<div class="side-by-side">
  <div class="box">

```verilog {filename="sample a"}
   mov r0, #10
   mov r1, #12
   ror r1, #2
   add r0, r1, r1 
```
cycles: 4
  </div>
  <div class="box">

```verilog {filename="sample b"}
   mov r0, #10
   mov r1, #12
   add r0, r1, r1, ror #2
```
cycles: 3
  </div>
</div>