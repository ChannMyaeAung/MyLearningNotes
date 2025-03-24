# Additional Flags for Signed Integers

- In addition to the carry flag (CF) and zero flag (ZF):
  - **Overflow flag (OF)**: 
    - Indicates that a signed arithmetic operation produced a result that exceeds the range that can be represented in the destination register.
    - For addition, if two positive numbers yield a negative result or two negative numbers yield a positive result, OF is set.
    - It helps detect errors in signed arithmetic, especially when the calculated value is too large (or too small) to fit
  - **Sign Flag (SF)**:
    - Reflects the sign of the result of an operation.
    - It is typically the most significant bit (MSB) of the result.
    - If SF is set (1), the result is considered negative; if clear (0), the result is non-negative.
- Different jump instructions check different flags if we are treating the values as signed instead of unsigned values.

---

