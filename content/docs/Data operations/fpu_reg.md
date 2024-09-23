---
weight: 6
title: FPU registers
type: docs
# prev: docs/
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

The FPU found in nearly all Cortex-M4 microcontrollers has thirty-two, 32-bit registers (s0-s31). Furthermore, values stored in them do not have to represent a valid floating point number. Moving between a CPU register and an FPU register always takes one cycle. Two registers can be moved at in a single instruction accessing two of the FPU registers using d0-d16, where d0 contains s0 and s1. `vmov s0, r0; vmov s1, r1` will take two cycles just like `vmov d0, s1, r1` this only saves code size. In nearly all cases, `vmov` will be faster than accessing memory. The only exception is accesses that can be optimized away. If the FPU is not used by any other functions, FPU registers can be used to pass information between functions, breaking free from the ABI restrictions without breaking compatibility with C code. Values can be directly loaded from memory into FPU registers. This is useful in cases where the same value is loaded from memory repeatedly, as an FPU register can be dedicated as cash, loading the value once to the FPU register and loading from that FPU register afterward.


![](/images/all_reg.png "Register overview")