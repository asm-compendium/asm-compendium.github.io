---
weight: 4
title: Avoid hazards
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

Hazards refer to situations that cause delay cycles. For memory instructions,
the largest source of hazards is using a register in address resolution right after
it was modified. An example of this is `sub sp, #4; ldr r0, [sp]`, space these
operations apart whenever possible. A more extreme example is a double
dereference, `ldr r0, [sp]; ldr r0, [r0]` as it also breaks the timing improvement
that grouping loads normally affords.