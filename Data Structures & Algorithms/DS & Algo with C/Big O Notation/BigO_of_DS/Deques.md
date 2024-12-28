### **Deque (Double-Ended Queue)**

A **deque** allows insertion and deletion at both ends (front and rear).

**Common Operations:**

- Push Front (Add an element to the front):
  - **Time Complexity:** O(1)
- Push Back (Add an element to the rear):
  - **Time Complexity:** O(1)
- Pop Front (Remove the front element):
  - **Time Complexity:** O(1)
- Pop Back (Remove the rear element):
  - **Time Complexity:** O(1)
- Front/Peek Front (View the front element):
  - **Time Complexity:** O(1)
- Back/Peek Back (View the rear element):
  - **Time Complexity:** O(1)
- isEmpty (Check if the deque is empty):
  - **Time Complexity:** O(1)

**Implementation Notes:**

- Commonly implemented using doubly linked lists or dynamic arrays with circular buffering.
- Both ends are accessible in constant time.