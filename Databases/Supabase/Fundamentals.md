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

## **Supabase Core Concepts**:

- **PostgreSQL Database**: Supabase uses PostgreSQL, an open-source relational database.
- **Realtime**: Enables real-time subscriptions to database changes via WebSockets.
- **Authentication**: Provides user management with support for email/password, OAuth providers, and more.
- **Storage**: Offers file storage for managing files like images and videos.
- **Edge Functions**: Serverless functions that run close to the user for low-latency responses.
- **API**: Automatically generates RESTful APIs for your database tables.
- **SDKs**: Client libraries for interacting with Supabase services in various languages.
- **Row-Level Security (RLS)**: Security feature to control access at the row level in tables.



### Core Features:

```typescript
// Authentication
const { data: { user } } = await supabase.auth.signUp({
  email: 'user@example.com',
  password: 'password123'
})

// Database Operations
const { data, error } = await supabase
  .from('posts')
  .select('*')
  .eq('author_id', user.id)

// Real-time Subscriptions
const subscription = supabase
  .from('posts')
  .on('INSERT', payload => {
    console.log('New post:', payload.new)
  })
  .subscribe()
```

### Key differences from MongoDB:

1. **Schema Definition**:

   ```sql
   -- Supabase/PostgreSQL
   CREATE TABLE posts (
     id UUID DEFAULT gen_random_uuid(),
     title TEXT NOT NULL,
     content TEXT,
     author_id UUID REFERENCES users(id)
   );
   
   -- vs MongoDB (schema-less)
   {
     title: String,
     content: String,
     author: ObjectId
   }
   ```

2. **Relationships**:

   ```typescript
   // Supabase - Using joins
   const { data } = await supabase
     .from('posts')
     .select(`
       *,
       users (
         name,
         email
       )
     `)
   
   // vs MongoDB - Using population
   await Post.findOne().populate('author')
   ```

3. **Core Supabase Features**:

   - **Authentication**

     ```typescript
     // Sign Up
     const { data, error } = await supabase.auth.signUp({
       email: 'user@example.com',
       password: 'password123',
       options: {
         data: {
           name: 'John Doe'
         }
       }
     })
     
     // Sign In
     const { data, error } = await supabase.auth.signInWithPassword({
       email: 'user@example.com',
       password: 'password123'
     })
     ```

   - **Row Level Security (RLS)**:

     ```sql
     -- Example RLS policy
     CREATE POLICY "Users can only see their own posts"
     ON posts
     FOR SELECT
     USING (auth.uid() = author_id);
     ```

   - **Storage**:

     ```typescript
     // Upload file
     const { data, error } = await supabase
       .storage
       .from('bucket-name')
       .upload('file-path', file)
     
     // Get public URL
     const { data } = supabase
       .storage
       .from('bucket-name')
       .getPublicUrl('file-path')
     ```

4. **Best Practices**:

   - **Database Design**:

     ```sql
     -- Use UUIDs for primary keys
     CREATE TABLE profiles (
       id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
       user_id UUID REFERENCES auth.users,
       username TEXT UNIQUE
     );
     
     -- Add timestamps
     CREATE TABLE posts (
       id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
       created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW()),
       updated_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW())
     );
     ```

   - **Security**:

     ```sql
     -- Always use RLS
     ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
     
     -- Create policies
     CREATE POLICY "Public profiles are viewable by everyone"
     ON profiles FOR SELECT
     USING (is_public = true);
     ```

Key Concepts to Remember:

1. Database Paradigm:

   - Supabase uses PostgreSQL (relational)

   - Structured data with predefined schemas

   - Strong relationships between tables

2. Authentication:

   - Built-in auth system

   - Multiple providers (email, social)

   - JWT-based sessions

3. Security:

   - Row Level Security (RLS)

   - Policies for access control

   - Database roles and permissions

4. Real-time:

   - Built-in real-time subscriptions

   - Channel-based broadcasts

   - Filtered real-time updates