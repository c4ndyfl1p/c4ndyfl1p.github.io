+++
author = "c4ndy"
title = "My Master's Thesis: Lattice-Based Zero-Knowledge Proofs with VOLE"
date = "2026-06-30"
description = "What my Master's thesis is about, in plain words: proving that a post-quantum ciphertext is well formed without revealing its secrets, and making those proofs smaller in batches."
tags = [
    "zero-knowledge",
    "post-quantum",
    "lattices",
    "vole",
    "ml-kem",
]
categories = [
    "security",
    "cryptography",
]
projectSection = "security"
+++

In June 2026 I handed in my Master's thesis in Computer Science at Aarhus University, titled **Lattice-based Zero-knowledge proofs with VOLE in the batch setting**. I was supervised by Peter Scholl and Diego F. Aranha.

**[Read the full thesis (PDF, 80 pages)](/voles_ZK_thesis-11.pdf)**

Here's what it's about, without the 80 pages.

## Proving something without giving it away

A zero-knowledge proof lets one party (the *prover*) convince another (the *verifier*) that a statement is true, without revealing anything beyond the fact that it's true. The classic example: proving you know a password without ever showing the password.

They're the backbone of a lot of privacy-enhancing technology: anonymous credentials, private payments, threshold and verifiable encryption, and more.

## Why lattices, and why now

Most public-key cryptography we use today (RSA, Diffie–Hellman, elliptic curves) can be broken by a large enough quantum computer. Nobody knows exactly when one will exist, but some projections say within one or two decades. That doesn't leave a lot of time to design, standardise and deploy replacements.

The strongest replacements are built on **lattices**, and specifically on the *Learning With Errors* (LWE) problem. The new post-quantum key-exchange standard, **ML-KEM** (formerly CRYSTALS-Kyber), is one of them.

The catch: zero-knowledge proofs for lattice statements are much harder than for the old discrete-log ones. It isn't enough to prove you know a secret **s** with **A·s = t**. You also have to prove that **s** is *small*, because without the smallness the statement is meaningless. Proving "small" efficiently is one of the big practical headaches in lattice cryptography.

## What I did

I built a zero-knowledge proof that an ML-KEM-style (Module-LWE) ciphertext is valid and well formed, without revealing the secret or the error used to make it. I used the real ML-KEM parameters, so it's grounded in something that's actually deployed.

The proof uses **VOLE-based zero-knowledge** (Vector Oblivious Linear Evaluation), a family of proofs that's fast for the prover, light on memory, and scales well to large statements.

Three pieces I'm happy with:

1. **Commitments for ring elements.** VOLE-based proofs work nicely over fields, but Kyber works over polynomial *rings*, where zero divisors break the usual security argument. I gave a commitment scheme for ring elements that works around this by lifting into an extension field, using subfield VOLE.
2. **Cheaper setup.** VOLE proofs need a lot of random correlations to begin with. I drew on techniques from *silent* secure computation (pseudorandom correlation generators) so these can be generated with communication sublinear in how many you need. I also tried to present those techniques in an accessible way, because the papers are not.
3. **Commit-and-sample.** ML-KEM samples its secrets from a central binomial distribution. Instead of committing to the full secret, the prover commits to a short pseudorandom seed and does the binomial sampling *on the committed values*. When many proofs are batched together, this cuts the proof size by roughly a third.

## What's still open

The thesis covers the core Module-LWE relation, not the full Kyber KEM. Extending it to the full encapsulation (including ciphertext compression and the Fujisaki–Okamoto transform for CCA security), doing a dedicated security analysis of the LPN parameters over Kyber's small field, and actually implementing and benchmarking the protocol are all natural next steps.

## If you want to read it

- **If you're not a cryptographer**, Chapter 1 is the intro and Chapter 2 builds up the maths from groups onwards.
- **If you work in secure computation**, Chapter 1.5 has a reading path for you: Chapter 3 (VOLE), the end of Chapter 4, then Chapters 6, 8 and 9.

And if you're working on anything nearby (lattices, VOLE, privacy-preserving protocols), I'd love to talk. [Get in touch](/contact/).
