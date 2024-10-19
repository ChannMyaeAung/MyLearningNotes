# **Supabase (PostgreSQL) Fundamentals**:

Supabase is built on **PostgreSQL**, which is a **relational** (SQL) database with strong data integrity and complex query capabilities.

#### 1. **Tables**:

- In PostgreSQL, data is organized into **tables**, which have a strict schema. Each table consists of rows (records) and columns (fields). All records in a table must conform to the defined schema.

#### 2. **SQL (Structured Query Language)**:

- PostgreSQL uses **SQL** to query and manipulate data. SQL is a powerful, declarative language used to define, manipulate, and retrieve data from relational databases.

- Example query:

  ```sql
  SELECT name, age FROM users WHERE age >= 20;
  ```

#### 3. **Schema**:

- PostgreSQL enforces a **schema**, which means we must define the structure of our data (columns, types, etc.) upfront. This ensures data integrity and consistency.

#### 4. **Relations & Foreign Keys**:

- PostgreSQL supports **relationships** between tables through foreign keys. This allows us to create complex models where data in one table references data in another.
- Example: A "users" table and an "orders" table might be related, where each order references a user.

#### 5. **Indexes**:

- Like MongoDB, PostgreSQL uses **indexes** to speed up queries. We can create indexes on one or more columns to improve query performance.

#### 6. **Joins**:

- PostgreSQL excels at **joins**, which allow us to combine data from multiple tables in a single query. Joins are essential for complex data relationships.

- Example:

  ```sql
  SELECT users.name, orders.product
  FROM users
  JOIN orders ON users.id = orders.user_id;
  ```

#### 7. **Transactions & ACID Compliance**:

- PostgreSQL is **ACID-compliant**, ensuring data integrity through **Transactions**. This means it guarantees **Atomicity, Consistency, Isolation, and Durability** for every operation, making it reliable for mission-critical applications.

- Example:

  ```sql
  BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
  COMMIT;
  ```

#### 8. **Real-time Features (via Supabase)**:

- Supabase provides **real-time** features on top of PostgreSQL using WebSockets. Changes to the database can be pushed instantly to clients, ideal for building collaborative apps, chats, or live dashboards.

#### 9. **Authentication & Authorization (via Supabase)**:

- Supabase offers built-in **authentication** (with JWT, OAuth) and **authorization** features, simplifying user management. This makes it easy to secure our application without custom backend development.

#### 10. **Replication & Scalability**:

- PostgreSQL supports **replication** (master-slave or synchronous replication) for high availability, but it is typically scaled **vertically**. While we can scale horizontally, it's more challenging compared to MongoDB’s sharding capabilities.

#### 11. **Use Cases**:

- Best suited for structured data, where relationships between entities are important and data integrity is critical. Commonly used for enterprise applications, financial systems, and any application requiring complex querying and analytics.