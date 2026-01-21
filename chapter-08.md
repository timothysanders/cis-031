# Chapter 8: Complex Data Types

## 8.1: Simple and complex types
### Simple types
- Relational database products support six broad categories of data types
  - *Integer* types represent positive and negative integers
  - *Decimal* types represent numbers with fractional values
  - *Character* types represent textual characters and may be fixed length or variable length strings, consisting of single-byte (ASCII) or double-byte (Unicode) characters
  - *Time* types represent date, time, or both. Some types include a time zone or specify a time interval
  - *Binary* types store data exactly as it appears in memory or computer files, bit for bit
  - *Semantic* types are based on other types, but have special meaning/functions. For example, `MONEY`, `BOOLEAN`, `UUID`, `ENUM`, etc.
- These types have relatively simple internal structures and are called **simple data types**
- Database functions can be used to decompose values of these types in different ways, for example `MONTH(BirthDate)` could return a month from a date
- |           | MySQL                                             | Oracle Database                                          | PostgreSQL                            | SQL Server                                        |
  |-----------|---------------------------------------------------|----------------------------------------------------------|---------------------------------------|---------------------------------------------------|
  | Integer   | BIT<br>TINYINT<br>SMALLINT<br>MEDIUMINT<br>BIGINT | INT<br>NUMBER                                            | BIT<br>SMALLINT<br>INTEGER<br>BIGINT  | BIT<br>TINYINT<br>SMALLINT<br>MEDIUMINT<br>BIGINT |
  | Decimal   | FLOAT<br>DOUBLE<br>DECIMAL                        | FLOAT<br>NUMBER                                          | REAL<br>NUMERIC<br>DECIMAL            | FLOAT<br>REAL<br>NUMERIC<br>DECIMAL               |
  | Character | CHAR<br>VARCHAR<br>TEXT                           | CHAR<br>VARCHAR2<br>LONG                                 | CHAR<br>VARCHAR<br>TEXT               | CHAR<br>VARCHAR<br>TEXT                           |
  | Time      | DATE<br>DATETIME<br>TIMESTAMP                     | DATE<br>TIMESTAMP<br>TIMESTAMP WITH TIMEZONE<br>INTERVAL | DATE<br>TIME<br>TIMESTAMP<br>INTERVAL | DATE<br>DATETIME<br>TIME<br>DATETIMEOFFSET        |
  | Binary    | TINYBLOB<br>MEDIUMBLOB<br>LONGBLOB                | BLOB<br>BFILE<br>RAW                                     | BYTEA                                 | BINARY<br>IMAGE                                   |
  | Semantic  | ENUM<br>BOOLEAN                                   | UROWID<br>BOOLEAN                                        | MONEY<br>BOOLEAN<br>UUID              | MONEY<br>UNIQUEIDENTIFIER                         |

### Complex types
- As relational database adoption increased, the need for additional types led to **complex data types**
- Most of these complex types fall into one of four categories
  - *Collection* types include several distinct values of the same base type, organized as a set or an array
  - *Document* types contain textual data in a structured formal such as XML or JSON
  - *Spatial* types store geometric information, such as lines, polygons, and map coordinates
  - *Object* types support object-oriented programming constructs, such as composite types, methods, and subtypes
- Research on complex types began in 1986 with PostgreSQL
- From the perspective of the database system, complex types, like simple types are atomic and stored as one value per cell
- |            | MySQL                                          | Oracle Database                                                              | PostgreSQL                                     | SQL Server            |
  |------------|------------------------------------------------|------------------------------------------------------------------------------|------------------------------------------------|-----------------------|
  | Collection | SET                                            | CREATE TYPE TypeName<br>AS VARRAY(n) OF <i>basetype</i>                      | <i>basetype</i>[n]<br><i>basetype</i> ARRAY[N] | <i>none</i>           |
  | Document   | JSON                                           | XMLTYPE<br>JSON                                                              | XML<br>JSON                                    | XML                   |
  | Spatial    | POINT<br>MULTIPOINT<br>POLYGON<br>MULTIPOLYGON | SDO_GEOMETRY<br>SDO_GEORASTER                                                | POINT<br>LINE<br>POLYGON<br>CIRCLE             | GEOMETRY<br>GEOGRAPHY |
  | Object     | <i>none</i>                                    | CREATE TYPE TypeName AS<br>OBJECT ...<br>CREATE TYPE TypeName AS<br>BODY ... | CREATE TYPE<br>TypeName AS ...                 | <i>none</i>           |

### User-defined types
- A **system-defined type**, also known as a **built-in type**, is provided by the database as a reserved keyword. Examples include `FLOAT`, `CHAR`, `ENUM`, etc.
- A **user-defined type** is created by the database designer or administrator with the `CREATE TYPE` statement. The **`CREATE TYPE`** statement specifies the type name and a **base type** that defines the implementation. This base type can be system defined or user defined. The new user defined type is implemented as the specified type, but they are different types and cannot be directly compared
- User-defined types can appear after column names in `CREATE TABLE` statements, just like system-defined types, and can be simple or complex
- `CREATE TYPE` is defined in the SQL standard and is widely supported (though not by MySQL)

## 8.2: Collection types
### Collection types
- A collection type is defined in terms of a base type and each collection type value contains zero, one, or many based type values
- Base type values are called **elements**
- Collections include four complex types
  - Elements of a **set** value cannot be repeated and are not ordered
  - Elements of a **multiset** value can be repeated and are not ordered
  - Elements of a **list** value can be repeated and are ordered
  - An **array** is an indexed list, with each element accessible with a numeric index
