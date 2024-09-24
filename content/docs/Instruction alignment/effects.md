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

 Most 16-bit instructions can be replaced by a 32-bit instruction by adding .w at the end of the mnemonic, shifting subsequent code by 16 bits and providing the ability to influence alignment.
 | Address    | Sample a |          | Sample b |          |
|------------|--------------------------|----------|--------------------------|----------|
| 0x80001b0  | c046                     | <span style=" font-weight:bold;">4fea</span> | 4fea                     | 0808     |
| 0x80001b4  | <span style=" font-weight:bold;">0808</span> | 4fea | <span style=" font-weight:bold;">4fea</span> | <span style=" font-weight:bold;">0808</span> |
| 0x80001b8  | 0808                     | <span style=" font-weight:bold;">4fea</span> | 4fea                     | 0808     |
| 0x80001bc  | <span style=" font-weight:bold;">0808</span> |          | <span style=" font-weight:bold;">4fea</span> | <span style=" font-weight:bold;">0808</span> |

