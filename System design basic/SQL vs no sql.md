# SQL vs. NoSQL

> **TL;DR:** **SQL** databases store data in strict tables and are best when accuracy and relationships matter (banking, inventory). **NoSQL** databases store data in flexible formats and are best when you need huge scale, speed, and changing data structures (social media, IoT).

---

## The Simple Idea

A database stores, manages, and retrieves data efficiently. SQL and NoSQL are the two main types, and each is designed for different needs.

| | SQL | NoSQL |
|---|---|---|
| **Think of it as** | An Excel spreadsheet with strict columns | A folder of flexible files or notes |
| **Analogy** | A school register. Every student has the same fields: name, age, class. | A box of index cards. Each card can have different details. |

---

## What Is a SQL Database?

SQL databases store data in **structured tables** with a **predefined schema** (a fixed layout that says what columns exist and what type of data each holds). They are best for complex queries and strong consistency.

**Example table: Users**

| id | name | email |
|---|---|---|
| 1 | Ada | ada@mail.com |
| 2 | Tunde | tunde@mail.com |

**Examples:** MySQL, PostgreSQL

---

## What Is a NoSQL Database?

NoSQL databases use **flexible data models** instead of fixed tables. They are ideal for large-scale, rapidly changing applications.

### The four main types

| Type | How it stores data | Example tool |
|---|---|---|
| **Document** | JSON-like documents | MongoDB |
| **Key-value** | A key and its value, like a dictionary | Redis |
| **Column-family** | Columns grouped together, built for huge data | Cassandra |
| **Graph** | Nodes and the connections between them | Neo4j |

**Example document (MongoDB):**

```json
{
  "name": "Ada",
  "email": "ada@mail.com",
  "hobbies": ["music", "football"]
}
```

Another document in the same collection could have different fields, and that is fine.

---

## Main Differences, One by One

### 1. How data is stored
- **SQL:** tables with rows and columns.
- **NoSQL:** documents, key-value pairs, graphs, or columns.

### 2. Data model
- **SQL:** relational. Tables are linked to each other through keys.
- **NoSQL:** non-relational. Data is usually kept together in one place.

### 3. Schema
- **SQL:** fixed. You must define the structure first, and changing it later can be hard.
- **NoSQL:** flexible. You can add new fields anytime without redesigning everything.

### 4. Joins
- **SQL:** supports complex joins, which combine data from several tables in one query.
- **NoSQL:** generally avoids joins. Related data is often stored together instead.

### 5. Transactions
- **SQL:** follows **ACID** properties.
- **NoSQL:** usually follows the **BASE** model.

| ACID (SQL) | Meaning |
|---|---|
| **Atomicity** | A transaction fully succeeds or fully fails. No half-done work. |
| **Consistency** | Data always follows the rules after a transaction. |
| **Isolation** | Transactions running at the same time don't interfere with each other. |
| **Durability** | Once saved, data stays saved, even after a crash. |

| BASE (NoSQL) | Meaning |
|---|---|
| **Basically Available** | The system stays up and responds, even during partial failures. |
| **Soft state** | Data may change over time, even without new input, as copies sync up. |
| **Eventually consistent** | All copies of the data will match after a short while, not instantly. |

### 6. Scaling
- **SQL:** scales **vertically**, by making one server stronger (more CPU, RAM, storage).
- **NoSQL:** scales **horizontally**, by adding more servers.

### 7. Type of data
- **SQL:** best for structured data.
- **NoSQL:** suits structured, semi-structured, and unstructured data.

### 8. Typical uses
- **SQL:** banking, ERP (business management software), and CRM (customer management) systems.
- **NoSQL:** social media, Big Data, and IoT applications.

### 9. Examples
- **SQL:** MySQL, PostgreSQL
- **NoSQL:** MongoDB, Cassandra

---

## Quick Comparison Table

| SQL | NoSQL |
|---|---|
| Data is stored in tables with rows and columns | Data is stored as documents, key-value pairs, graphs, or columns |
| Relational data model | Non-relational data model |
| Fixed schema | Flexible schema |
| Supports complex joins | Generally avoids joins |
| Follows ACID | Usually follows BASE |
| Scales vertically | Scales horizontally |
| Best for structured data | Suits structured, semi-structured, and unstructured data |
| Banking, ERP, CRM | Social media, Big Data, IoT |
| MySQL, PostgreSQL | MongoDB, Cassandra |

---

## Choosing Between SQL and NoSQL

The choice depends on what your application needs.

### Choose SQL when
- Data consistency is critical.
- Complex joins and transactions are required.
- The data structure is well-defined.
- You are building banking, ERP, or inventory management systems.

### Choose NoSQL when
- The application needs high scalability.
- Data is unstructured or semi-structured.
- Real-time processing is required.
- Frequent schema changes are expected.

---

## Important Points the Article Leaves Out

- **SQL is a language, not only a database type.** SQL stands for Structured Query Language. "NoSQL" is often read as "Not Only SQL," since some NoSQL databases also offer SQL-like queries.
- **The gap is shrinking.** Modern PostgreSQL can store JSON documents, and some NoSQL databases (like MongoDB) now support multi-document transactions. Many systems blur the line.
- **SQL can scale horizontally too**, using read replicas, sharding (splitting data across servers), and distributed SQL databases such as CockroachDB. It is just harder than in most NoSQL systems.
- **NoSQL trades consistency for speed and scale.** This is the CAP theorem idea: in a distributed system you cannot have perfect Consistency, Availability, and Partition tolerance all at once. NoSQL databases often pick availability over instant consistency.
- **Data duplication in NoSQL.** Because joins are avoided, the same data is often copied into several places. That is fast to read but harder to keep updated everywhere.
- **Schema flexibility has a cost.** With no enforced structure, messy or inconsistent data can slip in unless your application code checks it.
- **Money and critical records.** For payments, balances, and anything where being wrong is unacceptable, SQL is the safe default because of ACID.
- **Using both is common.** Many real systems use SQL for core business data (orders, payments, users) and NoSQL for caching (Redis), search, logs, or activity feeds. This is called **polyglot persistence**.
- **Query languages differ.** SQL is standard across databases. Each NoSQL database has its own query style, so skills don't transfer as easily.
- **When unsure, start with SQL.** It is mature, well understood, and handles most apps fine. Move to NoSQL when you have a clear need for scale or flexible data.

---

## Common Tools

| Category | Tools |
|---|---|
| **SQL** | MySQL, PostgreSQL, SQL Server, Oracle, SQLite |
| **Document** | MongoDB, CouchDB |
| **Key-value** | Redis, DynamoDB |
| **Column-family** | Cassandra, HBase |
| **Graph** | Neo4j, Amazon Neptune |

---

## Real-Life Examples

| Application | Likely choice | Why |
|---|---|---|
| Bank account system | SQL | Accuracy and transactions matter most |
| Online store orders and payments | SQL | Strong consistency and relationships |
| Social media feed | NoSQL | Huge scale and changing data shapes |
| IoT sensor data | NoSQL | Massive volumes of fast-arriving data |
| Shopping cart or session cache | NoSQL (key-value) | Very fast reads and writes |
| Friend recommendations | NoSQL (graph) | Relationships are the main data |

---

## Final Summary

| Question | Answer |
|---|---|
| Need strict accuracy and complex relationships? | **SQL** |
| Need massive scale and flexible data? | **NoSQL** |
| Not sure? | Start with **SQL** |
| Large real system? | Often **both** |
