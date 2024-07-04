
---

title: "Nand2Tetris Week 2 Notes: Boolean Arithmetic"
summary: "An in-depth exploration of Boolean arithmetic, binary numbers, and their applications in digital systems as part of the Nand2Tetris course."
date: "2024-07-04"
tags: ["Nand2Tetris", "Boolean Arithmetic", "Binary Numbers", "Digital Systems", "ALU"]
author: "Katana"
description: "An in-depth exploration of Boolean arithmetic, binary numbers, and their applications in digital systems as part of the Nand2Tetris course."
canonicalURL: "https://h3xkatana.github.io/Blog/post/Nand2Tetris-Week2/"
categories: ["Education", "Computer Science"]

---

### Boolean Arithmetic

Boolean arithmetic deals with binary numbers (0s and 1s) and operations such as AND, OR, NOT, and XOR. These operations are fundamental to digital circuits and computer logic.

- **AND**: The result is 1 if both inputs are 1; otherwise, it is 0.
- **OR**: The result is 1 if at least one input is 1; otherwise, it is 0.
- **NOT**: The result is the inverse of the input (0 becomes 1, 1 becomes 0).
- **XOR**: The result is 1 if only one of the inputs is 1; otherwise, it is 0.

### Important Notes

While both Boolean systems and binary numbers use 0 and 1, Boolean systems focus on logical operations and binary numbers focus on arithmetic and data representation. They are fundamental to digital systems and often work together, such as in the design of ALUs, where Boolean logic controls arithmetic operations on binary numbers.

### Boolean Systems
- **Definition**: Boolean systems deal with logical operations and variables that have two possible values: true (1) and false (0).
- **Operations**: Common operations include AND, OR, NOT, and XOR, which are fundamental to logic gates and digital circuits.
- **Usage**: Boolean logic is primarily used in decision-making processes, control systems, and designing digital circuits like multiplexers, demultiplexers, and arithmetic logic units (ALUs).

### Base-2 (Binary) Numbers
- **Definition**: Binary numbers are a numerical representation system with base 2, using only two digits: 0 and 1.
- **Operations**: Binary arithmetic includes addition, subtraction, multiplication, and division, analogous to decimal arithmetic but limited to the digits 0 and 1.
- **Usage**: Binary numbers are used to represent data and perform arithmetic in digital systems, including computers and other digital devices.

### Integers

Integers in computer systems are represented in binary form. The two common integer representations are:

- **Unsigned Integers**: Represent only non-negative numbers.
- **Signed Integers**: Use methods like Two's Complement to represent both positive and negative numbers.

### Addition

Binary addition follows the same principles as decimal addition but is limited to 0 and 1:

- 0 + 0 = 0
- 0 + 1 = 1
- 1 + 0 = 1
- 1 + 1 = 10 (which is 0 with a carry of 1)

For example, adding binary numbers 1101 and 1011:
```
  1101
+ 1011
------
 11000
```

### Negative Numbers

In digital systems, negative numbers are commonly represented using the **Two's Complement** method. This method allows for straightforward binary arithmetic operations, such as addition and subtraction. Let's break down the process and understand why it works.

#### One's Complement

To find the One's Complement of a binary number, you simply invert all the bits. This means turning all `1`s to `0`s and all `0`s to `1`s.

#### Two's Complement

To find the Two's Complement of a binary number, you add `1` to the result of the One's Complement.

### Example: Representing -5 in 8-bit Binary

Let's represent `-5` using an 8-bit binary number.

1. **Binary Representation of 5**:
   - The binary representation of `5` in 8 bits is `00000101`.

2. **One's Complement of 5**:
   - Invert all bits: `00000101` becomes `11111010`.

3. **Two's Complement of 5**:
   - Add `1` to the One's Complement: 
     - `11111010`
     - `+       1`
     - `__________`
     - `11111011`

So, the 8-bit binary representation of `-5` is `11111011`.

### Example for 3 bits

```
  000 = 0 
  001 = 1  (0...2^(n-1) -1)
  010 = 2
  011 = 3
  100 = -4
  101 = -3  (-1 ... 2^(n-1))
  110 = -2
  111 = -1
```

### Why Two's Complement Works

The Two's Complement method works because it transforms the binary number system into a form where subtraction can be performed using addition.

#### General Formula for Two's Complement

To understand the general formula, consider an `n`-bit binary number `x`. The negative of `x` in Two's Complement can be derived as follows:

\[ \text{Two's Complement of } x = 2^n - x \]

However, since \( 2^n \) in binary is just a `1` followed by `n` zeros (e.g., `100000000` for 8 bits, which is `256` in decimal), we subtract `x` from `2^n`.

#### Simplified Steps

1. **One's Complement**:
   - This can be thought of as \( 2^n - 1 - x \), where \( 2^n - 1 \) is just `11111111` for 8 bits.

2. **Two's Complement**:
   - Add `1` to the One's Complement: \( (2^n - 1 - x) + 1 = 2^n - x \).

### Using Full Adder for Subtraction

With Two's Complement, subtraction becomes addition of a negative number.

#### Example: \( y - x \)

Instead of subtracting `x` from `y`, we add the Two's Complement of `x` to `y`:

\[ y - x = y + (-x) \]

Where \( -x \) is the Two's Complement of `x`.

#### Full Adder

In hardware, this is implemented using a **Full Adder** chip, which can perform binary addition. The full adder handles carries and performs bit-wise addition, making it suitable for both addition and subtraction (using Two's Complement).

### Summary

- **One's Complement**: Invert all bits.
- **Two's Complement**: Add `1` to the One's Complement.
- Two's Complement simplifies binary arithmetic, particularly subtraction.
- Example: Represent `-5` in 8-bit binary as `11111011`.
- Full Adders can be used to perform subtraction by adding the Two's Complement of a number.

This method ensures efficient and straightforward binary arithmetic operations, which are crucial in computer systems and digital electronics.

### Arithmetic Logic Unit (ALU)

The ALU is a critical component of the CPU, responsible for performing arithmetic and logical operations. The ALU can perform:
- **Arithmetic Operations**: Addition, subtraction, multiplication, division.
- **Logical Operations**: AND, OR, NOT, XOR.
- **Bitwise Operations**: Shift left, shift right.

### Example of ALU Implementation

Let's consider a simple 1-bit ALU that can perform AND, OR, and ADD operations based on control signals.

#### Explanation:
1. **Zeroing**: The `zx` and `zy` control signals zero the `x` and `y` inputs, respectively.
2. **Negating**: The `nx` and `ny` control signals negate the `x` and `y` inputs, respectively.
3. **Function**: The `f` control signal selects between AND and ADD operations.
4. **Negate Output**: The `no` control signal negates the output.
5. **Zero and Negative Flags**: The `zr` flag is set if the output is zero, and the `ng` flag is set if the output is negative.