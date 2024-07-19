---
title: Documentation
next: first-page
---

This is a demo of the theme's documentation layout.

## Hello, World!
```verilog {filename="sample a"}
    ldr.w r1, [r0, #0x4]! //LSU +2
    ldr.w r2, [r0, #4]! //LSU +2
    ldr.w r3, [r0, #4]! //LSU +2
    ldr.w r4, [r0, #4]! //LSU +2
```
```verilog {filename="sample b"}
    ldr.w r1, [r0, #4]  //LSU +1
    ldr.w r1, [r0, #4]! //LSU +1
    ldr.w r2, [r0, #4]! //LSU +1
    ldr.w r3, [r0, #4]! //LSU +1
    ldr.w r4, [r0, #4]! //LSU +2
```

<table style="width: 100%; border-collapse: collapse; border: none;"><tr><td style="width: 50%; border: none;>

```verilog {filename="sample b"}
    ldr.w r1, [r0, #4]  //LSU +1
    ldr.w r1, [r0, #4]! //LSU +1
    ldr.w r2, [r0, #4]! //LSU +1
    ldr.w r3, [r0, #4]! //LSU +1
    ldr.w r4, [r0, #4]! //LSU +2
```
</td><td>

```verilog {filename="sample b"}
    ldr.w r1, [r0, #4]  //LSU +1
    ldr.w r1, [r0, #4]! //LSU +1
    ldr.w r2, [r0, #4]! //LSU +1
    ldr.w r3, [r0, #4]! //LSU +1
    ldr.w r4, [r0, #4]! //LSU +2
```
</td></tr>
</table>

