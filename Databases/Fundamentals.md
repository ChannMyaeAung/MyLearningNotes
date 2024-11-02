# Database Fundamental Concepts

## Schema & Query

- In the context of databases, both **schema** and **query** are fundamental concepts that define the structure of the data and how we interact with it.

### What is a Schema? 

A **schema** is essentially the blueprint or structure of a database. It defines how data is organized, including:

- **Tables** (or collections, in NoSQL databases).
- **Columns** (fields) and their **data types** (e.g., integer, string, date).
- **Relationships** between tables (in relational databases like SQL).
- **Constraints** (e.g., primary keys, foreign keys, unique constraints).

In simpler terms, a schema sets the rules for how data is stored and related within the database.

#### Example:

In a relational database like PostgreSQL or MySQL, the schema might define:

```sql
CREATE TABLE Users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Orders (
  id SERIAL PRIMARY KEY,
  user_id INT REFERENCES Users(id),
  product_name VARCHAR(100),
  amount INT,
  order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

```

In this case:

- The **schema** defines two tables: `Users` and `Orders`.
- It specifies the columns in each table and the **data types** for each field.
- There’s a **relationship** between the `Orders` and `Users` tables (`user_id` is a foreign key referencing the `id` column in `Users`).

#### Schema in NoSQL Databases:

- In a NoSQL database like MongoDB, the concept of a schema is more flexible. 
- Documents can have different structures, but we can still define schemas using libraries like **Mongoose** for validation and consistency:

```javascript
const userSchema = new mongoose.Schema({
  name: String,
  email: { type: String, unique: true },
  createdAt: { type: Date, default: Date.now }
});

```



### What is a Query?

A **query** is a request for data from the database. It’s the way we interact with the data stored in our database, such as:

- **Fetching** data (retrieving records from tables or collections).
- **Inserting** new records.
- **Updating** existing records.
- **Deleting** records.

Queries are written in a **query language**, depending on the type of database we’re using. For relational databases like MySQL or PostgreSQL, queries are written in **SQL** (Structured Query Language). In NoSQL databases, queries are written using their respective query syntax (like MongoDB's query language).

**Example Queries in SQL**:

- **Fetching data**:

  ```sql
  SELECT * FROM Users WHERE id = 1;
  ```

- **Inserting data**:

  ```sql
  INSERT INTO Users (name, email) VALUES ('Alice', 'alice@example.com');
  ```

- **Updating data**:

  ```sql
  UPDATE Users SET email = 'alice_new@example.com' WHERE id = 1;
  ```

- **Deleting data**:

  ```sql
  DELETE FROM Users WHERE id = 1;
  ```

#### Example Queries in MongoDB (NoSQL):

- **Fetching data**:

  ```javascript
  db.users.find({ name: 'Alice' });
  ```

- **Inserting data**:

  ```javascript
  db.users.insert({ name: 'Alice', email: 'alice@example.com' });
  ```

- **Updating data**:

  ```javascript
  db.users.update({ name: 'Alice' }, { $set: { email: 'alice_new@example.com' } });
  ```

- **Deleting data**:

  ```javascript
  db.users.remove({ name: 'Alice' });
  ```

### **Key Differences**:

- **Schema** is about **structure**: It defines how data is organized and related.
- **Query** is about **interaction**: It defines how we retrieve, insert, update, or delete data in the database.

### **Conclusion**:

- **Schema** = Blueprint/structure of the database.
- **Query** = How we request or manipulate data in the database.

Understanding both is essential to working with databases, whether it's designing data structures or interacting with them to build applications.



## **Core Database Fundamentals**:

- **Database**: An organized collection of structured data stored electronically.
- **Tables**: Structures within a database that store data in rows and columns.
- **SQL (Structured Query Language)**: A standard language for accessing and manipulating relational databases.
- **CRUD Operations**: The basic operations—Create, Read, Update, Delete—for managing data.
- **Relationships**: Associations between tables defined by foreign keys.
- **Joins**: Methods to combine rows from two or more tables based on related columns.
- **Indexes**: Data structures that improve the speed of data retrieval operations.
- **Transactions**: Sequences of database operations that are treated as a single unit.

## Core Database Concepts

1. **Relational vs Non-Relational Databases:**

   ```
   MongoDB (Non-Relational)         PostgreSQL/Supabase (Relational)
   -------------------------        -----------------------------
   Collections                      Tables
   Documents                        Rows
   Fields                          Columns
   Nested Documents                Foreign Keys & Joins
   Flexible Schema                 Strict Schema
   ```

2. **Key Database Concepts:**

   - **ACID Properties**:

     - Atomicity: Transactions are all-or-nothing

     - Consistency: Data remains valid after transactions

     - Isolation: Transactions don't interfere

     - Durability: Completed transactions are permanent.

   - **Relations & Keys**:

     ```sql
     -- Example of related tables
     users (id, name, email)
     posts (id, user_id, content)  -- user_id is a foreign key
     ```

   - **Normalization**: Organizing data to reduce redundancy

     - First Normal Form (1NF): No repeating groups
     - Second Normal Form (2NF): No partial dependencies
     - Third Normal Form (3NF): No transitive dependencies