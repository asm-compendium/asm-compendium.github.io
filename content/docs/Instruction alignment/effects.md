---
weight: 1
title: Instruction alignmente effects
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


 Most 16-bit instructions can be replaced by a 32-bit instruction by adding `.w` at the end of the mnemonic, shifting subsequent code by 16 bits and providing the ability to influence alignment.


#### Comparison of between unaligned and aligned move instructions.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
sample_a:
    mov r8, r8
    mov.w r8, r8
    mov.w r8, r8
    mov.w r8, r8
    bx  lr
```
Minimal example of unaligned instructions.
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
sample_b:
    mov.w r8, r8 // Changed
    mov.w r8, r8
    mov.w r8, r8
    mov.w r8, r8
    bx  lr
```
Minimal example of aligned instructions.
  </div>
</div>


#### Performance comparison of unaligned and aligned move instructions.
| Example                | Sample a | Sample bB |
|------------------------|----------|----------|
| **Instructions executed** | 4        | 4        |
| **LSU count**             | 0        | 0        |
| **CPI count**             | 1        | 0        |
| **Fold count**            | (-) 0    | (-) 0    |
| **Cycle count**           | 5        | 4        |

While the operations are identical, and the same number of instructions are executed, the code from example is slower. Taking a closer look at the results the difference is in the CPI counter, even though we are not executing any variable execution time instructions.

#### Disassembly of unaligned moves (sample a).
| adr       | bytes    | mnemonic   | operants   | exec_count | aligned   |
|-----------|----------|------------|------------|------------|-----------|
| 0x80001b0 | c046     | mov        | r8, r8     | 1          | True      |
| 0x80001b2 | 4fea0808 | mov.w      | r8, r8     | 1          | **False** |
| 0x80001b6 | 4fea0808 | mov.w      | r8, r8     | 1          | **False** |
| 0x80001ba | 4fea0808 | mov.w      | r8, r8     | 1          | **False** |

#### Disassembly of aligned moves (sample b).

| adr       | bytes    | mnemonic   | operants   | exec_count | aligned   |
|-----------|----------|------------|------------|------------|-----------|
| 0x80001b0 | 4fea0808 | mov.w      | r8, r8     | 1          | True      |
| 0x80001b4 | 4fea0808 | mov.w      | r8, r8     | 1          | True      |
| 0x80001b8 | 4fea0808 | mov.w      | r8, r8     | 1          | True      |
| 0x80001bc | 4fea0808 | mov.w      | r8, r8     | 1          | True      |

Moves between lower registers can be encoded using 16 bits (narrow). Moves with immediate values can not be encoded as a 16-bit instruction, those moves will be forced to use the 32-bit (wide) encoding. However, a move setting flags `movs` using an immediate value can be encoded as a 16-bit instruction. Most instructions can be encoded in both ways and changes to the operands can affect the encoding and break the instruction alignment. Always check if instructions are aligned after assembling them when optimizing for time.

#### Visualization of the bytecode of unaligned and unaligned instructions.
 Bytes encoding a single instruction are marked in the same weight (bold or normal). The weight is swapped at the start of each instruction.
 | Address    | Sample a |          | Sample b |          |
|------------|--------------------------|----------|--------------------------|----------|
| 0x80001b0  | c046                     | <span style=" font-weight:bold;">4fea</span> | 4fea                     | 0808     |
| 0x80001b4  | <span style=" font-weight:bold;">0808</span> | 4fea | <span style=" font-weight:bold;">4fea</span> | <span style=" font-weight:bold;">0808</span> |
| 0x80001b8  | 0808                     | <span style=" font-weight:bold;">4fea</span> | 4fea                     | 0808     |
| 0x80001bc  | <span style=" font-weight:bold;">0808</span> |          | <span style=" font-weight:bold;">4fea</span> | <span style=" font-weight:bold;">0808</span> |

