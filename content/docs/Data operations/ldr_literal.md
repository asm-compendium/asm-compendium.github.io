---
weight: 1
title: Loading larger values using ldr literal
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

It is possible to use a load instruction to move a variable into a register that would
be too big for the `mov` instruction. An example of this is `ldr r0, =0x01234567`.
A common belief is that it is faster to replace this with a combination of `mov` and `movt` to move the parts separately, such as described in this [paper](https://www.sciencedirect.com/science/article/pii/S1474667015373444).
 In testing these solutions seem to perform identically (both solutions taking two cycles) in most cases and a load
literal does not always cause a stall cycle as the aforementioned paper claims. Benchmarks show that
it does not perform identically as suggested in this accepted [stack-overflow answer](https://stackoverflow.com/questions/60626117/armv7-m-bare-metal-ldr-str-symbolic-memory), either.
The following observations were made based on benchmarking:
- Load literal only pipelines with another load literal.
- A str-ldr (literal) pair will take 5 cycles.
- The worst place for a store is before a load instruction.
- A load literal, loading a small value may be replaced by `mov` by the compiler.

Generally, it is faster to use `mov; movt` when the operation is preceded by a
store, and it is faster to use ldr (literal) when multiple literal loads can be
grouped, at least with the tested boards at 24mhz. Still, the replacement of
instructions can cause issues by interrupting the load chain with a "faster"
move, and in general, executing a completely different instruction reduces the
transparency of the code. Therefore, it is recommended to always start with the
`mov; movt` solution and using ldr (literal) only when, optimizing for a particular
board, it can be grouped, and the performance impact of using it can be verified.
Note that the load literal encoding depends on the surrounding structure, not
on the loaded value. The 16-bit instruction can encode an offset of multiples of
four in the range 0 to 1020 while the 32-bit instruction can value in the range
-4095 to 4095. This means that the instruction will switch between the two
encodings depending on the surrounding code and where the compiler stores the
literal values, making alignment harder to predict. Therefore it is recommended
to force 32-bit encoding using ldr.w.