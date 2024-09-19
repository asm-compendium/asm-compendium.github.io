---
weight: 8
title: Overwriting the stack-pointer
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

While gaining an additional register might sound promising, this has a few
pitfalls. Bits \[0,1\] are "Read As Zero/Write Ignored" thus, add sp, #5 will add
4 to the stack pointer instead of 5. It is also essential to have some way to
recover the stack pointer, this could be a hard-coded address or a FPU register.
Finally, there is the issue that any interrupts that rely on a valid stack pointer
will crash. An example of this is the interrupt handler used in PQM4 that
keeps track of overflowing cycle counts. Working on a system that seemingly
crashes randomly when an interrupt is triggered in the background would cause
headaches for any programmer.