---
weight: 1
title: Alignment rules
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


While testing the following behavior was found: There are unexplored edge cases as memory
operations that do not touch the FLASH seem to affect these delays. This behavior is
probably dependent on the specifics of the memory from which the instructions are loaded.
Testing was done by loading instructions from FLASH and using SRAM2 as RAM.
- Groups of three or more unaligned wide instructions will take one extra cycle.
- If the second unaligned wide instruction is a memory instruction, the DWT LSU counter will be incremented.
- If the second unaligned wide instruction is not a memory instruction, the DWT CPI counter will be incremented.
- Groups of unaligned wide str only add the penalty cycle from a length of 4.
- A group of unaligned wide instructions consisting of a mix of memory and non-memory instructions can cause more than one delay cycle.
- Branches to unaligned wide instructions will take one extra cycle.