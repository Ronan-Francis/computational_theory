# Computational Theory Assessment (2024/2025)

**Ronan Francis (G00403092)**  
*Computational Theory with Ian McLoughlin*

---

## Table of Contents

1. [Task 1: Binary Representations](#task-1-binary-representations)
2. [Task 2: Hash Functions](#task-2-hash-functions)
3. [Task 3: SHA256 Padding](#task-3-sha256-padding)
4. [Task 4: Prime Numbers](#task-4-prime-numbers)
5. [Task 5: Roots](#task-5-roots)
6. [Task 6: Proof of Work](#task-6-proof-of-work)
7. [Task 7: Turing Machines](#task-7-turing-machines)
8. [Task 8: Computational Complexity](#task-8-computational-complexity)
9. [Conclusion](#conclusion)

---

## Task 1: Binary Representations

This task involves implementing core functions for manipulating binary representations of 32-bit unsigned integers. These operations are essential in various areas including cryptography (e.g., SHA-256), data encoding, and low-level programming.

### Circular Shifts

Circular (or bitwise) shifts rotate the bits of a number left or right. Unlike logical shifts (which discard shifted bits), circular shifts wrap the bits around, preserving the overall data “weight” and bit-length.  
For instance:
- **Left circular shift (`rotl`):** Shifts bits to the left with overflow bits reinserted at the right.
- **Right circular shift (`rotr`):** Shifts bits to the right with overflow bits reinserted at the left.

For more information, see the [Circular Shift Wikipedia article](https://en.wikipedia.org/wiki/Circular_shift).

### Bitwise Operators in Python

Python’s bitwise operators allow efficient low-level manipulation:
- **AND (`&`):** Returns `1` only if both bits are `1`.
- **OR (`|`):** Returns `1` if at least one bit is `1`.
- **XOR (`^`):** Returns `1` when the bits differ.
- **NOT (`~`):** Inverts each bit.
- **Left Shift (`<<`):** Shifts bits to the left.
- **Right Shift (`>>`):** Shifts bits to the right.

For further details, see [GeeksforGeeks Bitwise Operators](https://www.geeksforgeeks.org/python-bitwise-operators/).

### Functions Implemented

#### 1. `rotl(x, n=1)`
- **Purpose:** Rotate the bits of a 32-bit unsigned integer `x` to the left by `n` positions.
- **Method:** Combines left and right shifts using a bitmask (e.g., `0xFFFFFFFF`) to ensure 32-bit integrity.  
- **Considerations:** Handles cases when `n >= 32` using modulo arithmetic.

#### 2. `rotr(x, n=1)`
- **Purpose:** Rotate the bits of a 32-bit unsigned integer `x` to the right by `n` positions.
- **Method:** Similar to `rotl` but with the shifting directions interchanged.
- **Considerations:** Uses bitmasking and modulo operations to maintain 32-bit constraints.

#### 3. `ch(x, y, z)`
- **Purpose:** Implements a bitwise “choice” function. For each bit position, if the bit in `x` is `1`, it selects the corresponding bit from `y`; otherwise, it uses the bit from `z`.
- **Application:** Often used in cryptographic algorithms such as SHA-256.

#### 4. `maj(x, y, z)`
- **Purpose:** Computes the majority vote for the bits in `x`, `y`, and `z` at each position. The result is `1` if at least two of the bits are `1`.
- **Application:** A core component in many hash functions to ensure effective diffusion of input bits.

*Examples and test cases are included in the code sections for each function.*

[Back to Top](#table-of-contents)

---

## Task 2: Hash Functions

This section converts a classic C hash function from *The C Programming Language* into Python. The original function multiplies the current hash value by `31` and adds the ASCII value of the character using `ord()`, then reduces the result modulo `101`.

### The Original C Hash Function

```c
unsigned hash(char *s) {
    unsigned hashval;
    for (hashval = 0; *s != '\0'; s++)
        hashval = *s + 31 * hashval;
    return hashval % 101;
}

### Python Implementation
The Python version follows the same logic:
- **Iteration:** Each character in the string is processed.
- **Calculation:** Uses a base of 31 and applies a modulo operation at the end.
- **Usage of ord():** Converts each character to its Unicode code point.

```python
def hash(s, base=31, modulus=101):
    hashValue = 0
    for c in s:
        hashValue = ord(c) + base * hashValue
    return hashValue % modulus
```

A set of test strings is then hashed, printing both the binary representation (with padding) and the numeric value.

### Testing and Collision Analysis
Additional code generates unique random alphanumeric strings to test collisions across different bases and moduli. For example, parameters such as 31 and 101 have been tested against alternatives to analyze the collision rate. The commonly used values of 31 (an odd prime that helps avoid information loss on overflow) and 101 (a prime modulus ensuring uniform distribution) emerged as effective choices.

For further reading, refer to [*Effective Java*](https://ia800308.us.archive.org/16/items/java_20230528/Joshua%20Bloch%20-%20Effective%20Java%20%283rd%29%20-%202018.pdf).


[Back to Top](#table-of-contents)

---

## Task 3: SHA256 Padding
*Details to be added...*


[Back to Top](#table-of-contents)

---

## Task 4: Prime Numbers
*Details to be added...*


[Back to Top](#table-of-contents)

---

## Task 5: Roots
*Details to be added...*


[Back to Top](#table-of-contents)

---

## Task 6: Proof of Work
*Details to be added...*


[Back to Top](#table-of-contents)

---

## Task 7: Turing Machines
*Details to be added...*


[Back to Top](#table-of-contents)

---

## Task 8: Computational Complexity
*Details to be added...*


[Back to Top](#table-of-contents)

---

## Conclusion
*Details to be added...*


[Back to Top](#table-of-contents)

---