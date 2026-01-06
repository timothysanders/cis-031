# Chapter 2: Relational Databases

## 2.1: Relational model
### Database models
- A **database model** is a conceptual framework for database systems and contains three parts
  - *Data structures* prescribe how data is organized
  - *Operations* manipulate data structures
  - *Rules* govern valid data
- A **relational model** is a database model based on a tabular data structure, which was published by E.F. Codd in 1970 and released in commercial products around 1980
- Before computer systems became more powerful, network and hierarchical databases were more common. A relational database is much simpler to manage, and as performance of computers improved, they rapidly displaced hierarchical and network database
- Relational databases were initially designed for transactional data, but have gradually adopted support for big data and other alternative database models
- |                | Primary data structure | Initial product releases | Example database system | Strengths                                                 |
  |----------------|------------------------|--------------------------|-------------------------|-----------------------------------------------------------|
  | *Hierarchical* | Tree                   | 1960s                    | IMS                     | Fast queries<br>Efficient storage                         |
  | *Network*      | Linked list            | 1970s                    | IDMS                    | Fast queries<br>Efficient storage                         |
  | *Relational*   | Table                  | 1980s                    | Oracle Database         | Productivity and simplicity<br>Transactional applications |
  | *Object*       | Class                  | 1990s                    | ObjectStore             | Integration with object-oriented programming languages    |
  | *Graph*        | Vertex and edge        | 2000s                    | Neo4j                   | Flexible schema<br>Evolving business requirements         |
  | *Document*     | XML<br>JSON            | 2010s                    | MongoDB                 | Flexible schema<br>Unstructured and semi-structured data  |
### Relational data structure
- Relational data structure is based on set theory, where a **set** is an unordered collection of elements enclosed in braces
  - For example, sets `{a, b, c}` and `{c, b, a}` are the same, as sets are not ordered
- A **tuple** is an ordered collection of elements enclosed in parentheses
  - For example, tuples `(a, b, c)` and `(c, b, a)` are different, since tuples are ordered
- Relational data structure organizes data in tables
  - **Table** has a fixed name, a fixed tuple of columns, and a varying set of rows
  - **Column** has a name and a data type
  - **Row** is an unnamed tuple of values, where each value corresponds to a column and belongs to the column's data type
  - **Data type** is a named set of values, from which column values are drawn
- The rows in a table have no inherent order, since a table is a set of rows
- The terms "table", "column", "row", and "data type" are commonly used in database processing. "Relation", "attribute", "tuple", and "domain" are equivalent mathematical terms, often used in academic literature. "File", "field", "record", and "data type" are similar terms from file processing
- | Databases | Mathematics | Files     |
  |-----------|-------------|-----------|
  | Table     | Relation    | File      |
  | Column    | Attribute   | Field     |
  | Row       | Tuple       | Record    |
  | Data type | Domain      | Data type |
### Relational operations
- Relational operations are based on set theory, with each operation generating a result table from one or two input tables
  - *Select* selects a subset of (or all) rows of a table.
  - *Project* selects one or more columns of a table.
  - *Product* lists all combinations of rows of two tables.
  - *Join* combines two tables by comparing related columns.
  - *Union* selects all rows of two tables.
  - *Intersect* selects rows common to two tables.
  - *Difference* selects rows that appear in one table but not another.
  - *Rename* changes a table name.
  - *Aggregate* computes functions over multiple table rows, such as sum and count.
- These are **relational algebra** operations and are the theoretical foundation of the SQL language
- Relational operations always result in a table, so the result of an SQL query is also a table
### Relational rules
- Rules are logical constraints that ensure data is valid
- **Relational rules** are part of the relational model and govern data in every relational database
  - *Unique primary key*: all tables have a primary key column, or group of columns, in which values may not repeat
  - *Unique column names*: different columns of the same table have different names
  - *No duplicate rows*: No two rows of the same table have identical values in all columns
- **Business rules** are based on business policy and are specific to a particular database
- Relational rules are implemented as SQL **constraints**, whereas business rules are discovered during database design
  - Business rules may be enforced by SQL constraints, but some complex rules must be enforced by applications running on the database
