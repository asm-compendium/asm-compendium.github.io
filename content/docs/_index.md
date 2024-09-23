---
title: Documentation
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

To illustrate the behavior of instructions, many side-by-side comparisons of assembly functions are used in this compendium. To facilitate reading we will go through some example figures. The assembly samples used in this explanation are shown below.
These examples perform the same function using different instructions.


<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
sample_a:
    mov r0, sp
    sub r0, r0, #4
    str r5, [r0]
    bx lr
```
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
sample_b:
    mov r0, sp
    str r5, [r0, #-4]!
    bx lr

```
  </div>
</div>


The tables below show an overview of the information known about each instruction in the test samples. The adr column shows the address where the instruction is located; this can be useful when reading jump statements as the labels are lost in compilation. The bytes column shows how an instruction is encoded. This can be helpful because this architecture supports different types of encoding for the same mnemonics. The bytecode can highlight differences between seemingly similar instructions. The mnemonic and operands columns form the actual disassembly, which is important to check since the compiler might lend a "helping hand" when the programmer does not expect it and replace instructions. Exec_count is based on the execution trace of the program and shows how often an instruction is executed during testing. Finally, the aligned column shows if 32-bit instructions are located at a word-aligned address.

#### Disassembly of sample_a.s
| adr        | bytes    | mnemonic | operands      | exec_count | aligned |
|------------|----------|----------|---------------|------------|---------|
| 0x80001b0  | 6846     | mov      | r0, sp        | 1          | True    |
| 0x80001b2  | a0f10400 | sub.w    | r0, r0, #4    | 1          | False   |
| 0x80001b6  | 0560     | str      | r5, [r0]      | 1          | True    |

#### Disassembly of sample_b.s
| adr        | bytes    | mnemonic | operands            | exec_count | aligned |
|------------|----------|----------|---------------------|------------|---------|
| 0x80001b0  | 6846     | mov      | r0, sp              | 1          | True    |
| 0x80001b2  | 40f8045d | str      | r5, [r0, #-0x4]!    | 1          | False   |


The following table shows the cycle counts collected while benchmarking the code. The processor can only count the total amount of cycles used (CYC), the number of additional cycles memory instructions took (LSU), and the number of extra cycles taken by non-memory instructions (CPI). By counting the instructions inside the execution trace we can also get the number of instructions executed. The sum of the instructions executed, LSU, and CPU should equal the CYC. The tool will give warnings if this does not hold.

#### Benchmark results
| Example                  | Sample a | Sample b |
|-------------------------|------------------------------------------------|------------------------------------------------|
| **Instructions executed**| 3                                              | 2                                              |
| **LSU count**            | 0                                              | 0                                              |
| **CPI count**            | 0                                              | 0                                              |
| **Fold count**           | (-) 0                                          | (-) 0                                          |
| **CYC count**            | 3                                              | 2                                              |

