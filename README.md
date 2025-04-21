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

*See the `tasks.ipynb` for full code and test cases.*

---

### Task 2: Hash Functions

**Implementation Insight:**  
I ported a classic C hash function to Python:
- The function iterates over each character of the string, converts it to its ASCII value using `ord()`, multiplies the current hash by 31 (an odd prime to prevent overflow issues), and then adds the ASCII value.
- The final hash value is obtained by applying a modulo 101 operation, which helps distribute hash values uniformly.

*Detailed reasoning and collision analysis are included in the `tasks.ipynb`.*

---

### Task 3: SHA256 Padding

**Implementation Insight:**  
I implemented a function that computes the SHA256 padding for a file by:
- Reading the file in binary mode.
- Appending a `0x80` byte to indicate the end of the original message.
- Calculating the necessary zero padding so that the message length (excluding the 8-byte length field) is 56 bytes modulo 64.
- Appending the original message length as a 64-bit big-endian integer.

*This implementation strictly follows the NIST FIPS 180-4 specification. See the `tasks.ipynb` for complete details.*

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

**Implementation Insight (Placeholder):**  
This task involves calculating the first 32 bits of the fractional part of the square roots of the first 100 prime numbers. I plan to detail the conversion from floating-point values to a 32-bit integer representation and relate the results to cryptographic initial hash values.

*Details to be added.*

---

### Task 6: Proof of Work

**Implementation Insight (Placeholder):**  
In this task, I will search for English words that yield the highest number of leading zero bits in their SHA256 hash digest. The implementation will include hash computation, leading zero counting, and dictionary verification.

*Details to be added.*

---

### Task 7: Turing Machines

**Implementation Insight (Placeholder):**  
This task requires designing a Turing Machine that adds 1 to a binary number. I will include state transition diagrams, a simulation in Python, and step-by-step execution details.

*Details to be added.*

---

### Task 8: Computational Complexity

**Implementation Insight (Placeholder):**  
I will implement bubble sort in Python, modified to count the number of comparisons for sorting each permutation of a list. The task will include a detailed analysis of comparison counts across permutations.

*Details to be added.*

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

This repository provides comprehensive implementations and insights for Tasks 1 to 4, with planned details for Tasks 5 to 8. For full code examples, detailed explanations, and test results, please refer to the attached `tasks.ipynb`.

---
