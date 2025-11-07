```markdown
# 🧠 MySQL Master: Curriculum for Instruction-Tuned SQL Generation

This repository defines a **multi-phase synthetic dataset plan** used to train a compact instruction-following model (e.g. **SmolLM2-360M**) into a capable **MySQL-aware query generator** and explainer.

The project aims to build a model that:
- Understands MySQL syntax and rules.
- Generates **one valid SQL query per instruction** (unless multiple are requested).
- Can **explain** or **fix** queries when asked.
- Operates entirely within realistic MySQL constraints and style.

---

## 🎯 Core Objective

> Build a *mini-MySQL expert* that knows DDL, DML, constraints, indexes, and relationships —  
> able to generate syntactically correct queries and explain them when prompted.

---

## 📚 Phase Overview

| Phase | Focus | Goal | Example Count |
|-------|--------|------|----------------|
| **0 — Vocabulary & Rules** | Teach keywords, clauses, types, and basic constraints. | Recognize valid MySQL tokens and grammar rules. | 1,000 |
| **1 — Syntax Primer** | Pure SQL samples. | Memorize structure and keyword placement. | 1,200 |
| **2 — Concept Primer** | Natural-language theory of SQL commands. | Understand what each statement does. | 800 |
| **3 — Syntax + Concept Fusion** | Mix explanations + SQL. | Fuse comprehension with generation. | 1,000 |
| **4 — Core DDL Tasks** | CREATE, ALTER, DROP, INDEX, FOREIGN KEY. | Master DDL generation. | 1,500 |
| **5 — DML & Query Logic** | SELECT, INSERT, UPDATE, DELETE, JOIN. | Master DML and basic query composition. | 1,200 |
| **6 — Constraints & Relationships** | FKs, cascades, unique, normalization. | Build relational understanding. | 700 |
| **7 — Multi-Step Prompts** | Multi-query generation. | Control multi-query behavior. | 500 |
| **8 — MySQL Master Review** | Reasoning, debugging, explanations. | Teach reflective SQL reasoning. | 600 |

**Total:** ~7,500 examples

---

## 🧩 Phase Summaries

### **Phase 0 — Vocabulary & Rules**
Teach the model the “language” of SQL:
- Keywords: `CREATE`, `ALTER`, `DROP`, `INDEX`, etc.
- Data types: `INT`, `VARCHAR(255)`, `DECIMAL`, `DATE`, etc.
- Syntax constraints:
  - Only one `AUTO_INCREMENT` per table.
  - `VARCHAR` length ≤ 255.
  - `PRIMARY KEY` must be unique.

**Example:**
```json
{ "instruction": "Write a rule about AUTO_INCREMENT in MySQL.", "output": "Only one column can be AUTO_INCREMENT, and it must be indexed or a primary key." }
```

---

### **Phase 1 — Syntax Primer**
Pure SQL statements for pattern memorization.

**Example:**
```json
{ "instruction": "Create a table named employees with id, name, and hire_date.", "output": "CREATE TABLE employees (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(200), hire_date DATE);" }
```

---

### **Phase 2 — Concept Primer**
Natural-language theory without SQL code.

**Example:**
```json
{ "instruction": "What does ALTER TABLE do?", "output": "It modifies the structure of an existing table by adding, deleting, or changing columns." }
```

---

### **Phase 3 — Syntax + Concept Fusion**
80% SQL, 20% concept — bridges language with structure.

**Example:**
```json
{ "instruction": "Explain and then create a users table with id and email.", "output": "The users table stores basic user data. CREATE TABLE users (id INT AUTO_INCREMENT PRIMARY KEY, email VARCHAR(200) UNIQUE);" }
```

---

### **Phase 4 — Core DDL Tasks**
Focus on structural SQL operations.

**Example:**
```json
{ "instruction": "Add a foreign key from orders.user_id to users.id.", "output": "ALTER TABLE orders ADD CONSTRAINT fk_orders_users FOREIGN KEY (user_id) REFERENCES users(id);" }
```

---

### **Phase 5 — DML & Query Logic**
CRUD operations and data manipulation.

**Example:**
```json
{ "instruction": "Insert three products into the products table.", "output": "INSERT INTO products (product_name, price) VALUES ('Laptop', 999.99), ('Tablet', 499.99), ('Monitor', 199.99);" }
```

---

### **Phase 6 — Constraints & Relationships**
Normalize relational patterns and cascades.

**Example:**
```json
{ "instruction": "Create a customers table and add a foreign key to orders.", "output": "CREATE TABLE customers (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(200)); ALTER TABLE orders ADD FOREIGN KEY (customer_id) REFERENCES customers(id) ON DELETE CASCADE;" }
```

---

### **Phase 7 — Multi-Step Prompts**
Teach the model when to output multiple queries.

**Example:**
```json
{ "instruction": "Create two tables: users and orders.", "output": "CREATE TABLE users (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(100)); CREATE TABLE orders (id INT AUTO_INCREMENT PRIMARY KEY, user_id INT, FOREIGN KEY (user_id) REFERENCES users(id));" }
```

---

### **Phase 8 — MySQL Master Review**
Reasoning + SQL + debugging + explanations.

**Example:**
```json
{ "instruction": "Explain what this query does: SELECT name FROM users WHERE id = 5;", "output": "It retrieves the name of the user whose ID is 5 from the users table." }
```

---

## 🧠 Model Behavior Objectives

| Behavior | Description |
|-----------|-------------|
| **Single-Query Discipline** | Default: generate one query per prompt. |
| **Multi-Query Awareness** | Output multiple queries only if explicitly asked. |
| **Syntax Validity** | Enforce MySQL correctness: one AUTO_INCREMENT, proper PRIMARY KEYs, VARCHAR ≤ 255, etc. |
| **Context Understanding** | Generate outputs based on SQL-related context only. |
| **Explain Mode** | Respond in natural language for "why", "explain", or "meaning" prompts. |

---

## ⚖️ Training Tips

- Use separate fine-tuning stages:
  - Merge early phases (0–3) for foundation.
  - Add DDL/DML layers (4–6) for structure.
  - Finish with reasoning (7–8).
- Maintain a balanced instruction-to-SQL ratio (~40:60 overall).
- Validate outputs using a MySQL parser before training.
- For compact models (e.g., **SmolLM2-360M**), prioritize **clean, diverse examples** over quantity.

---

## 📦 Output Behavior Goals

| Type | Example Prompt | Expected Output |
|------|----------------|-----------------|
| **Query Generation** | “Create a users table with id and name.” | One valid CREATE TABLE statement. |
| **Explanation** | “What does ALTER TABLE do?” | Natural-language explanation. |
| **Multi-Query** | “Create two tables for users and orders.” | Two valid CREATE TABLE statements. |
| **Debug** | “Fix this SQL: CREATE TABLE test (id AUTO_INCREMENT, name).” | Corrected version with proper syntax. |

---

## 🧩 License & Attribution

This dataset curriculum is open for **educational and research use**.  
Inspired by MySQL documentation and designed for lightweight instruction tuning.

**Author:** [Your Name or Org]  
**Model Target:** `HuggingFaceTB/SmolLM2-360M`  
**Version:** v1.0 — Structured Curriculum for MySQL Instruction Tuning
```
