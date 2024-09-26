---
weight: 9
title: Banking the stack pointer
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


As seen in the register overview on the [Register balancing]({{< relref "register.md" >}}) page, the stackpointer is bankable, this means that `sp` can either point to the Process Stack Pointer (PSP) or the Main Stack Pointer (MSP). Register banking is a common feature in the more powerful ARM architectures, such as ARM9 and AArch32. It is intended for exception or privilege separation but can be used to repurposes registers used as high-performance spillover. Unfortunately, this architecture only has a banked stack pointer, limiting the possible uses. On the M4, register banking is generally used by embedded operating systems to make sure the stack of the operating system and the stack of user processes are separated. It takes two cycles to bank the stack pointer and a register containing a specific value and still comes with the issues discussed in the previous subsection, limiting its viability in most cases. One way to use this would be to change the stack to a CCM address for a specific function or to back up the stackpointer before overwriting it with other data. The examples below illustrate the usage of banking.

#### Comparison of tactics to interchange the value in a register with a saved value.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
sample_a:
    //Spill r1 and load value
    str.w r1, [sp, #4]
    ldr.w r1, [sp, #8]
    //Modify the loaded value
    add.w r1, #4
    //Store value and restore r1
    str.w r1, [sp, #8]
    ldr.w r1, [sp, #4]
    bx lr
```
Using the stack to interchange values.
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
sample_b:
    //Bank stackpointer
    mov.w r1, #2
    msr.w control, r1
    //Modify the loaded value
    add.w sp, #4
    //Bank stackpointer
    mov.w r1, #0
    msr.w control, r1
    bx lr
```
Using the stack pointer to interchange values.
  </div>
</div>

While banking the stack pointer is faster than using the stack, it is not faster than using FPU registers. Using register banking is thus only viable when all registers have been exhausted. 

#### Benchmarks comparing tactics to swap the value in a register with a saved value.
| Example                   | sample a | sample b |
|--------------------------|------------------------|-------------------------|
| **Instructions executed** | 5                      | 5                       |
| **LSU count**             | 6                      | 0                       |
| **CPI count**             | 0                      | 2                       |
| **Fold count**            | (-) 0                  | (-) 0                   |
| **Cycle count**           | 11                     | 7                       |

Note that according to the technical reference manual, a barrier instruction is needed after writing to the control register, which would make this slower than using the stack. ARM has implemented register banking without barriers in the cortex M4 [standard library](https://www.github.com/ARM-software/CMSIS_4/blob/master/CMSIS/RTOS/RTX/SRC/GCC/HAL_CM4.s).

While no issues were encountered in testing, it is highly recommended to carefully test not using barrier for unexpected behavior if used.

