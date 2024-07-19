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

<table>
<tr><td style="width: 50%;">

```verilog {filename="sample a"}

    ldr.w r1, [r0, #4]! //LSU +2
    ldr.w r2, [r0, #4]! //LSU +2
    ldr.w r3, [r0, #4]! //LSU +2
    ldr.w r4, [r0, #4]! //LSU +2
```

</td><td style="width: 100%;">

```verilog {filename="sample b"}
    ldr.w r1, [r0, #4]  //LSU +1
    ldr.w r1, [r0, #4]! //LSU +1
    ldr.w r2, [r0, #4]! //LSU +1
    ldr.w r3, [r0, #4]! //LSU +1
    ldr.w r4, [r0, #4]! //LSU +2
```

</td></tr>
</table>

<table style="width: 100%; border-collapse: collapse;">
  <thead>
    <tr>
      <th style="border: none; padding: 8px; text-align: left;">Header 1</th>
      <th style="border: none; padding: 8px; text-align: left;">Header 2</th>
      <th style="border: none; padding: 8px; text-align: left;">Header 3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: none; padding: 8px;">Data 1</td>
      <td style="border: none; padding: 8px;">Data 2</td>
      <td style="border: none; padding: 8px;">Data 3</td>
    </tr>
    <tr>
      <td style="border: none; padding: 8px;">Data 4</td>
      <td style="border: none; padding: 8px;">Data 5</td>
      <td style="border: none; padding: 8px;">Data 6</td>
    </tr>
  </tbody>
</table>

