### Comparison: Merge Sort vs. Selection Sort

| Feature          | Selection Sort           | Merge Sort                   |
| ---------------- | ------------------------ | ---------------------------- |
| Time Complexity  | O(n²)                    | O(n log n)                   |
| Space Complexity | O(1) (in-place)          | O(n) (not in place)          |
| Stability        | Not stable (can be made) | Stable                       |
| Divide & Conquer | No                       | Yes                          |
| Best Use Case    | Small datasets           | Large datasets, linked lists |
| Implementation   | Simple                   | More complex                 |
| Number of Swaps  | O(n)                     | O(n log n)                   |



### **Key Takeaways:**

- **Selection Sort** is straightforward and requires minimal additional memory, making it suitable for small or nearly sorted datasets where its inefficiency is less impactful.
- **Merge Sort** is highly efficient for large datasets and ensures stability, but it requires additional memory and a more complex implementation.