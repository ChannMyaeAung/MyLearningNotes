### **Queue**

A **queue** is a First-In-First-Out (FIFO) data structure. It allows insertion at the rear and removal from the front.

**Common Operations:**

- Enqueue (Add an element to the rear):
  - **Time Complexity:** O(1)
- Dequeue (Remove the front element):
  - **Time Complexity:** O(1)
- Front/Peek (View the front element without removing it):
  - **Time Complexity:** O(1)
- isEmpty (Check if the queue is empty):
  - **Time Complexity:** O(1)

**Implementation Notes:**

- Can be implemented using linked lists (with pointers to both head and tail) or circular buffers.
- Both implementations ensure constant time operations for enqueue and dequeue.