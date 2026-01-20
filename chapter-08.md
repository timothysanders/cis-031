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
