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

## 2.2: Structured Query Language
### Sublanguages
- **Structured Query Language** (**SQL**) is the query language of relational databases, the preferred pronunciation is 'S-Q-L'
- SQL statements are grouped into five sublanguages
  - **Data Definition Language**
  - **Data Query Language**
  - **Data Manipulation Language**
  - **Data Transaction Language**
  - **Data Control Language**
- | Sublanguage                                                           | Acronym | Example statements                                                                                                | MySQL documentation                                                                                                     |
  |-----------------------------------------------------------------------|---------|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
  | **Data Definition Language**<br>defines database structure.           | **DDL** | <pre><code>CREATE TABLE City (<br>  ID INTEGER,<br>  Name VARCHAR(15),<br>  Population INTEGER<br>);</code></pre> | [Data Definition Statements](https://dev.mysql.com/doc/refman/8.4/en/sql-data-definition-statements.html)               |
  | **Data Query Language**<br>retrieves data.                            | **DQL** | <pre><code>SELECT Name<br>FROM City<br>WHERE Population &gt; 15000;</code></pre>                                  | [Data Manipulation Statements](https://dev.mysql.com/doc/refman/8.4/en/sql-data-manipulation-statements.html)           |
  | **Data Manipulation Language**<br>inserts, updates, and deletes data. | **DML** | <pre><code>INSERT INTO City<br>VALUES (100, 'Geneva', 206000);</code></pre>                                       | [Data Manipulation Statements](https://dev.mysql.com/doc/refman/8.4/en/sql-data-manipulation-statements.html)           |
  | **Data Transaction Language**<br>manages transactions.                | **DTL** | <pre><code>START TRANSACTION;<br>COMMIT;</code></pre>                                                             | [Transactional and Locking Statements](https://dev.mysql.com/doc/refman/8.4/en/sql-transactional-statements.html)       |
  | **Data Control Language**<br>specifies user access to data.           | **DCL** | <pre><code>CREATE USER 'jordan';<br>GRANT ALL ON sakila.actor<br>TO 'jordan';</code></pre>                        | [Database Administration Statements](https://dev.mysql.com/doc/refman/8.4/en/sql-server-administration-statements.html) |

### Language elements
- | Language element | Description                                                                                                                                                  | Examples                                                                             |
  |------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
  | *Literal*        | Explicit value such as a character string or number.<br>String literals are enclosed in either single or double quotes.                                      | <pre><code>'Maria'<br>"Hello world!"<br>129<br>80.3</code></pre>                     |
  | *Keyword*        | Word with a special meaning for the language processor.<br>Keywords are defined by the database system.                                                      | <pre><code>SELECT<br>INSERT<br>INTEGER</code></pre>                                  |
  | *Identifier*     | Name of a database object, such as a column, table, or database.<br>Identifiers are specified by the programmer.                                             | <pre><code>City<br>FirstName<br>Population</code></pre>                              |
  | *Expression*     | Sequence of literals, identifiers, and operations that evaluate to a single value.                                                                           | <pre><code>Population &gt; 1000 OR Name = 'Tokyo'</code></pre>                       |
  | *Comment*        | Text that is ignored by the language processor.<br>Single-line comments begin with -- followed by a space.<br>Multi-line comments are enclosed in /* and */. | <pre><code>-- single line comment<br><br>/* multi-line<br>   comment */</code></pre> |
- A **statement** is a complete, executable instruction, ended with a semicolon. These statements have one or more clauses, with each **clause** beginning with a keyword, followed by other language elements
- Line breaks are ignored, but it is good to write each clause on a separate line
- String literals are always case-sensitive, keywords/identifiers are usually not case sensitive

### Syntax definitions
- The following are MySQL syntax definitions
  - `UPPERCASE` text indicates SQL keywords.
  - `lowercase` text indicates an identifier or expression provided by the user. In some cases, lower case text is a placeholder for a complex construction, defined elsewhere.
  - Square brackets `[]` enclose an optional language element.
  - Curly braces `{}` enclose a series of alternative language elements, separated by vertical bars. One, and only one, of the alternatives must be included in the statement.
  - Ellipsis `...` indicates that the preceding language element may be repeated.
  - Parentheses `()` and commas are literal symbols. Unlike brackets and braces, parentheses and commas appear in SQL statements exactly as written in the syntax definition. 

### The SQL standard
- The **SQL standard** specifies official syntax and behavior of SQL statements, which is published by American National Standards Institute (ANSI) and the International Organization for Standardization (ISO). This was originally published in 1986
- The latest version of the standard was published in 2023
- A version of the SQL standard can be found [here](https://www.iso.org/obp/ui/en/#iso:std:iso-iec:9075:-1:ed-6:v1:en)

## 2.3: Managing Databases
### `CREATE DATABASE` and `DROP DATABASE` statements
- A **database system instance** is a single executing copy of a database system
- Several SQL statements are used by database administrators/designers/users to manage the databases on an instance
  - `CREATE DATABASE DatabaseName` creates a new database
  - `DROP DATABASE DatabaseName` deletes a database, including all tables in the database
- ```sql
  CREATE DATABASE petStore;
  CREATE DATABASE bikeStore;
  DROP DATABASE petStore;
  ```

### `USE` and `SHOW` statements
- `USE DatabaseName` selects the default database for use in subsequent statements
- `SHOW` provides information about databases, tables, and columns
  - `SHOW DATABASES` lists all databases in the database system instance
  - `SHOW TABLES` lists all tables in the default database
  - `SHOW COLUMNS FROM TableName` lists all columns in the TableName of the default database
  - `SHOW CREATE TABLE TableName` shows the `CREATE TABLE` statement for the TableName table of the default database
  - Additional `SHOW` statements can be used to display other information, see details [here](https://dev.mysql.com/doc/refman/8.0/en/show.html)

## 2.4: Tables
### Tables, columns, and rows
- All data in relational databases is structured in tables
  - **Table** has a name, a fixed sequence of columns, and a varying set of rows
  - **Column** has a name and a data type
  - **Row** is an unnamed sequence of values, with each value corresponding to a column and belonging to the column's datatype
  - **Cell** is a single column of a single row
- A table must have at least one column, but can have any number of rows (including none)

### Rules governing tables
- Tables must obey relational rules, such as...
  1. *Exactly one value per cell*: a cell may not contain multiple values, unknown data is represented with `NULL`
  2. *No duplicate column names*: column names may not be duplicated in the same table
  3. *No duplicate rows*: no two rows may have identical values in all columns (though many database systems allow you to break this rule)
  4. *No row order*: rows are not ordered and the ordering of rows on the storage device never affects the query results
- Rule #4 is called **data independence**, which allows DBAs to organize the data on the storage device without affecting query results

### `CREATE TABLE` and `DROP TABLE` statements
- The `CREATE TABLE` statement creates a new table by specifying table name, column name(s), and column data type(s)
  - Sample data types may include
    - `INT` or `INTEGER` - integer values
    - `VARCHAR(N)` - values with 0 to N characters
    - `DATE` - date values
    - `DECIMAL(M, D)` - numeric values with M digits, of which D digits follow the decimal point
  - This statement will fail if the named table already exists
  - Can avoid these failures by combining with `IF NOT EXISTS`
- The `DROP TABLE` statement deletes a table, along with all the tables rows, from a database
  - This statement will fail if the named table does not exist
  - Can avoid these failures by combining with `IF EXISTS`
- ```sql
  CREATE TABLE [IF NOT EXISTS] TableName (
    Column1 DATA_TYPE,
    Column2 DATA_TYPE,
    ...
    ColumnN DATA_TYPE
  );
  
  DROP TABLE [IF EXISTS] TableName;
  ```

### `ALTER TABLE` statement
- The `ALTER TABLE` statement adds, deletes, or modifies columns in an existing table, the statement specifies the table name followed by a clause that indicates what should be altered
- | ALTER TABLE clause | Description       | Syntax                                                                                                  |
  |--------------------|-------------------|---------------------------------------------------------------------------------------------------------|
  | ADD                | Adds a column     | <pre><code>ALTER TABLE TableName<br>ADD ColumnName DataType;</code></pre>                               |
  | CHANGE             | Modifies a column | <pre><code>ALTER TABLE TableName<br>CHANGE CurrentColumnName NewColumnName<br>NewDataType;</code></pre> |
  | DROP               | Deletes a column  | <pre><code>ALTER TABLE TableName<br>DROP ColumnName;</code></pre>                                       |

## 2.5: Data types
### Data type categories
- A **data type** is a named set of values from which column values are drawn, with most data types falling into the following categories
  - **Integer**: represents positive and negative integers. Several integer data types exist, varying by number of bytes allocated for each value. Examples are INT (4 bytes) and SMALLINT (2 bytes)
  - **Decimal**: represent numbers with fractional values. Varies by number of digits after the decimal point and maximum size. Common types are FLOAT and DECIMAL
  - **Character**: represents textual characters. Common types are CHAR (fixed string of characters) and VARCHAR (string of variable length up to specified maximum length)
  - **Date and time**: represent date/time or both. Some types include a time zone or may represent a time interval. Common types include DATE, TIME, DATETIME, and TIMESTAMP
  - **Binary**: stores data exactly as it appears in memory/computer files, bit for bit. Common types include BLOB, BINARY, VARBINARY, and IMAGE
  - **Spatial**: stores geometric information, like lines, polygons, and coordinates. Examples include POLYGON, POINT, and GEOMETRY, these are relatively new and vary greatly across database systems
  - **Document**: contains textual data in a structured format such as XML or JSON
- Additional data types may be used for specialized use cases
- | Category      | Data type | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
  |---------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  | Integer       | INT       | -9281344                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
  | Decimal       | FLOAT     | 3.1415                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
  | Character     | VARCHAR   | Chicago                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
  | Date and time | DATETIME  | 12/25/2020 10:35:00                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
  | Binary        | BLOB      | 1001011101...                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
  | Spatial       | POINT     | (2.5, 33.44)                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
  | Document      | XML       | <pre><code>&lt;menu&gt;<br>  &lt;selection&gt;<br>    &lt;name&gt;Greek salad&lt;/name&gt;<br>    &lt;price&gt;\$13.90&lt;/price&gt;<br>    &lt;text&gt;Cucumbers, tomatoes, onions, and feta cheese&lt;/text&gt;<br>  &lt;/selection&gt;<br>  &lt;selection&gt;<br>    &lt;name&gt;Turkey sandwich&lt;/name&gt;<br>    &lt;price&gt;$9.00&lt;/price&gt;<br>    &lt;text&gt;Turkey, lettuce, tomato on choice of bread&lt;/text&gt;<br>  &lt;/selection&gt;<br>&lt;/menu&gt;</code></pre> |

### MySQL data types
- All relational databases support integer, decimal, date and time, and character data types
- Most allow integer and decimal numbers to be signed or unsigned, **signed** number may be negative, **unsigned** number cannot be negative
- Different data types have different storage requirements
  - Character types use one or two bytes per character
  - Integer types use a fixed number of bytes per number
  - Unsigned types can store larger numbers that the signed version of the same data type
- To minimize table size, the data type with the smallest storage requirements should be used
- | Category      | Example               | Data type      | Storage                                                                   | Notes                                                                                                 |
  |---------------|-----------------------|----------------|---------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
  | Integer       | 34 and -739448        | TINYINT        | 1 byte                                                                    | Signed range: -128 to 127<br>Unsigned range: 0 to 255                                                 |
  | Integer       | 34 and -739448        | SMALLINT       | 2 bytes                                                                   | Signed range: -32,768 to 32,767<br>Unsigned range: 0 to 65,535                                        |
  | Integer       | 34 and -739448        | MEDIUMINT      | 3 bytes                                                                   | Signed range: -8,388,608 to 8,388,607<br>Unsigned range: 0 to 16,777,215                              |
  | Integer       | 34 and -739448        | INTEGER or INT | 4 bytes                                                                   | Signed range: -2,147,483,648 to 2,147,483,647<br>Unsigned range: 0 to 4,294,967,295                   |
  | Integer       | 34 and -739448        | BIGINT         | 8 bytes                                                                   | Signed range: -2<sup>63</sup> to 2<sup>63</sup> - 1<br>Unsigned range: 0 to 2<sup>64</sup> - 1        |
  | Decimal       | 123.4 and -54.29685   | DECIMAL(M,D)   | Varies depending on M and D                                               | Exact decimal number where M = number of significant digits, D = number of digits after decimal point |
  | Decimal       | 123.4 and -54.29685   | FLOAT          | 4 bytes                                                                   | Approximate decimal numbers with range: -3.4E+38 to 3.4E+38                                           |
  | Decimal       | 123.4 and -54.29685   | DOUBLE         | 8 bytes                                                                   | Approximate decimal numbers with range: -1.8E+308 to 1.8E+308                                         |
  | Date and time | '1776-07-04 13:45:22' | DATE           | 3 bytes                                                                   | Format: YYYY-MM-DD. Range: '1000-01-01' to '9999-12-31'                                               |
  | Date and time | '1776-07-04 13:45:22' | TIME           | 3 bytes                                                                   | Format: hh:mm:ss                                                                                      |
  | Date and time | '1776-07-04 13:45:22' | DATETIME       | 5 bytes                                                                   | Format: YYYY-MM-DD hh:mm:ss. Range: '1000-01-01 00:00:00' to '9999-12-31 23:59:59'                    |
  | Character     | 'string'              | CHAR(N)        | N bytes                                                                   | Fixed-length string of length N; 0 &le; N &le; 255                                                    |
  | Character     | 'string'              | VARCHAR(N)     | Number of characters<br>+ 1 byte if N &le; 255<br>+ 2 bytes if N &gt; 255 | Variable-length string with maximum N characters; 0 &le; N &le; 65,535                                |
  | Character     | 'string'              | TEXT           | Number of characters<br>+ 2 bytes                                         | Variable-length string with maximum 65,535 characters                                                 |

## 2.7: Null values
### NULL
- **NULL** is a special value that represents unknown or inapplicable data. This is not the same as zero for numeric numbers or empty strings for character data types

### NOT NULL constraint
- Columns may contain NULL values by default, but in some cases, a business rule may require that a column should never contain NULL (such as a name of an employee for a business)
- The **NOT NULL** constraint prevents a column from having a NULL value, any statement trying to insert a NULL value will be rejected
- Example syntax of the `NOT NULL` constraint
- ```sql
  CREATE TABLE Employee (
      ID        SMALLINT UNSIGNED,
      Name      VARCHAR(60) NOT NULL,
      BirthDate DATE,
      Salary    DECIMAL(7, 20)
  );
  ```

### NULL arithmetic and comparisons
- When doing arithmetic or comparing with one or more NULL values, the result is NULL. If a WHERE clause evaluates to NULL for a row, the row is not selected

### IS NULL operator
- Because the comparison operators return NULL when either operand is NULL, comparison operators cannot be used to select NULL values. For these scenarios, you need to use `IS NULL` and `IS NOT NULL` operators to retrieve those rows/values
- Example syntax of the `IS NULL` operator
- ```sql
  SELECT *
  FROM Country
  WHERE IndepYear IS NULL;
  ```
- Example syntax of the `IS NOT NULL` operator
- ```sql
  SELECT *
  FROM Country
  WHERE Population IS NOT NULL;
  ```

### NULL logic
- In traditional math, expressions are always TRUE or FALSE, though with NULL values involved, you may have TRUE, FALSE, and NULL
  - TRUE AND TRUE is TRUE
  - TRUE AND FALSE is FALSE
  - TRUE AND NULL is NULL
- | x     | y     | x AND y | x OR y |
  |-------|-------|---------|--------|
  | TRUE  | NULL  | NULL    | TRUE   |
  | NULL  | TRUE  | NULL    | TRUE   |
  | FALSE | NULL  | FALSE   | NULL   |
  | NULL  | FALSE | FALSE   | NULL   |
  | NULL  | NULL  | NULL    | NULL   |
- MySQL does not have a specific data type for logical values, but rather represents FALSE as 0 and TRUE as 1 in query results
- Be careful about generalizing the way one database system treats NULL values to others, they are not all the same

