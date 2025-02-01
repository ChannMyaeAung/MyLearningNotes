# Application Binary Interface

Let's imagine that we write a C program, and run it on the machine. 

- C code cannot possibly be directly deciphered by the CPU. It must be converted into machine language. 
- So, we understand that on modern systems we will have a toolchain installed – this includes the compiler, linker, library objects, and various other tools. 
- We compile and link the C source code, converting it into an executable format that can be run on the system.
- The processor **Instruction Set Architecture (ISA)** – documents the machine's instruction formats, the addressing schemes it supports, and its register model. 
- In fact, CPU **Original Equipment Manufacturers (OEMs)** release a document that describes how the machine works; this document is generally called the **ABI**. 
- The **ABI** describes more than just the ISA. it describes the machine instruction formats, the register set details, the calling convention, the linking semantics, and the executable file format, such as ELF. 
  