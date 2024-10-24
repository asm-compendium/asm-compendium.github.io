---
weight: 5
title: Register balancing
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

The concept of register balancing is important for any architecture but vital for microcontrollers, due to the combination of few and small registers. This architecture uses sixteen 32-bit registers, thirteen of which are general-purpose registers. 

It is possible to increase the number of usable registers to fourteen by storing the link register somewhere and restoring it before returning. This can be done without cost in many cases, as it is already necessary for any function calling subsequent functions. 

![](/images/all_reg.png "Overview of the ARMv7-M CPU and FPU registers.")
The CPU registers are shown as depicted in the architectural reference manual. The FPU registers can be referenced as 32-bit `s` registers and 64-bit `d` registers. Each `d` register consist of two `s` registers.