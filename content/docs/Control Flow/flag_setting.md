---
weight: 3
title: Flexible flag setting
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

ARM processors traditionally include four condition flags, combinations of which as for conditional execution, including branches. For the Cortex-M series, these are stored in the Application Program Status Register (APSR).
As seen in in the table, the NE condition actually checks if the zero-flag has not been set.
An interesting feature of this instruction set is that many instructions can set these flags based on the result by appending an `s` to the mnemonic. An example of this would be using using `adds` instead of `add` to be able to check for overflowing results.

#### Table depicting the Application Program Status Register bits checked for each condition. 
| Condition | N  | Z  | C  | V  | Description                         |
|-----------|----|----|----|----|-------------------------------------|
| EQ        | -  | 1  | -  | -  | Equal                               |
| NE        | -  | 0  | -  | -  | Not equal                           |
| CS/HS     | -  | -  | 1  | -  | Carry set                           |
| CC/LO     | -  | -  | 0  | -  | Carry clear                         |
| MI        | 1  | -  | -  | -  | Minus, negative                     |
| PL        | 0  | -  | -  | -  | Plus, positive or zero              |
| VS        | -  | -  | -  | 1  | Overflow                            |
| VC        | -  | -  | -  | 0  | No overflow                         |
| HI        | -  | 0  | 1  | -  | Unsigned higher                     |
| LS        | -  | 1  | 0  | -  | Unsigned lower or same              |
| GE        | =  | -  | -  | =  | Signed greater than or equal        |
| LT        | ≠  | -  | -  | ≠  | Signed less than                    |
| GT        | =  | 0  | -  | =  | Signed greater than                 |
| LE        | ≠  | 1  | -  | ≠  | Signed less than or equal           |
| AL        | -  | -  | -  | -  | Always (unconditional)              |


The example code shows how conditional flags can help minimize loop control instructions. The conditional flags are limited and can not check arbitrary values. By counting down instead of up, the zero flag will be set when counted all the way down, and the zero-flag can be checked using `eq` and  `ne`. The code shown below uses this approach to eliminate the need for a `cmp` instruction, saving space and cycles. Depending on the operations needed in a loop, it may also be possible to hijack an operation inside the loop, for loop control by choosing the condition and flag-setting instructions carefully.

#### Comparison of loop control approaches to loop 10 times.
<div class="side-by-side">
  <div class="box">

```verilog {filename="sample_a.s"}
count_to_ten:
    mov r0, #0
    loop:
        add r0, #1
        cmp r0, #10
        bne loop   
    bx lr
```
Minimal comparison based loop control for executing 10 times.
  </div>
  <div class="box">

```verilog {filename="sample_b.s"}
count_from_ten:
    mov r0, #10  
    loop:
        subs r0, #1
        bne loop
    bx lr

```
Minimal flag-setting based loop control for executing 10 times.
  </div>
</div>


#### Benchmarks of loop control approaches.
| Example                | Sample A | Sample B |
|------------------------|----------|----------|
| **Instructions executed** | 31        | 21        |
| **LSU count**             | 0        | 0        |
| **CPI count**             | 9        | 9        |
| **Fold count**            | (-) 0    | (-) 0    |
| **Cycle count**           | 40        | 30        |

Caution, instructions setting flags use a distinct and often shorter encoding. A compiler might replace instructions such as `mov r0, \#0` with `movs r0, \#0` as the latter can use 16-bit thumb encoding. This is not an issue with GCC, as it gives the error 'cannot honor width suffix' when compiling, `mov.n r0,\#0`, a forced 16-bit(narrow) version of move immediate. NASM on the other hand can encode `mov r0, \#0` as `movs r0, \#0` without prompting. These replacements can cause unexpected behavior, thus check the disassembly of the program when this seems to be the case.
