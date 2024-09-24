---
weight: 4
draft: false
title: Instruction alignment
type: docs
prev: docs/
# next: docs/folder/registers
sidebar:
  open: true
---

To save space the thumb instructions set uses a mix of 16- and 32-bit instructions. This can easily result in a misaligned block of instructions which will occasionally force the use of an additional fetching cycle.