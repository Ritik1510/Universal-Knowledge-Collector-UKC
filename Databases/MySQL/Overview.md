# MySQL — Complete Overview

## 1. What Is MySQL

MySQL is an open-source relational database management system (RDBMS) that uses SQL (Structured Query Language) to define, manipulate, and query data. It's owned by Oracle Corporation (since acquiring Sun Microsystems in 2010), and is one of the most widely used databases in the world — powering everything from small apps to large-scale platforms like Facebook, YouTube (early years), and Twitter.

- **Type:** Relational (table-based, with rows and columns)
- **License:** Dual-licensed — GPL (free/open-source) and commercial (paid, from Oracle)
- **Language:** Written in C and C++
- **First released:** 1995, by MySQL AB (Sweden)

---

## 2. Core Architecture

MySQL has a **pluggable storage engine architecture** — the SQL layer (parser, optimizer, query cache) is separate from the storage layer, which handles how data is actually written to disk.

### Key Layers
1. **Connection Layer** — handles client connections, authentication, and thread management.
2. **SQL Layer** — parses queries, optimizes execution plans, manages caching.
3. **Storage Engine Layer** — the pluggable component that actually reads/writes data.

### Storage Engines
| Engine | Notes |
|---|---|
| **InnoDB** (default since MySQL 5.5) | ACID-compliant, supports transactions, foreign keys, row-level locking, crash recovery |
| **MyISAM** | Older, faster for read-heavy workloads, no transactions, table-level locking |
| **Memory (HEAP)** | Stores data in RAM — very fast, non-persistent |
| **Archive** | Compressed storage for historical/log data |
| **CSV** | Stores data as comma-separated files |

InnoDB is the practical default for almost all modern use cases because it supports transactions and foreign keys.

---

## 3. Data Types

| Category | Examples |
|---|---|
| **Numeric** | `INT`, `BIGINT`, `DECIMAL`, `FLOAT`, `DOUBLE`, `BOOLEAN` |
| **String** | `CHAR`, `VARCHAR`, `TEXT`, `BLOB`, `ENUM`, `SET` |
| **Date/Time** | `DATE`, `DATETIME`, `TIMESTAMP`, `TIME`, `YEAR` |
| **JSON** | Native `JSON` type (since MySQL 5.7) — allows semi-structured data inside relational tables |
| **Spatial** | `GEOMETRY`, `POINT`, `POLYGON` (for GIS use cases) |

---

## 4. SQL Fundamentals in MySQL

### DDL (Data Definition Language)
```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(150) UNIQUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE users ADD COLUMN age INT;
DROP TABLE users;
```

### DML (Data Manipulation Language)
```sql
INSERT INTO users (name, email) VALUES ('Amego', 'amego@example.com');
UPDATE users SET age = 30 WHERE id = 1;
DELETE FROM users WHERE id = 1;
```

### DQL (Data Query Language)
```sql
SELECT name, email FROM users WHERE age > 25 ORDER BY created_at DESC LIMIT 10;
```

