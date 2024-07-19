---
title: Documentation
next: first-page
---

This is a demo of the theme's documentation layout.

## Not pipelinable load chain
Example assembly snipets:

<style>
  .side-by-side {
    display: flex;
    gap: 10px;
  }
  .box {
    flex: 1;
    border: none;
    box-sizing: border-box;
    padding-top: 20px;
    padding-bottom: 20px;
  }
  @media (max-width: 400px) {
            .side-by-side {
                flex-direction: column; /* Stack items vertically */
            }
        }
</style>

<div class="side-by-side">
  <div class="box">

```verilog {filename="sample a"}

    ldr.w r1, [r0, #4]! //LSU +2
    ldr.w r2, [r0, #4]! //LSU +2
    ldr.w r3, [r0, #4]! //LSU +2
    ldr.w r4, [r0, #4]! //LSU +2
```
  </div>
  <div class="box">

```verilog {filename="sample b"}
    ldr.w r1, [r0, #4]  //LSU +1
    ldr.w r1, [r0, #4]! //LSU +1
    ldr.w r2, [r0, #4]! //LSU +1
    ldr.w r3, [r0, #4]! //LSU +1
    ldr.w r4, [r0, #4]! //LSU +2
```
  </div>
</div>
