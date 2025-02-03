# Execution contexts withing the kernel

Kernel code always executes in one of two contexts:

- Process
- Interrupt

## Process context

- When invoking kernel services by issuing a system call, the calling process runs the kernel code of the system call in kernel mode. This is called **process context** - kernel code is now running in the context of the process that invoked the system call.
- Process context code has the following attributes:
  - Always triggered by a process (or thread) issuing a system call
  - Top-down approach
  - Synchronous execution of kernel code by a process.

## Interrupt Context

- In system programming, **interrupt context** refers to the execution state when the processor handles an interrupt—an asynchronous event triggered by hardware (or sometimes software) that temporarily halts the current execution flow.
  - For example, a network packet destined for our Ethernet MAC address arrives at the hardware adaptor, it must now let the Network Interface Card (NIC) device driver know, so that it can fetch and process packets. It kicks the NIC driver into action by asserting a hardware interrupt.
  - **Device drivers reside in kernel-space**, and therefore their code runs in Supervisor or kernel Mode. 
  - The (kernel privilege) driver code **Interrupt service routine (ISR)** now executes, fetches the packet, and sends it up the OS network protocol stack for processing.

- The interrupt context code has the following attributes:
  - Always triggered by a hardware interrupt (not a software interrupt, fault or exception; that's still process context).
  - Bottom-up approach
  - Asynchronous execution of kernel code by an interrupt