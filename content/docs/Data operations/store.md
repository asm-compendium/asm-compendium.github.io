---
title: Store placement
type: docs
prev: docs/
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

The following guidelines will help ordering memory instructions to minimize
the cycles used.
-  The optimal place for a store is after a load and before a non-memory
instruction.
-  The second best place for a store is between non-memory instructions.
- The worst place for a store is before a load instruction.
- The store multiple instruction stm is space efficient but not necessarily faster, individual stores can perform faster if not followed by a load.