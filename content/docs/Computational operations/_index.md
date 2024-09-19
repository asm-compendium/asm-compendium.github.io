---
draft: false
title: Computational operations
type: docs
# prev: docs/first-page
# next: docs/folder/registers
sidebar:
  open: true
---
When optimizing a specific algorithm for a specific microarchitecture the main
question is: “How to perform the operations needed by the algorithm with the
set operations available”. Microcontrollers often contain specialized circuitry for
computationally hard tasks. It is recommended to go through the ways these
can be used and check if any are usable by the algorithm. This is not always
straightforward; sometimes part of a specialized operation can be used such
as shown in `Unsigned `~~`Multiply`~~` Accumulate Long`, or a specialized instruction can be used in some
other unintended way, as shown in `SEL based conditional move`.
