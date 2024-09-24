---
weight: 10
title: Accessing the process stack pointer directly
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



In contrast to some other banked register implementations, the process stack pointer can be accessed even when the sp register points to the main stack pointer. Sample a shows how to store to and load from the process stack pointer. Accessing the process stack pointer takes one cycle, which makes it nearly equivalent to an FPU register with 2 missing bits. 



#### Examples of using the process stack pointer to store a value.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
sample_a:                
    mov.w r0, #0x1330
    //Save top 30 bits of r0
    msr psp, r0
    //Use r0
    //Recover top 30 bits of r0
    mrs r0, psp
    bx lr



```
Directly saving to the PSP when the bottom two bits are not needed.
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
sample_b:
    mov.w r0, #0x1337
    //Save 30 lsb of r0
    lsl r0, #2
    msr psp, r0
    //Use r0
    mov r0, #1
    //Recover 30 lsb of r0
    mrs r0, psp
    lsr r0, #2
    bx lr
```
Spilling the 30 least significant bits to the PSP using a shift.
  </div>
</div>


Adding a shift operation allows saving the 30 least significant bits. As this doubles the cycles needed (from two to four), the process stack pointer is primarily useful for storing aligned memory addresses. The PSP is designed to hold an aligned memory address, so this is to be expected.

#### Measured cycle counts for the examples shown above.
| Example:                |  sample a  | sample b |
|--------------------------|------------------------|------------------------|
| **Instructions executed** | 3                      | 6                      |
| **LSU count**             | 0                      | 0                      |
| **CPI count**             | 0                      | 0                      |
| **Fold count**            | (-) 0                  | (-) 0                  |
| **Cycle count**           | 3                      | 6                      |

In some cases it will be possible to eliminate the second shift by using the barrelshifter, this brings it back down to three cycles, which is faster SRAM. With a microcontroller that accesses to core coupled memory, it is a more complex choice, as loading a value only takes two cycles and storing takes one cycle, while it is the other way around barrels-shifted PSP access. The difference in load time means that this technique can be faster than memory operation on core coupled memory, but only when multiple loads are performed. An example of this would be a constant that is used in multiple places and can not be kept in a register all the time.
