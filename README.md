# Computational Theory Assessment (2024/2025)

**Ronan Francis (G00403092)**  
*Computational Theory with Ian McLoughlin*

---

## Table of Contents

1. [Overview](#overview)
2. [Implemented Tasks](#implemented-tasks)
   - [Task 1: Binary Representations](#task-1-binary-representations)
   - [Task 2: Hash Functions](#task-2-hash-functions)
   - [Task 3: SHA256 Padding](#task-3-sha256-padding)
   - [Task 4: Prime Numbers](#task-4-prime-numbers)
   - [Task 5: Roots](#task-5-roots)
   - [Task 6: Proof of Work](#task-6-proof-of-work)
   - [Task 7: Turing Machines](#task-7-turing-machines)
   - [Task 8: Computational Complexity](#task-8-computational-complexity)
3. [Usage Instructions](#usage-instructions)
4. [Commit History & Development Process](#commit-history)
5. [Conclusion](#conclusion)

---

## Overview

This repository contains my submission for the Computational Theory Assessment. It includes complete implementations for Tasks 1 through 4 and plans for the remaining tasks. Full details, code examples, and comprehensive tests are available in the attached `tasks.ipynb`.

---

## Implemented Tasks

### Task 1: Binary Representations

**Implementation Insight:**  
I developed four key functions for manipulating 32-bit unsigned integers, which are critical for cryptographic algorithms such as SHA-256:
- **`rotl(x, n=1)`:**  
  Determines the bit-width, creates a bitmask to preserve 32-bit integrity, and performs a left circular shift with modulo arithmetic.
- **`rotr(x, n=1)`:**  
  Similar to `rotl`, but shifts bits to the right.
- **`ch(x, y, z)`:**  
  Uses bitwise operators to select bits from `y` or `z` based on the bits in `x`.
- **`maj(x, y, z)`:**  
  Computes the majority vote of bits from the three inputs.

*[See the code & tests](tasks.ipynb#Task‑1:-Binary-Representations)*

---

### Task 2: Hash Functions

**Implementation Insight:**  
I ported a classic C hash function to Python:
- The function iterates over each character of the string, converts it to its ASCII value using `ord()`, multiplies the current hash by 31 (an odd prime to prevent overflow issues), and then adds the ASCII value.
- The final hash value is obtained by applying a modulo 101 operation, which helps distribute hash values uniformly.

*Detailed reasoning and collision analysis are included in the [Hash-Functions](tasks.ipynb#Task‑2:-Hash-Functions).*

---

### Task 3: SHA256 Padding

**Implementation Insight:**  
I implemented a function that computes the SHA256 padding for a file by:
- Reading the file in binary mode.
- Appending a `0x80` byte to indicate the end of the original message.
- Calculating the necessary zero padding so that the message length (excluding the 8-byte length field) is 56 bytes modulo 64.
- Appending the original message length as a 64-bit big-endian integer.


---

### Task 4: Prime Numbers

**Implementation Insight:**  
I generated the first 100 prime numbers using two different algorithms:
- **Sieve of Eratosthenes:**  
  A Boolean array marks multiples of each prime. The 100th prime (541) is reached by setting the limit to 542.
- **Sieve of Sundaram:**  
  This method filters out numbers using the formula `i + j + 2*i*j` and then transforms the remaining candidates into primes.

*Performance comparisons and detailed code are provided in the `tasks.ipynb`.*

---

### Task 5: Roots

**Implementation Insight:**  
The helper `get_fractional_bits(prime, bits=32)`:

1. computes `√prime` with `math.sqrt`,  
2. isolates the fractional part,  
3. left-shifts it by 2³²,  
4. converts to an `int`, then  
5. formats the result as a zero-padded 32-bit binary string. 

Running this over the first 100 primes reproduces—bit-for-bit—the eight SHA-256
initial hash constants **H₀…H₇** published in FIPS 180-4, providing a concrete link
between “mysterious” spec values and simple number theory.  
All 100 results are printed in a four-column table inside `tasks.ipynb`, and each
constant was cross-checked against the spec.

*See the notebook for [full code](tasks.ipynb#task-5-roots), the validation script, and the generated table.*

---

### Task 6: Proof of Work

**Implementation Insight:**  
Using the 350 k-word SCOWL list, each candidate word is UTF-8 hashed with
`hashlib.sha256`; a tiny bit-scanner then counts leading 0 bits.  
The undisputed champion is **`goaltenders`**, whose digest begins with **18
consecutive zero bits**—the best found after the entire list was exhausted. 

**Testing & Verification:**  

* *Digest check* – the leading-zero counter is unit-tested with known inputs  
  (`"hello"` ⇒ 2 zeros, `"Digest"` ⇒ 1 zero, etc.).  
* *Dictionary proof* – a live link to the Collins English Dictionary entry for
  “goaltenders” is recorded in the notebook for audit purposes. 

---

### Task 7: Turing Machines

**Implementation Insight:**  
A minimalist, four-state Turing Machine (“the crab”) walks right to the tape end, turns around, and performs binary +1 with proper carry handling:

* `0 → 1` then **halt**  
* `1 → 0`, move left, keep carrying  
* blank `_` at the MSB → write `1`, **halt**  

The implementation `crab_add_one(tape_string)` adds leading/trailing blanks so the head never falls off the tape.

**Testing:**  
A 10-case suite (empty string, single-bit, full-carry 32-bit, etc.) all pass; the
edge case `1111` correctly yields `10000`, proving multi-carry correctness.

---

### Task 8: Computational Complexity

**Implementation Insight:**  
`bubble_sort_with_comparisons()` counts comparisons while sorting.  By enumerating **all 120 permutations of `[1,2,3,4,5]`**:

| Case | Example | Comparisons |
|------|---------|-------------|
| Best | `12345` | **4** |
| Typical | many | 7 – 9 |
| Worst | `54321` | **10** |

Early-exit and shrinking-inner-loop optimisations drop best-case cost to **O(n)**
while preserving the classical **O(n²)** worst-case bound. The full table is
rendered in the notebook for inspection. 

---

## Usage Instructions

1. **Clone the Repository:**
    ```bash
    git clone https://github.com/yourusername/reponame.git
    cd reponame
    ```
2. **Set Up the Environment:**
    ```bash
    python -m venv env
    source env/bin/activate  # For Windows: env\Scripts\activate
    pip install -r requirements.txt
    ```
3. **Run the Notebook:**
    ```bash
    jupyter notebook tasks.ipynb
    ```

---

## Commit History & Development Process

I maintained a detailed commit history to document incremental improvements throughout the project. Please refer to the repository’s commit log for the complete history.

---

## Conclusion
Over the course of this assessment I implemented eight standalone tasks that move from low-level bit manipulation to high-level complexity analysis. Completing them deepened my understanding of cryptographic primitives, algorithmic efficiency, and formal models of computation. Future work could extend the proof-of-work search to GPU acceleration and add visualisations of the Turing-machine tape evolution

---
