---
weight: 3
title: Load placement
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

Load and store placement are linked, thus the previous page should also be taken into account. When swapping a stored in a register for a value stored in memory, such as `str r0, [sp]; str r0, [sp\#4]`, it is advisable to space these operations out with a different instruction. In contrast to stores, sequential loads are the optimal way to use multiple load instructions. Note that, this can break down when using pre-indexing (`ldr r0, [sp, \#4]!`) or post-indexing (`ldr r0, [sp], \#4`), collectively referred to as writeback. When a sequence of loads starts with a load using writeback, this instruction will take extra cycles, each subsequent load using writeback will also take extra cycles. 