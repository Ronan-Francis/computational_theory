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
The function computes a hash value by iterating over the string and combining character ASCII values with a multiplication factor (traditionally 31) and then reducing via a modulus (typically 101).

```c
unsigned hash(char *s) {
    unsigned hashval;
    for (hashval = 0; *s != '\0'; s++)
        hashval = *s + 31 * hashval;
    return hashval % 101;
}
```

### Python Implementation
The Python version follows the same logic:
- **Iteration:** Each character in the string is processed.
- **Calculation:** Uses a base of 31 and applies a modulo operation at the end.
- **Usage of ord():** Converts each character to its Unicode code point.

A set of test strings is then hashed, printing both the binary representation (with padding) and the numeric value.

### Testing and Collision Analysis
Additional code generates unique random alphanumeric strings to test collisions across different bases and moduli. For example, parameters such as 31 and 101 have been tested against alternatives to analyze the collision rate. The commonly used values of 31 (an odd prime that helps avoid information loss on overflow) and 101 (a prime modulus ensuring uniform distribution) emerged as effective choices.

For further reading, refer to [*Effective Java*](https://ia800308.us.archive.org/16/items/java_20230528/Joshua%20Bloch%20-%20Effective%20Java%20%283rd%29%20-%202018.pdf).


[Back to Top](#table-of-contents)

---

## Task 3: SHA256 Padding

This task requires the implementation of a Python function that calculates the SHA256 padding for a given file. SHA256 is part of the SHA-2 family, and its padding scheme is specified in the [NIST FIPS 180-4](https://doi.org/10.6028/NIST.FIPS.180-4) standard. The purpose of padding is to ensure that the final message length is a multiple of 512 bits (or 64 bytes).

### What is SHA256 Padding?

In this task, you will implement the padding routine for the SHA256 algorithm:

- **File Input:**  
  Open and read the file in binary mode.

- **Padding Steps:**  
  1. Append a single `0x80` byte to signal the end of the original message.
  2. Append enough `0x00` bytes so that the total length (excluding the length field) is congruent to 56 modulo 64.
  3. Append the original message length in bits as an 8-byte (64-bit) big-endian integer.

The resulting padding should be returned in hexadecimal format.

### Reasoning Behind the Approach

- **Message Block Alignment:**  
  SHA256 works on 512-bit (64-byte) blocks. By ensuring the padded message aligns perfectly with these blocks—as discussed in a [Crypto StackExchange thread](https://crypto.stackexchange.com/questions/79734/how-to-pad-a-448-bit-message-for-sha256)—the algorithm can process the data efficiently.

- **Clear End-of-Message Marker:**  
  Appending `0x80` (a `1` bit followed by seven `0` bits) unambiguously signals the end of the original message, even when the message itself may end with zero bytes.

- **Ensuring Sufficient Padding:**  
  The calculation that makes the message (excluding the final 8-byte length field) 56 bytes modulo 64 guarantees that, after appending the 8-byte length field, the total length is a multiple of 64. This precise padding is critical for the SHA256 compression function to work correctly.

- **Standards Compliance:**  
  Adhering to the padding scheme as detailed in [NIST FIPS 180-4](https://doi.org/10.6028/NIST.FIPS.180-4) and further explained in [RFC 6234](https://datatracker.ietf.org/doc/html/rfc6234) ensures that the implementation is compatible with other SHA256 implementations.

### Summary

In summary, the Python implementation:

- Reads the file in binary mode.
- Appends a `1` bit as `0x80`.
- Pads with zeros so that the message (without the length) is 56 bytes modulo 64.
- Appends the original message length in bits as a 64-bit big-endian integer.

This method strictly adheres to the SHA256 padding requirements defined in [NIST FIPS 180-4](https://doi.org/10.6028/NIST.FIPS.180-4), as well as the guidelines provided by [RFC 6234](https://datatracker.ietf.org/doc/html/rfc6234) and discussions on [Crypto StackExchange](https://crypto.stackexchange.com/questions/79734/how-to-pad-a-448-bit-message-for-sha256).



[Back to Top](#table-of-contents)

---

# Task 4: Prime Numbers

In this task, we explore two distinct methods for generating the first 100 prime numbers. Prime numbers, which are greater than 1 and divisible only by 1 and themselves, serve as the fundamental building blocks of number theory and are pivotal in areas like cryptography.

---

## Overview

Prime numbers play a critical role in mathematics, particularly because of the [Fundamental Theorem of Arithmetic](https://en.wikipedia.org/wiki/Fundamental_theorem_of_arithmetic). They not only underpin many theoretical aspects of mathematics but also drive modern cryptographic techniques, such as RSA encryption. This task demonstrates two different algorithms to generate prime numbers, allowing for a discussion on algorithmic strategies, performance differences, and optimizations.

---

## Method 1: Sieve of Eratosthenes

### Concept

The Sieve of Eratosthenes is a classic algorithm that efficiently finds all primes up to a specified limit. The method works by iteratively marking multiples of each prime, starting with 2, and then collecting the numbers that remain unmarked.

### Steps

1. **Initialization:**  
   Create an array (or list) of boolean values for all numbers up to the desired limit, marking 0 and 1 as non-prime.

2. **Marking Multiples:**  
   For each number starting at 2 up to the square root of the limit:
   - If the number is still marked as prime, mark all its multiples as non-prime.

3. **Collection:**  
   After the marking process, gather all indices that remain marked as prime. Given that the 100th prime is 541, the limit is set just above this (e.g., 542) to ensure sufficient primes are captured.

### Advantages

- **Efficiency:**  
  Well-suited for generating primes in a moderate range.
- **Simplicity:**  
  The algorithm is straightforward to implement and understand.

### Considerations

- **Memory Usage:**  
  Although efficient for small to medium ranges, the space requirement can become a constraint for very large limits.

---

## Method 2: Sieve of Sundaram

### Concept

The Sieve of Sundaram is an alternative prime generation technique that specifically targets odd numbers, thus reducing the number of elements to process. It removes numbers based on a specific arithmetic form, and then transforms the remaining list into prime numbers.

### Steps

1. **Setup:**  
   Define an upper bound based on the requirement to generate at least 100 primes. The algorithm works with a list of integers representing potential candidates.

2. **Elimination Process:**  
   Remove numbers that fit the form `i + j + 2ij` for valid pairs `(i, j)`, effectively filtering out composite numbers.

3. **Transformation:**  
   Convert the remaining candidates by applying the transformation `2x + 1` to obtain the final list of prime numbers.

### Advantages

- **Reduced Computation:**  
  Focuses only on odd numbers, which cuts down on unnecessary checks.
- **Space Efficiency:**  
  Generally uses less memory by eliminating even numbers upfront.

### Considerations

- **Extra Steps:**  
  Requires an additional transformation step to convert candidates into actual primes.

---

## Performance Comparison

| **Algorithm**                         | **Time Complexity**    | **Space Complexity** | **Pros**                                                                                     | **Cons**                                                           |
|---------------------------------------|------------------------|----------------------|----------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| **Sieve of Eratosthenes**             | O(n log log n)         | O(n)                 | Simple and effective for moderate ranges.                                                  | Memory usage increases with very large limits.                     |
| **Sieve of Sundaram**                 | O(n log n)             | O(n)                 | Works on odd numbers only, reducing unnecessary work.                                        | Requires extra steps to obtain the final list of primes.           |
| **Optimized Trial Division Method**   | O(√n)                  | O(1)                 | Efficient for checking individual numbers; minimal memory overhead.                          | Less suited for generating large lists of primes compared to sieve methods. |

### Optimized Trial Division Method Overview

Every integer can be written as 6 multiplied by some number `k` plus a remainder `i` (where `i` is 0, 1, 2, 3, 4, or 5). If `i` is 0, 2, 3, or 4, then the number is divisible by 2 or 3. Since primes greater than 3 cannot be divisible by 2 or 3, they must leave a remainder of either 1 or 5 when divided by 6 (i.e. they are of the form `6k+1` or `6k+5`). This observation allows the method to check for primality by only testing numbers in these two forms, significantly reducing the number of required checks.

---

## Conclusion

Each algorithm offers valuable insights into prime number generation:

- **Sieve of Eratosthenes** is ideal for scenarios where simplicity and direct implementation are key, especially for moderate-sized ranges.
- **Sieve of Sundaram** provides a space-efficient alternative by focusing on odd numbers, albeit with an extra transformation step.
- **Optimized Trial Division Method** is particularly efficient for checking the primality of individual numbers, leveraging mathematical insights to minimize redundant checks.

Understanding these methods helps highlight the trade-offs between computational efficiency and implementation complexity in prime number generation.

[Back to Top](#task-4-prime-numbers)


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
