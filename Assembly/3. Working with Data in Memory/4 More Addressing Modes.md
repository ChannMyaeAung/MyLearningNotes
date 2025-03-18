# More Addressing Modes

- Using just VALUE alone is the direct addressing mode, and using (BASEREG) is the register indirect addressing mode.
- Other common modes include:

**Displacement (Base) Addressing (64-bit):**

- Combines a base register with a constant offset (displacement) to compute the effective address.

- Example:

  ```assembly
  movq 16(%rax), %rbx  # Load the quadword from the address (%rax + 16) into %rbx
  ```

**Indexed Addressing (64-bit):**

- Uses a base register, an index register, and a scale factor to compute the effective address.

- Example:

  ```assembly
  movq (%rax, %rcx, 8), %rdx  # Effective address = %rax + (%rcx * 8); load quadword into %rdx
  ```

**RIP-Relative Addressing (64-bit):**

- The effective address is determined relative to the current instruction pointer, ideal for position-independent code.

- Example:

  ```assembly
  movq myData(%rip), %rax  # Load the quad
  ```