### Joins
```sql
SELECT orders.id, users.name
FROM orders
JOIN users ON orders.user_id = users.id;
```
Types: `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `CROSS JOIN` (no native `FULL OUTER JOIN` — simulated via `UNION` of LEFT and RIGHT joins).

---

## 5. Indexing

Indexes speed up reads at the cost of slower writes and extra storage.

- **Primary Key** — unique identifier, automatically indexed (usually a B-Tree/clustered index in InnoDB)
- **Unique Index** — enforces uniqueness
- **Composite Index** — spans multiple columns (order matters — leftmost prefix rule)
- **Full-Text Index** — for text search (`MATCH() AGAINST()`)
- **Spatial Index** — for geometry types

```sql
CREATE INDEX idx_email ON users(email);
EXPLAIN SELECT * FROM users WHERE email = 'x@y.com'; -- check if index is used
```

InnoDB tables are **clustered** on the primary key — the actual row data is stored in primary key order, which affects performance for range queries and inserts.

---

## 6. Transactions & ACID

InnoDB supports full ACID compliance:

- **Atomicity** — all or nothing
- **Consistency** — data remains valid per constraints
- **Isolation** — concurrent transactions don't interfere (levels below)
- **Durability** — committed data survives crashes

```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- or ROLLBACK;
```

### Isolation Levels
1. `READ UNCOMMITTED`
2. `READ COMMITTED`
3. `REPEATABLE READ` (MySQL's default)
4. `SERIALIZABLE`

MySQL uses **MVCC** (Multi-Version Concurrency Control) in InnoDB to allow high concurrency without heavy locking.

---

## 7. Replication & Scaling

### Replication
- **Primary-Replica (Master-Slave)** — one write node, multiple read replicas; replication via binary log (binlog) shipping.
- **Group Replication / MySQL InnoDB Cluster** — multi-primary, synchronous replication for high availability.
- **Semi-synchronous replication** — a middle ground between async and full sync.

### Scaling Strategies
- **Vertical scaling** — bigger hardware
- **Read replicas** — offload read traffic
- **Sharding** — partition data across multiple servers (manual or via tools like Vitess)
- **Connection pooling** — via app-level pools or proxies like ProxySQL

---

## 8. Security

- User/privilege management via `CREATE USER`, `GRANT`, `REVOKE`
- SSL/TLS for encrypted connections
- Data-at-rest encryption (InnoDB tablespace encryption)
- Role-based access control (since MySQL 8.0, `CREATE ROLE`)

```sql
CREATE USER 'app_user'@'%' IDENTIFIED BY 'strong_password';
GRANT SELECT, INSERT, UPDATE ON mydb.* TO 'app_user'@'%';
```

---

## 9. Performance Tuning Basics

- Use `EXPLAIN` / `EXPLAIN ANALYZE` to inspect query plans
- Avoid `SELECT *` in production queries
- Index columns used in `WHERE`, `JOIN`, and `ORDER BY`
- Tune the **InnoDB buffer pool** (`innodb_buffer_pool_size`) — the most impactful single setting for read performance
- Use slow query log to catch problem queries
- Normalize for integrity, denormalize selectively for read-heavy performance
- Batch writes instead of row-by-row inserts where possible

---

## 10. Tooling & Ecosystem

- **MySQL Workbench** — official GUI for design, querying, admin
- **CLI** — `mysql` client
- **ORMs** — Sequelize, TypeORM, Prisma (Node.js); commonly paired with Express/NestJS apps
- **Migration tools** — Flyway, Liquibase, or ORM-native migrations
- **Managed hosting** — AWS RDS/Aurora, Google Cloud SQL, PlanetScale (built on Vitess), Azure Database for MySQL
- **Backup** — `mysqldump` (logical), `Percona XtraBackup` (physical/hot backups)

---

## 11. MySQL vs Other Databases (Quick Reference)

| | MySQL | PostgreSQL | MongoDB |
|---|---|---|---|
| Model | Relational | Relational (more standards-compliant SQL) | Document (NoSQL) |
| Best for | Web apps, read-heavy workloads | Complex queries, data integrity, extensions | Flexible/evolving schemas, horizontal scale |
| Transactions | Yes (InnoDB) | Yes, more robust | Yes (multi-document since v4.0) |
| JSON support | Native JSON type | Native JSONB (more powerful) | Native (BSON) |

---

## 12. Versions Worth Knowing

- **5.7** — introduced native JSON type, generated columns
- **8.0** (current major line) — window functions, CTEs (`WITH` clauses), roles, invisible indexes, improved optimizer, better default charset (`utf8mb4`)

---

## Summary

MySQL is a mature, battle-tested relational database best known for reliability at web-scale, a straightforward SQL dialect, and a strong ecosystem of managed hosting and tooling. InnoDB is the engine to default to for anything transactional. Its main trade-offs versus PostgreSQL are slightly less SQL-standard compliance and fewer advanced features (e.g., weaker native JSON querying, no true full outer joins), but it remains extremely fast and simple to operate for typical CRUD-heavy application workloads.
