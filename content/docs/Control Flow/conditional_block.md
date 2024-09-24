---
weight: 4
title: Conditional block
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


The Cortex M4 supports constant time conditional execution using \asm{it} blocks. This is useful when avoiding timing attacks. A documented example from the Technical Reference Manual\cite{ARM_Cortex-M4_TRMr0p1} is shown below.

#### Example of conditional execution using an `it` block.
``` {filename="it example"}
MOVS R0, R1 ; R0 = R1, setting flags
IT MI ; skipping next instruction if value 0 or positive
RSBMI R0, R0, #0 ; If negative, R0 = -R0
```

 The comments from the example above could lead one to believe that it would take two cycles if the value is 0 or positive, due to the final instruction being "skipped", and three when it is negative. Luckily instructions are not skipped, the state is unaffected by skipped instructions due to the results being discarded. This is similar to how grouped processing elements in a GPU act when not all executing the same instructions, for GPU it is considered undesirable due to the speed impact. Yet in this case, the speed impact is ideal, as it enables constant time conditional execution. `it` instructions have the unique ability to be folded into a preceding 16-bit Thumb instruction, enabling execution in zero cycles. This can be seen comparing the tables below.

\begin{table}[ht]
\centering
\caption{Metrics of the code shown in Listing\ref{fig:it_example} code, showing identical cycle times regardless of execution.}
\label{tab:it_unalign}
    \begin{tabular}{|lrr|}
    \hline
     Argument:               & r1 = 3 & r1 = -3 \\
     \hline\hline
     Instructions executed: & 3 & 3 \\
     LSU count:             & 0 & 0 \\
     CPI count:             & 0 & 0 \\
     Fold count:             & (-) 1  & (-) 1  \\
     \hline
     Cycle count:           & 2 & 2 \\
    \hline
    \end{tabular}
\end{table}

#### Benchmarks of the example code, showing identical cycle times regardless of execution.
| Argument                | r1 = 3 | r1 = -3 |
|------------------------|----------|----------|
| **Instructions executed** | 3        | 3        |
| **LSU count**             | 0        | 0        |
| **CPI count**             | 0        | 0        |
| **Fold count**            | (-) 1    | (-) 1    |
| **Cycle count**           | 2        | 2        |

#### Benchmarks of the example code, with the instruction before the `it` instruction changed to a wide instruction.
| Argument                | r1 = 3 | r1 = -3 |
|------------------------|----------|----------|
| **Instructions executed** | 3        | 3        |
| **LSU count**             | 0        | 0        |
| **CPI count**             | 0        | 0        |
| **Fold count**            | (-) 0    | (-) 0    |
| **Cycle count**           | 3        | 3        |
