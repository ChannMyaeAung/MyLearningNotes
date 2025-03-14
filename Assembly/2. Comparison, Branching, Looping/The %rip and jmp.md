# The %rip Register and the jmp Instruction

- %rip (a special-purpose register) - the instruction pointer simply points to the next memory location that the processor is going to process an instruction from.
  - This lets the CPU know where to pull the next instruction from when the next clock cycle runs.
- This register can be manipulated thru `jump` instructions.
- A jump instruction tells the computer to alter the normal sequential flow of program execution.
- Instead of executing instructions one after another, a jump instruction directs the CPU to continue execution from a different location in memory. 
  - Fundamental for creating control flow constructs such as loops, conditionals, and function calls.

## Code Example

```assembly
.globl _start

.section .text
_start:
    movq $7, %rdi
    jmp nextplace

    # These two instructions are skipped
    movq $8, %rbx
    addq %rbx, %rdi

nextplace:
    movq $60, %rax
    syscall
```

```sh
chan@CMA:~/C_Programming/Assembly$ as test.s -o test.o
test.s: Assembler messages:
test.s: Warning: end of file not at end of a line; newline inserted
chan@CMA:~/C_Programming/Assembly$ ld test.o -o test
chan@CMA:~/C_Programming/Assembly$ ./test
chan@CMA:~/C_Programming/Assembly$ echo $?
7
```

