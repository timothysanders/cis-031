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

## 8.4: Spatial types
### Spatial data
- **Spatial data** is a geometric object, like a point, line, polygon, or sphere, given as coordinates in an N-dimensional space. This spatial data is commonly written in a format called **Well-Known Text** (**WKT**), which specifies a shape name followed by vertex coordinates

### Geometric and geographic data
- Spatial data is either geometric or geographic
  - **Geometric data** is embedded in a flat plane or 'square' three-dimensional space, called a **Cartesian coordinate system**
  - **Geographic data** is defined with reference to the surface of the earth (such as latitude, longitude, and elevation above sea level)
- Since the surface of the earth is curved, distance/area computations are different for geometric and geographic data. The surface of the earth can be described with many alternative coordinate systems, which are called **spatial reference systems**. These are standardized and identified with **spatial reference system identifiers** (**SRID**)
- Many databases implement a spatial data standard from the Open Geospatial Consortium (OGC)
- Most databases support spatial data natively or through separate, optional components. MySQL has native support for spatial types and functions with certain storage engines

### Spatial types
- MySQL types are restricted to two-dimensional coordinate systems, with support for four basic spatial types
  - `POINT`: describes a specific location, such as an address
  - `LINESTRING`: consists of one or more line segments and represents objects like rivers or streets. Linestrings can be closed, like a rectangle, but is one-dimensional and does not have an area
  - `POLYGON`: describes two dimensional surfaces such as regions or postal codes. Polygons are closed and have an area. Polygons may have holes, represented by inner polygons within an outer polygon
  - `GEOMETRY`: values can be either `POINT`, `LINESTRING`, or `POLYGON`
- Each value of these types is a single geometric element. MySQL also supports four types that contain multiple geometric elements in each value: `MULTIPOINT`, `MULTILINESTRING`, `MULTIPOLYGON`, and `GEOMETRYCOLLECTION`
- MySQL stores spatial values in an internal format
  - Four bytes for the SRID
  - Four bytes for the spatial type
  - Eight bytes for each coordinate

### Spatial functions
- MySQL supports many functions that manipulate spatial data. These functions typically depend on a spatial reference system, so they utilize the SRID stored with each spatial value. Most of the functions have an `ST_` prefix ("spatial type")
  - `ST_Area()`: returns the area of a polygon or multipolygon
  - `ST_Distance()`: determines the distance between two spatial values
  - `ST_Overlaps()`: determines if two spatial values overlap
  - `ST_Union()`: merges two spatial values into one
  - `ST_X()` and `ST_Y()` return the X- and Y-coordinate of a point
- A **minimum bounding rectangle** or **MBR** is the smallest rectangle that contains a spatial value. Many spatial operations are optimized using minimum bounding rectangles
- MySQL supports functions to manipulate MBRs, such as
  - `ST_Envelope()`: returns the smallest rectangle that contains a spatial value
  - `MBRContains()`: determines if the MBR of one spatial value contains the MBR of another
  - `MBROverlaps()`: determines if the MBRs of two spatial values overlap
- MySQL operators do not work with spatial values, but the operators must use numeric values returned by spatial functions

### Spatial indexes
- Indexes on spatial columns have several special problems
  - *Index entries must be sorted on column values*: two- and three-dimensional spatial values do not have an obvious sort order
  - *Index entries must be small*: a linestring or polygon value may have hundreds of bytes, which can make the index very large and slow down searches
  - *Index entry comparisons must be fast*: spatial searches are relatively slow and require numerous arithmetic computations
- Most databases use a special structure for spatial indexes, called an **R-tree**. This is a B+tree in which index entries contain MBRs instead of column values. The R-tree has multiple index levels (like a B+tree)
  - *The bottom level* has one index entry for each spatial value in a column and each entry has the MBR for the value and a pointer to the block containing the value
  - *Higher levels* consist of index entries pointing to lower levels. Each entry has a pointer to one lower-level block, along with an MBR containing all MBRs in the block
- As spatial values are inserted/deleted, index entries are added and removed in the bottom level. Index blocks eventually fill or empty and, as with a B+tree, split or merge to maintain a balanced tree.
- If an insertion causes a block split, index entries in one block are divided across two blocks using the **quadratic split** algorithm, which attempts to minimize the size of MBRs in index entries pointing to the two new blocks
- In MySQL, certain storage engines will create R-tree indexes automatically for spatial columns

## 8.5: Object types
### Object-orientation
- Because relational models were developed prior to the rise in popularity of object-oriented programming languages like Java/C++, most relational databases did not initially support object-oriented capabilities
  - **Composite types**: combine several properties into one type
  - **Methods**: functions or procedures associated with a type
  - **Subtypes**: derived from an existing type, called a **supertype**. The subtype automatically inherits all supertype properties and methods, but may also have additional properties and methods
  - Subtype methods may **override**, or redefine behavior from the supertype
- A composite type is called a **class** in OOP, a subtype is called a **derived class**, and a supertype is called a **base class**

### Object-relational databases
- **Object database** adds database capabilities, like persistence and transaction management, to an object-oriented language. Many object databases exist, but they are not widely used
  - Relational databases were already mature when object databases were developed
  - Database management and application programming have different technical requirements, adding database capabilities to object-oriented languages is challenging/complex
- **Object-relational mapping** (**ORM**) is a software layer between the programming language and a relational database, which converts object structures and queries to relational structures and queries. ORM is more successful than object databases
- **Object-relational database** extends SQL with an object type, this was incorporated into the SQL standard in 1999. However, there are challenges and most relational databases do not support an object type
- Some relational databases support subtables instead of subtypes, where a **subtable** inherits columns and constraints from another table, called a **supertable**

### Object type
- Oracle Database is a leading object-relational database