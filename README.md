# Proof Lab

A small zkVM built as a test bed for one question: **when a proof system is "formally verified", how do we check that what was verified matches what actually ships?**

This repository is at the planning stage. I'm publishing the plan before the code so that feedback can shape it. If you work on proof systems or formal verification, I'd value ten minutes of your criticism. The questions I most want answered are [further down](#feedback-i-am-looking-for).

## The question

A machine-checked proof can be valid and still not mean what its readers think. I see three places where that happens:

- **The statement.** The theorem may not say what was intended: a hypothesis that can never hold, a missing side condition, a definition that quietly changes the meaning.
- **The model.** The proof may be about a simplified model of the code, not the code that runs.
- **The trust base.** The checker, or a translation step on the way to it, may itself be wrong.

Two public results from 2026 show these gaps are real:

- A forged proof was accepted by Lean's kernel and by an independent checker, through two unrelated bugs ([postmortem](https://leodemoura.github.io/blog/2026-8-1-postmortem-for-kernel-soundness-bug-14576/)).
- The Arguzz fuzzer found eleven soundness and completeness bugs in three of six production zkVMs, some of them previously audited ([paper](https://arxiv.org/abs/2509.10819)).

As AI systems make proofs cheap to produce, checking what a proof actually establishes becomes the scarce part. That is the part I want to work on.

## What I am building

1. **A small STARK-based zkVM in Rust.** A custom instruction set of about twenty instructions, a small prime field and a FRI-based proof. It is a test bed I understand end to end, not a competitor to production zkVMs.
2. **A verifier tamper harness.** Give it any verifier and one valid proof. It makes structured changes (Merkle paths, openings, transcript order, public inputs, parameters) and requires every changed proof to be rejected. It treats the verifier as a black box, so it should work on verifiers other than mine.
3. **A corpus of planted bugs.** Known-bad variants of a correct system, for testing verification tools. It will include constraints that are deterministic but wrong, which tools that only check for under-constraint miss.
4. **An executable Lean specification, and "law mutation".** A Lean spec of the instruction set, written as a step-limited interpreter and tested against the Rust interpreter. Then a measure of how much the spec constrains: change the program slightly and count how many changed versions still satisfy every theorem.

## Status and roadmap

Nothing here is usable yet. Dates are intentions, and I work on this part-time.

| When | What |
| --- | --- |
| Now (late 2026) | Instruction set and interpreter |
| First half of 2027 | STARK prover and verifier, plus an independent second verifier |
| Mid 2027 | Tamper harness and a first planted-bug corpus, both published |
| 2027 to 2028 | Lean spec of the instruction set; law-mutation experiments |

## Feedback I am looking for

1. **Is this a gap you meet in your own work?** If so, where does it hurt most: the statement, the model or the trust base?
2. **What already exists that I should use or contribute to instead of building?**
3. **Which first artefact would be most useful to you:** the tamper harness, the bug corpus or the spec-strength measure?

Open an issue on this repository, or reach me as [@pandelvgun](https://github.com/pandelvgun) on GitHub. Blunt answers are welcome.

## Work this sits next to

- [Arguzz](https://github.com/veridise/arguzz): fuzzing zkVMs from the prover and execution side. My harness aims at the verifier side.
- [Verified zkEVM](https://github.com/Verified-zkEVM), including ArkLib and [Clean](https://github.com/Verified-zkEVM/clean): formal verification of proof systems and circuits in Lean.
- Aeneas: translating Rust into Lean, which I plan to use to connect my Rust code to its Lean spec.
- [lean4lean](https://github.com/digama0/lean4lean) and [nanoda](https://github.com/ammkrn/nanoda_lib): verified and independent checkers for Lean.
- [zkPi](https://eprint.iacr.org/2024/267): proving Lean theorems in zero knowledge.

## About

I'm a software developer in Cape Town, South Africa, with a background in Python and backend performance work. I'm learning Rust, proof systems and Lean in the open, and everything in this repository will be open source.
