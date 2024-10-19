# **MongoDB Fundamentals:**

MongoDB is a **NoSQL**, document-oriented database that stores data in flexible, JSON-like documents, ideal for unstructured or semi-structured data.

#### 1. **Documents**:

- MongoDB stores data as **documents**, which are JSON-like objects in the BSON (Binary JSON) format. Documents are flexible, meaning we don't need to define a strict schema upfront.

- Example document:

  ```json
  {
    "_id": "123",
    "name": "Alice",
    "age": 25,
    "interests": ["music", "sports"]
  }
  ```

#### 2. **Collections**:

- A **collection** in MongoDB is a group of documents. Collections are similar to tables in SQL, but they don't enforce a rigid schema. Documents in the same collection can have different fields or data types.

#### 3. **NoSQL & Schema-less**:

- MongoDB is schema-less, meaning we can add, modify, or remove fields without modifying the entire structure. This provides flexibility when handling evolving data models.

#### 4. **Indexing**:

- Indexes improve the performance of queries. MongoDB allows creating indexes on fields to speed up searches, much like in relational databases.

#### 5. **Sharding (Horizontal Scaling)**:

- MongoDB supports **sharding**, which distributes data across multiple servers. This allows MongoDB to handle large datasets by splitting data across different physical machines.

#### 6. **Replica Sets (High Availability)**:

- A **replica set** is a group of MongoDB instances that maintain the same data. One primary node accepts writes, while secondary nodes replicate the data for redundancy and failover.

#### 7. **Aggregation**:

- MongoDB provides a powerful **aggregation framework** for transforming and analyzing data. Aggregations process data records and return computed results, similar to SQL’s `GROUP BY`, but more flexible.

#### 8. **Query Language**:

- MongoDB uses its own query language, which is JSON-like. Instead of SQL commands, we use MongoDB queries to interact with the database, like:

  ```js
  db.users.find({ age: { $gte: 20 } })
  ```

#### 9. **Use Cases**:

- Best suited for applications with unstructured data, flexible schemas, and where horizontal scaling is important. Commonly used in real-time analytics, content management systems, and e-commerce platforms.