- The SQL standard includes multiset and array types but not set and list types. Most databases support some for of collection type, but implementations vary greatly

### Set type
- The MySQL `SET` type is similar to the `ENUM` type, where both have a base type consisting of character strings. Each `ENUM` value must contain exactly one element, while each `SET` value may contain zero, one, or many elements
- A `SET` type is defined with the `SET` keyword followed by the base type strings, `SET('apple', 'banana', 'orange')`. A `SET` value is specified as a string with commas between each element and no blank spaces.
- A `SET` value can contain no elements, and a value with no elements represents the empty set, which is not the same as a NULL value
- Internally, each `SET` value is a series of bits, each corresponding to a specific element. When a bit is one, the corresponding element is included, when the bit is zero, the corresponding element is not included. A base type can have at most 64 elements, so each `SET` value requires at most 64 bits, or eight bytes
- `SET` values can be compared with string functions
- ```mysql
  CREATE TABLE Employee (
      ID INTEGER,
      Name VARCHAR(20),
      Language SET('English', 'French', 'Spanish', 'Mandarin', 'Japanese'),
      PRIMARY KEY (ID)
  );
  INSERT INTO Employee (ID, Name, Language)
  VALUES (2538, 'Lisa Ellison', 'English,Spanish'),
         (6381, 'Maria Rodriguez', 'English,Spanish,Japanese'),
         (7920, 'Jiho Chen', 'Mandarin');
  ```

### Array type
- PostgreSQL specifies array types by appending pairs of brackets to any base type, with a number in the brackets indicating the array size (ex. `INTEGER[4]`). Optionally, you can use the `ARRAY` keyword (ex. `INTEGER ARRAY[4]`). If no number appears in the brackets, the array size is variable (up to the system maximum)
- An array value is specified as a string with comma-separated values within braces (ex. `{2, 5, 11, 6}`) and array elements are accessed via an index (ex. `WHERE MonthlyHours[2] > 100`)
- A multi-dimensional array type can be specified with multiple bracket pairs (ex. `INTEGER[4][9]`)
- In Oracle, an array is a user defined type

## 8.3: Document Types
### Document Types
- **Structured data** is stored as a fixed set of named data elements, organized in groups, and a type is explicitly declared for each element. Each group will have the same number of elements with the same names and types.
- **Semistructured data** is similar to structured data, but each group can have a different number of elements with different names, and the types are not explicitly declared. Elements are stored as characters and the type is inferred from the data
- **Unstructured data** is stored as elements embedded in a continuous string of characters or bits, and no element names or types are declared
- Semistructured data is stored in a **document** as text in a flexible format, such as XML or JSON. Most relational databases support XML or JSON document types. Each value of a document type is a complete XML or JSON document and may have many elements

### XML format
- XML stands for eXtensible Markup Language and uses tags instead of columns. A **tag** is a name enclosed in angle brackets, ex. `<tag name>`. An **XML element** consists of a start tag, data, and an end tag. The end tag is the start tag with a forward slash `/` before the name
  - Example: `<Department>Accounting</Department>` is an element with the tag name "Department" and data "Accounting"
- XML elements can be nested by embedding one set of tags in another and an XML document must have a **root** element that contains all other elements
- XML documents may have an optional first line, the **declaration**, which specifies document processing information like the XMl version of the character encoding
  - Example: `<?xml version = "2.0" encoding = "UTF-8"?>`
- XML tags are similar to relational columns but have a few advantages
  - *Readable*: tag names and embedded data are readily legible in XML and easy to understand
  - *Flexible*: tags can be easily added/dropped, which is important for semistructured data
  - *Hierarchical*: hierarchical data is easily represented by nesting elements within elements, while this would require multiple tables/foreign keys/referential integrity rules/etc. in a relational representation
- A primary disadvantage of XML is document size, tags are repeated for each value

### JSON format
- **JSON** stands for JavaScript Object Notation and is commonly pronounced 'JAY-sun'. JSON is similar to XML but more compact
- A **JSON element** consists of a name and associated data, written as `"name":data`
  - For example, `"Department":"Accounting"` is equivalent to the XML `<Department>Accounting</Department>`
- Unlike XML, JSON elements have a type that is one of the following six
  - *String*: series of characters enclosed in double quotes
  - *Number*: series of digits with an optional decimal point
  - *Boolean*: strings `true` or `false`
  - *Null*: the string `null`
  - *Array*: multiple data values enclosed in brackets
  - *Object*: multiple elements enclosed in braces
- Hierarchical data is represented in JSON by nesting elements, similar to XML
  - Example: `"Employee":{"Name":{"First":"Sam", "Last":"Snead"},"Salary":55000}`
- JSON is commonly used to transmit data over the internet, owing to its compact size
- The **name** of a JSON element is commonly called a key and the **data** is commonly called a value

### XML and JSON types
- Most relational databases support XML or JSON types, but the implementations can vary widely
- In MySQL, a JSON value is stored as an internal binary format rather than a string
- MySQL supports comparisons of JSON values with `<`, `>` and `=` operators but not `BETWEEN` or `IN`
- MySQL also has a number of functions that can be used to manipulate JSON
  - `JSON_ARRAY()`: formats string or numeric data as a JSON array
  - `JSON_DEPTH()`: determines the maximum number of levels in a JSON document hierarchy
  - `JSON_EXTRACT()`: returns data from a JSON document
  - `JSON_OBJECT()`: converts element names and data to a JSON document
  - `JSON_PRETTY()`: prints a JSON document in a format that is easy to read, with one element per line
