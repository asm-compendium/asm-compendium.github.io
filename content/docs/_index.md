---
title: Documentation
next: first-page
---

This is a demo of the theme's documentation layout.

## Hello, World!
```verilog {filename="main_2.go"}
.syntax unified
.cpu cortex-m4

.align 4
.global sample_a
.type sample_a, %function
sample_a:
    //9
    //Setup_assembly
    mov.w r0, sp
    sub.w r1, sp, #4
    str.w sp, [r1]
    mov.w r2, #-8
    mov.w r5, #-4
    mov.w r4, sp
    mov.w r8, r8
    mov.w r8, r8
    //Target_assembly
    ldr.w r3, [r1] //+1
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1] //+1
    //End_target_assembly
    mov.w r8, r8
bx lr

```

 ```cobol {filename="main_2.go"}
.syntax unified
.cpu cortex-m4

.align 4
.global sample_a
.type sample_a, %function
sample_a:
    //9
    //Setup_assembly
    mov.w r0, sp
    sub.w r1, sp, #4
    str.w sp, [r1]
    mov.w r2, #-8
    mov.w r5, #-4
    mov.w r4, sp
    mov.w r8, r8
    mov.w r8, r8
    //Target_assembly
    ldr.w r3, [r1] //+1
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1] //+1
    //End_target_assembly
    mov.w r8, r8
bx lr

```


 ```emacs {filename="main_2.go"}
.syntax unified
.cpu cortex-m4

.align 4
.global sample_a
.type sample_a, %function
sample_a:
    //9
    //Setup_assembly
    mov.w r0, sp
    sub.w r1, sp, #4
    str.w sp, [r1]
    mov.w r2, #-8
    mov.w r5, #-4
    mov.w r4, sp
    mov.w r8, r8
    mov.w r8, r8
    //Target_assembly
    ldr.w r3, [r1] //+1
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1] //+1
    //End_target_assembly
    mov.w r8, r8
bx lr

```

```fortran {filename="main_2.go"}
.syntax unified
.cpu cortex-m4

.align 4
.global sample_a
.type sample_a, %function
sample_a:
    //9
    //Setup_assembly
    mov.w r0, sp
    sub.w r1, sp, #4
    str.w sp, [r1]
    mov.w r2, #-8
    mov.w r5, #-4
    mov.w r4, sp
    mov.w r8, r8
    mov.w r8, r8
    //Target_assembly
    ldr.w r3, [r1] //+1
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1] //+1
    //End_target_assembly
    mov.w r8, r8
bx lr

```

```asm {filename="main_2.go"}
.syntax unified
.cpu cortex-m4

.align 4
.global sample_a
.type sample_a, %function
sample_a:
    //9
    //Setup_assembly
    mov.w r0, sp
    sub.w r1, sp, #4
    str.w sp, [r1]
    mov.w r2, #-8
    mov.w r5, #-4
    mov.w r4, sp
    mov.w r8, r8
    mov.w r8, r8
    //Target_assembly
    ldr.w r3, [r1] //+1
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1] //+1
    //End_target_assembly
    mov.w r8, r8
bx lr

```


```haskell {filename="main_2.go"}
.syntax unified
.cpu cortex-m4

.align 4
.global sample_a
.type sample_a, %function
sample_a:
    //9
    //Setup_assembly
    mov.w r0, sp
    sub.w r1, sp, #4
    str.w sp, [r1]
    mov.w r2, #-8
    mov.w r5, #-4
    mov.w r4, sp
    mov.w r8, r8
    mov.w r8, r8
    //Target_assembly
    ldr.w r3, [r1] //+1
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1] //+1
    //End_target_assembly
    mov.w r8, r8
bx lr

```





```hexdump {filename="main_2.go"}
.syntax unified
.cpu cortex-m4

.align 4
.global sample_a
.type sample_a, %function
sample_a:
    //9
    //Setup_assembly
    mov.w r0, sp
    sub.w r1, sp, #4
    str.w sp, [r1]
    mov.w r2, #-8
    mov.w r5, #-4
    mov.w r4, sp
    mov.w r8, r8
    mov.w r8, r8
    //Target_assembly
    ldr.w r3, [r1] //+1
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1]
    str.w r3, [r0, r2] //+1
    str.w r3, [r1] //+1
    //End_target_assembly
    mov.w r8, r8
bx lr

```
