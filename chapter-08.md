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