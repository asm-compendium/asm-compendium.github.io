---
weight: 3
title: Nop usage
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



Before the introduction of Unified Assembly Language (UAL), `nop` used to be an alias for `mov r8, r8`, which would always take one cycle. However, the newer nop hint-instruction does not give such a guarantee. The ARMv6-M technical reference manual specifically warns that it is not constant-time, while the ARMv7-M manual only states that its behavior is implementation dependent. There have been [reports](https://www.pabigot.com/arm-cortex-m/the-effect-of-the-arm-cortex-m-nop-instruction/) of the nop hint-instruction taking zero cycles on the M4 under certain conditions. 

In experimentation we found that `nop` pipelines with `ldr`, with `ldr`-`nop` taking three cycles and `ldr`-`mov r8, r8` 4 (as seen in the table below). This behavior is observed regardless of 16- or 32-bit encoded instructions. 

#### Comparison of the two ways `ldr`, `nop` can be encoded.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
sample_a:
    ldr r0, [sp]
    mov r8, r8 //c046
    bx      lr
```
Move instruction based `nop`. (old)
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
sample_b:
    ldr r0, [sp]
    nop //00bf
    bx      lr
```
Hint instruction based `nop`. (new)
  </div>
</div>

Notable in the table below is the distribution of DWT cycles, a classic reading would be an LSU count of zero meaning that the load took one cycle and the `nop` two cycles. While comparing the total cycles could result in concluding the nop takes zero cycles. This may be similar behavior to `ldr` pipelining with a `str` instruction. Regardless of the underlying mechanics, using the UAL `nop` instruction introduces uncertainty and there might even be interactions with other instruction types.

#### Benchmarks highlighting different behavior of `nop` types.
| Example                | Sample a | Sample b |
|------------------------|----------|----------|
| **Instructions executed** | 2        | 2        |
| **LSU count**             | 2        | 0        |
| **CPI count**             | 0        | 1        |
| **Fold count**            | (-) 0    | (-) 0    |
| **Cycle count**           |  4       | 3        |


The choice between nop encoding is assembler dependent, for example, NASM uses the old `mov r8, r8` while gas and GCC use the UAL `nop` hint-instruction. This uncertainty combined with implementation dependent cycle counts is not something we want in our code, nops should thus be manually replaced by `mov r8, r8`. Forcing the 32-bit encoding for some instructions to align your code is guaranteed to cost 0 cycles and is thus preferable while aiming for alignment. 

In the event that `nop` after a load is unavoidable for alignment purposes, it would be faster to use the UAL `nop`, this can be forced by directly encoding it using `.short 0xbf00` where you would normally place the instruction.

Note that `mov r8, r8` is also not completely free of side effects as it can introduce a read-after-write delay. It does not matter that it does not change the content of r8. This can be mitigated by moving a different register (sample d).


#### Example of the old `nop` instruction coursing a delay and a proposed solution.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_c.s"}
sample_c:
    mov r8, lr
    mov r8, r8
    ldr r0, [r8]
    bx      lr
```
Old nop introducing a read-after-write delay.
  </div>
  <div class="box">

```verilog {filename="sample_d.s"}
sample_d:
    mov r8, lr
    mov r7, r7
    ldr r0, [r8]
    bx      lr
```
Changing the moved register to avoid the delay.
  </div>
</div>


#### Benchmark highlighting the interaction of a `mov` based `nop` and a load instruction.
| Example                | Sample c | Sample d |
|------------------------|----------|----------|
| **Instructions executed** | 3        | 3        |
| **LSU count**             | 3        | 3        |
| **CPI count**             | 1        | 0        |
| **Fold count**            | (-) 0    | (-) 0    |
| **Cycle count**           | 7        | 6        |