---
weight: 5
title: Digital signal processing instructions (DSP)
type: docs
# prev: docs/
# next: docs/folder/
---

[Link Text]({{< relref "division.md" >}})


The DSP extension consists of a collection of single instruction, and multiple data (SIMD) instructions. As this processor does not possess specialized vector registers, DSP instructions generally operate on bytes or half-words within registers. Some instructions operate on full registers such as `umaal` discussed in [Unsigned Accumulate Long]({{< relref "accum.md" >}}) and some that can be made to operate on full registers such as `sel` discussed in [SEL based conditional move]({{< relref "sel_mov.md" >}}). This means that it generally does not provide extra computational power in the same way vector instructions do for more complex architectures. Nevertheless, it does provide flexibility and will increase performance in situations where operating on numbers that are not represented in a multiple of 32 bits is unavoidable. In cryptography, this often happens while working with big integer arithmetic. 

The authors of "Saber on ARM CCA-secure module lattice-based key encapsulation on ARM" [[eprint]](https://eprint.iacr.org/2018/682) were able to use DSP multiplication instructions in schoolbook multiplication to eliminate one-third of the multiplication instructions. "Polynomial Multiplication in NTRU Prime Comparison of Optimization Strategies on Cortex-M4" [[eprint]](https://eprint.iacr.org/2020/1216)) also describes multiple effective uses for DSP instructions.