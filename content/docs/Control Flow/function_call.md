---
weight: 6
title: Functions calls
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

    Jumping to a function and returning costs four cycles, any function that calls functions needs to spill and recover the link register, this will cost at least two cycles depending on how the link register is saved. Passing arguments in the standardized way following the Application Binary Interface (ABI) causes additional overhead. This slows the function call as it forces spilling registers due to only having four registers available to pass arguments and most registers being callee saved (r4-r11).

    Functions that are only called from assembly functions do not need this standardization, significantly decreasing the overhead of using functions. Even so, it will never be as fast as inlining the function. Functions that are called once should be inlined or rewritten into a macro if inlining decreases the readability. In all other cases, the trading of 4-6 cycles per execution needs to be weighed against the additional code size.