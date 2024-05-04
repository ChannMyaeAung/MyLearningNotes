# Computer Science Essentials



## ASCII

ASCII stands for "American Standard Code for Information Interchange". It is a way of representing characters (letters, numbers, and symbols) as numbers.

Each character has a unique number assigned to it in the ASCII table. For example:

- The letter 'A' has the ASCII value of 65
- The number '5' has the ASCII value of 53
- The symbol '!' has the ASCII value of 33

The ASCII table goes from 0 to 127, covering the most common characters used in English and some basic symbols.

In a computer, characters are stored and manipulated using these ASCII values. When you type a letter on your keyboard, the computer is actually detecting the corresponding ASCII value and using that to represent the character.

This allows computers to work with and process text data, as they can store and manipulate the numeric ASCII values instead of the actual characters. It's a fundamental way that computers handle textual information.

So, in simple terms, ASCII is just a way to assign a unique number to each character, so that computers can understand and work with them. It's a standardized system that allows different computer systems to communicate and process text data in a consistent way.



#### Central Processing Unit (CPU)

- Memory gives the computer a place to store data and program instructions, but it's the CPU, or processor that carries out those instructions.
- A processor implements a set of instructions that programmers can then use to construct meaningful software.
- Types of instructions that CPUs support:
  - Memory access - read, write (to memory)
  - Arithmetic - add, subtract, multiply, divide, increment
  - Logic - AND, OR, NOT
  - Program Flow- jump (to a specific part of a program), call (a subroutine)
- CPU instructions are just operations that the processor can perform.
- Program instructions reside in memory. The CPU reads these instructions so it can run the program.



#### CPU Internals

- Registers - Processor registers are locations within the CPU that hold data during processing.
  - Fast, small temporary storage
  - Compared to accessing main memory, accessing registers is a very fast operation for a CPU but registers can only hold very small amounts of data.
  - The memory cells used in the register file are typically a type of SRAM.
- Arithmetic logic unit(ALU) - performs logical and mathematical operations.
  - The ALU outputs the result of the operation along with a status that provides more detail on execution of the operation.
- Control Unit- directs the CPU, communicating with the processor registers, the ALU, and main memory.
  - Fetch an instruction from memory
  - Decode it
  - Execute it
  - The control unit reads the instruction from the specified memory address, stores the instruction in a register called the instruction register and updates the program counter to point to the next instruction. The control unit then decodes the current instruction, making sense of the 1s and 0s that represent an instruction. 
  - Once decoded, the control unit executes the instruction which may require coordinating with other components in the CPU.
  - Once an instruction has completed, the control unit repeats the cycle: fetch, decode, execute.



#### Clock, Cores and Cache

- Increasing the frequency of the clock allows a CPU to perform more instructions per second.
- Manufacturing processes that led to increased transistor density, which allowed for CPUs with higher clock rates but roughly the same power consumption.
- Multicore CPU, a CPU with multiple processing units called cores focuses on execution of multiple instructions in parallel. 
- A CPU core is effectively an independent processor that resides alongside other independent processors in a single CPU package.
- For example, a four-core CPU - each core has its own registers, ALU and control unit.
- Multiple cores running in parallel is not the same as pipelining which divides instructions into smaller steps so that portions of multiple instructions cal be run in parallel by a single processor.
- The parallelism of multicore means that each core works on a different task, a separate set of instructions.
- In contrast, pipelining happens within each core, allowing portions of multiple instructions to be executed in parallel by a single core.
- Each core added to a processor opens the door to a computer running additional instructions in parallel.
- CPU load data from main memory into registers for processing and then store that data back from registers to memory for later use.
- Programs tend to access the same memory locations over and over.
- Going back to main memory multiple times to access the same data is inefficient. To avoid this inefficiency, a small amount of memory resides within the CPU that holds a copy of data frequently accessed from main memory. This memory is known as a CPU cache.
- The processor checks the cache to see if data it wishes to access is there. If so, the processor can speed things up by reading or writing to the cache rather than to main memory.
- When needed data is not in the cache, the processor can move that data into cache once it has been read from main memory.
- It's common for processors to have multiple cache levels, often three, referred to as L1 cache, L2 cache, and L3 cache.
- A CPU first checks L1 for the needed data, then L2, then L3 before finally going to main memory.
- L1 cache is the fastest to access but it's also the smallest.
- L2 is slower and larger and L3 is slower and larger still.
- Even with these progressively slower levels of cache, it is still slower to access main memory than any level of cache.
- In multicore CPUs, some caches are specific to each core, whereas others are shared among the cores.
- Each core may have its own L1 cache, whereas L2 and L3 caches are shared.



#### Beyond Memory and Processor

- Both memory and CPUs are volatile: they lose state when power is removed.
- A computer with only memory and a processor has no way of interacting with the outside world.
- Secondary storage and I/O devices fill these gaps.
- Secondary storage is nonvolatile and therefore remembers data even when the system is powered down.
- Unlike RAM, secondary storage is not directly addressable by the CPU.
- Such storage is usually much cheaper per byte than RAM, allowing for a large capacity of storage as compared to main memory.
- Secondary storage is considerably slower than RAM; it isn't a suitable replacement for main memory.
- Hard disk drives and solid-state drives are the most common secondary storage devices.
- A hard disk drive (HDD) stores data using magnetism on a rapidly spinning platter, whereas a solid-state drive(SSD) stores data using electrical charges in nonvolatile memory cells.
- SSDs are faster, quieter and more resistant to mechanical failure since SSDs have no moving parts.
- With a secondary storage device in place, a computer can load data on demand.
- When a computer is powered on, the OS loads from secondary storage into main memory; any applications that are set to run at startup also load.
- After startup, when an application is launched, program code loads from secondary storage into main meory.
- The same goes for any user data (documents, music, settings, and so on) stored locally; it must load from secondary storage into main memory before it can be used.
- Secondary storage is often referred to simply as storage while primary/main memory is just called memory or RAM.



#### Input/Output

A computer consisting of a processor, memory and storage doesn't have any way of interacting with the outside world. This is where input/output devices come in.

- An input/output (I/O) device is a component that allows a computer to receive input from the outside world (keyboard, mouse), send data to the outside world (monitor, printer) or both (touch-screen).
- Human interaction with a computer requires going through I/O.
- Computer-to-Computer interaction also requires going through I/O, often in the form of a computer network, such as the internet.
- You may not think of accessing internal storage as I/O, but from the perspective of the CPU, reading or writing to storage is just another I/O operation.
- Reading from the storage device is input while writing to the storage device is output.



#### Bus Communication

- A bus is a hardware communication system used by computer components.
- A bus was simply a set of parallel wires, each carrying an electrical signal.
- This allowed multiple bits of data to be transferred in parallel; the voltage on each wire represented a single bit.
- Today's bus designs aren't always that simple, but the intent is similar.
- There are 3 common bus types used in communication between the CPU, memory and I/O devices.
- An `address bus` acts as a selector for the memory address that the CPU wishes to access.
  - If a program wishes to write to address 0x2FE, the CPU writes 0x2FE to the address bus.
- The `data bus` transmits a value read from memory or a value to be written to memory. 
  - If the CPU wishes to write the value 25 to memory,  then 25 is written to the data bus.
  - Or if the CPU is reading data from memory, the CPU reads the value from the data bus.
- A `control bus` manages operations happening over the other two buses.
  - The CPU uses the control bus to indicate that a write operation is about to happen.
  - Or the control bus can carry a signal indicating the status of an operation.