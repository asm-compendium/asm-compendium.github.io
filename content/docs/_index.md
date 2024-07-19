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
| Status | Response  |
| ------ | --------- |
| 200    |<pre lang="verilog">ldr.w r1, [r0, #0x4]! //LSU +2&#13;ldr.w r1, [r0, #0x4]! //LSU +2&#13;ldr.w r1, [r0, #0x4]! //LSU +2</pre>|
| 400    |Some text here|

<table><tr><td>

```verilog {filename="sample b"}
    ldr.w r1, [r0, #4]  //LSU +1
    ldr.w r1, [r0, #4]! //LSU +1
    ldr.w r2, [r0, #4]! //LSU +1
    ldr.w r3, [r0, #4]! //LSU +1
    ldr.w r4, [r0, #4]! //LSU +2
```
</td></tr><tr><td>

```verilog {filename="sample b"}
    ldr.w r1, [r0, #4]  //LSU +1
    ldr.w r1, [r0, #4]! //LSU +1
    ldr.w r2, [r0, #4]! //LSU +1
    ldr.w r3, [r0, #4]! //LSU +1
    ldr.w r4, [r0, #4]! //LSU +2
```
</td></tr>
</table>

<table style="width: 50%; border-collapse: collapse;">
    <tr>
        <td style="padding: 15px; text-align: left;">

```verilog {filename="sample a"}
ldr.w r1, [r0, #4]  //LSU +1
ldr.w r1, [r0, #4]! //LSU +1
ldr.w r2, [r0, #4]! //LSU +1
ldr.w r3, [r0, #4]! //LSU +1
ldr.w r4, [r0, #4]! //LSU +2
```
        </td>
        <td style="padding: 15px; text-align: left;">

```verilog {filename="sample b"}
ldr.w r1, [r0, #4]  //LSU +1
ldr.w r1, [r0, #4]! //LSU +1
ldr.w r2, [r0, #4]! //LSU +1
ldr.w r3, [r0, #4]! //LSU +1
ldr.w r4, [r0, #4]! //LSU +2
```
        </td>
    </tr>
</table>

# Simple HTML Table with Two Cells Side by Side

<table style="width: 50%; border-collapse: collapse;">
    <tr>
        <td style="padding: 15px; text-align: left;">
            
```verilog {filename="sample b"}
    ldr.w r1, [r0, #4]  //LSU +1
    ldr.w r1, [r0, #4]! //LSU +1
    ldr.w r2, [r0, #4]! //LSU +1
    ldr.w r3, [r0, #4]! //LSU +1
    ldr.w r4, [r0, #4]! //LSU +2
```</td>
        <td style="padding: 15px; text-align: left;">
            
```verilog {filename="sample b"}
    ldr.w r1, [r0, #4]  //LSU +1
    ldr.w r1, [r0, #4]! //LSU +1
    ldr.w r2, [r0, #4]! //LSU +1
    ldr.w r3, [r0, #4]! //LSU +1
    ldr.w r4, [r0, #4]! //LSU +2
```</td>
    </tr>
</table>
