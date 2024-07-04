
---

title: "Nand2Tetris Week 1 Notes"
summary: "A comprehensive guide to understanding the basics of Boolean logic, truth tables, and implementing logic gates in the Nand2Tetris course."
date: "2024-07-03"
tags: ["Nand2Tetris", "Boolean Logic", "Truth Tables", "Logic Gates", "HDL"]
author: "Katana"
description: "A comprehensive guide to understanding the basics of Boolean logic, truth tables, and implementing logic gates in the Nand2Tetris course."
canonicalURL: "https://h3xkatana.github.io/Blog/post/Nand2Tetris-Week1/"
categories: ["Education", "Computer Science"]
showToc: true  # Enable Table of Contents
tocOpen: true


---

### From Functions to Truth Tables
1. **Identify Variables**: Determine all variables involved in the function.
2. **List Possible Combinations**: For each variable, list all possible combinations. The number of combinations is \(2^n\), where \(n\) is the number of variables.
3. **Construct Truth Table**: Create a truth table that includes all combinations of input variables and their corresponding output values for the given function.

### From Truth Tables to Functions
1. **Identify Terms**: Each term in the function corresponds to a row in the truth table where the function's output is 1.
2. **Create Sub-Forms**:
    - For each row where the function output is 1, create a term composed of the input variables.
    - Use AND gates to combine the variables within each term.
    - If a variable is 0 in a row, use its NOT (complement) in that term.
3. **Combine Terms with OR Gates**: Use OR gates to combine all the terms created in the previous step. This forms the final function.

### Implementing Functions with Logic Gates
1. **Basic Logic Gates**:
    - **NOT Gate**: Inverts the input.
    - **AND Gate**: Outputs 1 only if all inputs are 1.
    - **OR Gate**: Outputs 1 if at least one input is 1.

2. **NAND Gate**:
    - A universal gate that can be used to implement any logic function.
    - **NOT using NAND**: \((x \text{ NAND } x) = \text{NOT}(x)\)
    - **AND using NAND**: \(x \text{ AND } y = \text{NOT}(x \text{ NAND } y)\)
    - **OR using NAND**: \(\text{NOT}(x) \text{ NAND } \text{NOT}(y) = x \text{ OR } y\)

3. **Summary**:
    - All functions can be implemented using NOT, AND, and OR gates.
    - Any function can also be implemented using only NAND gates due to their versatility.

### HDL (Hardware Description Language)
Purpose: Simplifies the implementation of gates and chips by describing their behavior and structure in a high-level language. It is almost like Java syntax for enums or some classes.

Example: Implementation of an XOR gate using HDL based on NOT, AND, and OR gates. An automatic testing language will test all the possible results against the expected results to see the behavior of our chip.

#### XOR Gate HDL Example
```hdl
CHIP Xor {
    IN a, b;    // Input pins
    OUT out;    // Output pin

    PARTS:
    // Intermediate signals
    NOT(a=a, out=nota);
    NOT(b=b, out=notb);

    AND(a=a, b=notb, out=and1);
    AND(a=nota, b=b, out=and2);

    OR(a=and1, b=and2, out=out);
}
```