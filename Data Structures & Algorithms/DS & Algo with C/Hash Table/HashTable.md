## Hash Table

A **Hash Table** (also known as a **Hash Map**) is a data structure that implements an **associative array**—a structure that can map **keys** to **values**. It allows for **efficient** insertion, deletion, and lookup operations, typically achieving **O(1)** time complexity on average.

**Key Characteristics:**

- **Key-Value Pairs:** Each entry in the hash table consists of a unique key and its associated value.
- **Hash Function:** A function that converts a key into an index in the underlying array.
- **Buckets:** The array that holds the data. Each bucket can store one or more key-value pairs.
- **Collision Handling:** Mechanisms to handle scenarios where multiple keys hash to the same index.

**Real-World Analogy:**

Imagine a library where books (values) are stored on shelves (buckets). Each book has a unique ISBN (key). The hash function determines which shelf a book should go on based on its ISBN. If two books have ISBNs that hash to the same shelf, collision handling ensures both books are stored without loss.

### Components of a Hash Table

1. **Keys and Values:**
   - **Key:** A unique identifier used to store and retrieve associated data.
   - **Value:** The data associated with a key.
2. **Hash Function:**
   - Converts a key into an index within the hash table's array.
   - Should distribute keys uniformly to minimize collisions.
3. **Buckets (Array):**
   - An array where each element is a bucket that can store key-value pairs.
   - The size of the array is typically a prime number to reduce collisions.
4. **Collision Handling Mechanism:**
   - **Separate Chaining:** Each bucket contains a linked list of entries that hash to the same index.
   - **Open Addressing:** Finds another slot within the array using probing methods (linear, quadratic, double hashing).

### Hash Functions

A **hash function** is crucial for the performance of a hash table. It must efficiently convert keys into valid array indices with minimal collisions.

**Properties of a Good Hash Function:**

- **Deterministic:** The same key always hashes to the same index.
- **Uniform Distribution:** Hashes keys uniformly across the array to minimize collisions.
- **Efficient:** Computes quickly, especially for large datasets.

**Simple Example:**

A basic hash function for integer keys might be:

```C
int hashFunction(int key, int size){
    return key % size;
}
```

- **Explanation:** It divides the key by the size of the hash table and returns the remainder as the index.

### Collision Handling Techniques

Collisions occur when two distinct keys hash to the same index. Effective collision handling ensures that all key-value pairs are stored and retrieved correctly.

#### Separate Chaining

**Description:**

- Each bucket in the array contains a linked list.
- All entries that hash to the same index are stored in the linked list for that bucket.

**Advantages:**

- Simple to implement.
- Efficient when the load factor (number of entries divided by number of buckets) is low.
- Can handle a large number of collisions gracefully.

**Disadvantages:**

- Requires additional memory for pointers in the linked lists.
- Performance degrades if many keys hash to the same bucket.

**Visualization:**

```yaml
Hash Table Array:
Index 0: [Key1, Value1] -> [Key2, Value2] -> NULL
Index 1: NULL
Index 2: [Key3, Value3] -> NULL
...
```

### Open Addressing

**Description:**

- All key-value pairs are stored directly within the array.
- When a collision occurs, the hash table probes for the next available slot using a probing sequence.

**Common Probing Methods:**

1. **Linear Probing:**
   - Moves sequentially through the array to find the next available slot.
   - **Formula:** `(hash + i) % size` where `i` is the probe number.
2. **Quadratic Probing:**
   - Uses a quadratic function to determine the probe sequence.
   - **Formula:** `(hash + i^2) % size`.
3. **Double Hashing:**
   - Uses a second hash function to determine the probe step size.
   - **Formula:** `(hash1 + i * hash2(key)) % size`.

**Advantages:**

- No additional memory overhead for pointers.
- Better cache performance due to data locality.

**Disadvantages:**

- More complex to implement.
- Clustering can occur, especially with linear probing.
- Deletion can be tricky to handle.

**Visualization:**

```yaml
Hash Table Array:
Index 0: [Key1, Value1]
Index 1: [Key2, Value2]
Index 2: NULL
...
```

### Basic Operations

#### Insertion

**Separate Chaining:**

1. Compute the hash index using the hash function.
2. Insert the key-value pair at the beginning of the linked list in the corresponding bucket.

**Open Addressing (Linear Probing Example):**

1. Compute the hash index.
2. If the slot is occupied, probe the next slot sequentially.
3. Insert the key-value pair in the first available slot.

#### Search

**Separate Chaining:**

1. Compute the hash index.
2. Traverse the linked list in the corresponding bucket to find the key.

**Open Addressing (Linear Probing Example):**

1. Compute the hash index.
2. If the key at the index matches, return the value.
3. If not, probe the next slot sequentially until the key is found or an empty slot is encountered.

#### Deletion

**Separate Chaining:**

1. Compute the hash index.
2. Traverse the linked list in the corresponding bucket to find the key.
3. Remove the node containing the key from the linked list.

**Open Addressing (Linear Probing Example):**

1. Compute the hash index.
2. Find the key using the search method.
3. Remove the key-value pair and handle subsequent probes to maintain the integrity of the probe sequence.

### Use Cases of Hash Tables

Hash tables are versatile and widely used in various applications due to their efficient average-case performance for insertion, deletion, and search operations. Here are some common real-world use cases:

1. **Databases:**
   - **Indexing:** Hash indexes allow quick retrieval of records based on key values.
   - **Caching:** Frequently accessed data is stored in a hash table to speed up queries.
2. **Programming Languages:**
   - **Symbol Tables:** Compilers use hash tables to keep track of identifiers (variables, functions).
   - **Dictionaries:** Languages like Python and Java use hash tables for their dictionary data types.
3. **Networking:**
   - **Routing Tables:** Routers use hash tables to store and quickly access routing information.
   - **DNS Caching:** Caches domain name system (DNS) lookups using hash tables for faster resolution.
4. **Operating Systems:**
   - **Process Management:** Quick access to process information using process IDs as keys.
   - **Memory Management:** Hash tables can manage memory pages and their statuses.
5. **Web Development:**
   - **Session Management:** Storing user sessions with session IDs as keys.
   - **URL Routing:** Mapping URLs to handler functions efficiently.
6. **Game Development:**
   - **Collision Detection:** Efficiently managing and querying game objects.
   - **Resource Management:** Storing and accessing game assets like textures and sounds.
7. **Security:**
   - **Hash Tables for Passwords:** Storing hashed passwords for quick verification.
   - **Data Integrity:** Using hash functions to verify the integrity of data.
8. **Bioinformatics:**
   - **Genomic Data Storage:** Efficiently storing and querying large genomic datasets.
