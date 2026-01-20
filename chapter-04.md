# Chapter 4: Database Design

## 4.1: Entities, relationships, and attributes
### The entity-relationship model
- Database design starts with verbal/written requirements for a database, which are then formalized as an entity-relationship model, then implemented in SQL
- **Entity-relationship model** is a representation of data requirements, ignoring specific implementation details
- An entity-relationship model has three kinds of objects in it
  - **Entity**: a person, place, product, concept, or activity
  - **Relationship**: a statement about two entities (though these may be the same type of entity, this is a **reflexive relationship**)
  - **Attribute**: a descriptive property of an entity
- When the model is implemented in SQL, the entity typically becomes a table, while relationships and attributes typically become foreign keys and columns (though these are not hard rules)
- An **attribute** is a formal term for a column

### Entity-relationship diagram and glossary
- Entities, relationships, and attributes are usually depicted in an **entity-relationship diagram** (often called an **ER diagram** or an **ERD**)
- Entities are depicted as rectangles with rounded corners, relationships as lines connecting rectangles, and attributes within entity rectangles. An ER diagram always includes entities and relationships, it does not necessarily need to include attributes
- A **glossary** (also known as a **data dictionary** or **repository**) documents additional details in text format. This glossary includes names/synonyms/descriptions or entities/relationships/attributes
- The ER diagram and glossary are complementary and can be used to completely describe an entity relationship model

### Types and instances
- In an entity-relationship model, a type is a set
  - **Entity type** is a set of things (ex. all employees in a company)
  - **Relationship type** is a set of related things (ex. employee manages department)
  - **Attribute type** is a set of values (ex. employee salaries)
- Entity, relationship, and attribute *types* usually become tables, foreign keys, and columns
- An instance is an element of a set
  - **Entity instance** is an individual thing (ex. a particular employee)
  - **Relationship instance** is a statement about entity instances (ex. a particular employee manages the sales department)
  - **Attribute instance** is an individual value (ex. salary of $35,000)
- Entity, relationship, and attribute *instances* usually become rows, foreign key values, and column values
- |              | Type                    | Instance                 |
  |--------------|-------------------------|--------------------------|
  | Entity       | Passenger               | Muhammed Ali             |
  | Attribute    | BookingCode             | 39240                    |
  | Relationship | Passenger-Holds-Booking | Muhammed Ali holds 39240 |

### Database design
- Complex databases are developed in three phases
  1. **Conceptual design**: develops an entity-relationship model, capturing data requirements while ignoring implementation details
  2. **Logical design**: converts ER model into tables, columns, and keys for a particular database system
  3. **Physical design**: adds indexes, specifies how tables are organized on storage media
- Conceptual design is particularly important for complex databases when documenting requirements is challenging. For small databases, this step is less important
- Physical design is dependent on specific index and table structures, which can vary greatly across relational databases
- Conceptual design steps
  - | Step | Name                                             |
    |------|--------------------------------------------------|
    | 1    | Discover entities, relationships, and attributes |
    | 2    | Determine cardinality                            |
    | 3    | Distinguish strong and weak entities             |
    | 4    | Create supertype and subtype entities            |
- Logical design steps
  - | Step | Name                    |
    |------|-------------------------|
    | 5    | Implement entities      |
    | 6    | Implement relationships |
    | 7    | Implement attributes    |
    | 8    | Apply normal form       |

## 4.2: Discovery
### Discovery
- Database requirements are determined by interviewing database users and managers, who are usually familiar with requirements from an old database or a manual process.
- Entities, relationships, and attributes will often surface as nouns and verbs in an interview
  - Entities are usually nouns, but not all nouns are entities
  - Relationships are usually verbs; designers should ignore statements that are not about entities, not relevant to the database, or redundant. Look for relationships that are not explicitly stated
  - Attributes are usually nouns that denote specific data, like names, dates, quantities, and monetary values
- Written documents are also a good source of data requirements (for example, old user manuals)

### Names
- Entity names are usually a singular noun ("Employee" vs "Employees")
- Relationship names have the form `Entity-Verb-Entity` and the verb should be active, rather than passive ("Manages" vs "IsManagedBy"). The same verb can sometimes be used to relate different entity pairs
- Attribute names have the form `EntityQualifierType` (ex. EmployeeFirstName)
  - `Entity` is the name of the entity that the attribute describes. If the entity is obvious, `QualifierType` is sufficient and the entity name can be omitted
  - `Qualifier` describes the meaning of the attribute (ex. first, last, alternate), though sometimes the qualifier is unnecessary/omitted
  - `Type` is chosen from a list of standard attribute types such as Name, Number, and Count. Note that these are not identical to SQL data types
- Standard attribute types should be documented in the glossary and applied uniformly across all attribute names
- |              | Formal name              | Informal name |
  |--------------|--------------------------|---------------|
  | Entity       | Vehicle                  | Vehicle       |
  | Relationship | Vehicle-BelongsTo-Person | BelongsTo     |
  | Attribute    | VehicleLicenseNumber     | LicenseNumber |

### Synonyms and descriptions
- Entity, relationship, and attribute names will often have synonyms, which are commonly used in informal communications. One official name should be selected for each entity/relationship/attribute, with other names documented in the glossary as synonyms
- Glossary should also contain complete descriptions of entities/relationships/attributes, with the meaning of each stated in complete sentences. It should also contain examples and counter examples to illustrate usage

### Database design
- First step of conceptual design is discovery of entities, relationships, and attributes in interviews and document review. The designer creates an ER diagram, determines standard attribute types, and documents names/synonyms/descriptions in the glossary
- Database designers commonly move back and forth between steps and additional entities/relationships/attributes are discovered. The ER diagram/glossary are usually developed in parallel
- | Step | Activity                                                        |
  |------|-----------------------------------------------------------------|
  | 1A   | Identify entities, relationships, and attributes in interviews. |
  | 1B   | Draw ER diagram.                                                |
  | 1C   | List standard attribute types in glossary.                      |
  | 1D   | Document names, synonyms, and descriptions in glossary.         |

## 4.3: Cardinality
### Relationship maximum
- In ER modeling, **cardinality** refers to maxima and minima of relationships and attributes
- **Relationship maximum** is the greatest number of instances that one entity can relate to a single instance of another entity, there is a maxima for each "direction" of the related entities
  - Maxima are usually specified as one or many
  - Related entity is **singular** when the maximum is one and **plural** when the maximum is many
- On an ER diagram, maximum of one is shown as a short bar across the relationship line, while a maximum of many is shown as three converging short lines (**crow's foot** notation)
- Occasionally a plural entity has a fixed numeric maximum, these should be documented in the glossary

### Relationship minimum
- **Relationship minimum** is the least number of instances of one entity that can relate to a single instance of another entity, each entity in the relationship has a minima.
  - Minima are usually specified as zero or one
  - A related entity is **optional** when the minimum is zero and **required** when the minimum is one
- On an ER diagram, minimum of one is shown as a short bar across the relationship line, while a minimum of zero is shown as a circle
- Maxima symbols always appear next to the entity and the minima symbols appear farther from the entity
- An entity may occasionally have a minima of greater than one, this should be documented in the glossary

### Attribute maximum and minimum
- **Attribute maximum** is the greatest number of attribute values that can describe each entity instance. This can be specified as one (singular) or many (plural)
- **Attribute minimum** is the least number of attribute values that can describe each entity instance. This can be specified as zero (optional) or one (required)
- In an ER diagram, an attribute is presumed to be singular and optional, unless noted otherwise
  - "P" following the attribute indicates it is plural
  - "R" following the attribute indicates it is required
- Attributes may have fixed minimum/maximum numbers other than zero, one, or many, these should be documented in the glossary

### Unique attributes
- Each value of a **unique attribute** describes at most one entity instance (ex. VIN for a single Vehicle entity)
- Unique is not the same as singular
  - A unique attribute has at most one entity instance for each attribute value
  - A singular attribute has at most one attribute value for each entity instance
- In ER diagrams, attributes are not presumed to be unique, unless specified with a "U". Unique composite attributes are grouped with a brace before the "U" symbol

> ### Entity-Has-Attribute relationship
> Entities have an implicit relationship with their attributes. Attribute minimum/maximum are the cardinality of Attribute in the relationship
> An attribute is unique when Entity is singular in this relationship

### Database design
- Relationship and attribute cardinality depends on business rules, which the designer looks for in interviews and document reviews. These are then converted to specifications, which are documented in the ER diagram and glossary
- Cardinality may not always appear on an ER diagram, which are usually drawn with software tools that can show/hide cardinality
- | Step | Activity                                                         |
  |------|------------------------------------------------------------------|
  | 2A   | Determine relationship maxima and minima.                        |
  | 2B   | Determine attribute maxima and minima.                           |
  | 2C   | Identify unique attributes.                                      |
  | 2D   | Document cardinality in glossary and, optionally, on ER diagram. |

## 4.4: Strong and weak entities
### Strong entities
- An **identifying attribute** is unique, singular, and required. These correspond one-to-one, or **identify**, entity instances
- A **strong entity** has one or more identifying attributes. When a strong entity is implemented as a table, of the identifying attributes may become the primary key

### Weak entities
- A **weak entity** does not have an identifying attribute, but instead usually has a relationship (called an **identifying relationship**), with another entity, called an **identifying entity**. This identifying entity must be singular and required in an identifying relationship
- In an ER diagram, an identifying relationship has a diamond next to the identifying entity. Since an identifying entity is always singular and required, the diamond replaces the entity's cardinality symbols
- For weak entities, identifying relationships replace identifying attributes

### Identifying entities
- A weak entity is usually identified by a strong entity, but a weak entity can be identified by another weak entity or by several entities
- When a weak entity is identified by a weak entity or multiple entities, the identifying relationship may be complex. In these cases, the identifying attribute depends on business rules and may not be apparent in the ER diagram

### Database Design
- During the conceptual design phase, the designer distinguishes strong and weak entities. For each weak entity, the identifying relationship is noted and should be documented in the glossary and ER diagram
- Conceptual design steps are not always linear in practices, but are often iterative
- | Step | Activity                                                                         |
  |------|----------------------------------------------------------------------------------|
  | 3A   | Identify strong and weak entities.                                               |
  | 3B   | Determine the identifying relationship(s) for each weak entity.                  |
  | 3C   | Document weak entities and identifying relationships in glossary and ER diagram. |

## 4.5: Supertype and subtype entities
### Supertype and subtype entities
- Entity types are sets of entity instances. A **subtype entity** is a subset of another entity type, which is called the **supertype entity**
  - For example, Managers are a subset of employee, Manager is a subtype entity of the Employee supertype entity
- A supertype entity usually has several subtypes and attributes of the supertype apply to all subtypes (similar to inheritance in OOP), but attributes of a subtype do not apply to other subtypes or the supertype
- Subtypes can be used to highlight subsets of data with different attributes and relationships than the supertype, clarifying semantics/behavior
- Optional supertype attributes can become required subtype attributes
- An attribute repeated in several subtypes becomes a single supertype attribute

### Similar entities and optional attributes
- Supertype and subtype entities are often created from similar entities and optional attributes
- **Similar entities** are entities with many common attributes and relationships, and can become subtypes of a new supertype entity. Common attributes and relationships move to the new supertype entity. Non-shared attributes and relationships remain with the subtype entity
- If an entity has many optional attributes, that suggests a new supertype and subtype entities. The entity becomes a new supertype entity and retains all required attributes, while the optional attributes become required attributes of new subtype entities
- Creating a new supertype for similar entities is not automatic or objective decision, but must be judged based on business requirements

### Partitions
- A **partition** of a supertype entity is a group of mutually exclusive subtype entities. A supertype may have several partitions, and subtype entities within a partition are disjoint and do not share instances. Subtype entities in different partitions overlap and do share instances
- Each partition corresponds to an optional **partition attribute** of the supertype entity. The partition attribute indicates which subtype entity is associated with each supertype instance

### IsA relationship
- A supertype entity identifies its subtype entities, this identifying relationship is called an **IsA relationship**. Every subtype has an IsA relationship to its supertype, so the relationship is assumed and may be omitted from the ER diagram

### Database design
- After entities, relationships, attributes, cardinality, and strong/weak entities are determined, database designers look for supertype and subtype entities. Similar entities and optional attributes suggest new supertype/subtype entities. Mutually exclusive subtype entities are grouped into partitions, with each partition having a partition attribute added to the supertype entity
- | Step | Activity                                                                              |
  |------|---------------------------------------------------------------------------------------|
  | 4A   | Identify supertype and subtype entities.                                              |
  | 4B   | Replace similar entities and optional attributes with supertype and subtype entities. |
  | 4C   | Identify partitions and partition attributes.                                         |
  | 4D   | Document supertypes, subtypes, and partitions in glossary and ER diagram.             |
- This step is the last of four conceptual design steps, with logical design following
  1. Discover entities, relationships, and attributes
  2. Determine cardinality
  3. Distinguish strong and weak entities
  4. Create supertype and subtype entities

## 4.6: Alternative modeling conventions
### Diagram conventions
- ER diagram conventions may vary widely, for example they may
  - Depict relationship names in diamonds
  - Depict weak entities in double rectangles
  - Depict each attribute in a separate ellipse, connected to the entity with a line
  - Depict subtype entities with IsA relationships rather than inside of supertype entities
  - Use color, dashed lines, or double lines to convey additional information
- Variation in cardinality symbols are also common

### Model conventions
- ER modeling concepts also vary, where some ER models
  - Allow relationships between three or more entities
  - Decompose a complex model into a group of related entities, called a **subject area**
  - Refer to strong entities as **independent** and weak entities as **dependent**
- Some modeling conventions are standardized and widely used, with some examples
  - **Unified Modeling Language** (**UML**) is commonly used for software development, but also includes ER conventions
  - **IDEF1X** stands for Information DEFinition version 1X. IDEF1X became popular, in part, due to early adoption by the US DoD
  - **Chen notation** appeared in an early ER modeling paper by Peter Chen. It is not standardized, but often appears in literature and tools
- These differences in conventions are usually stylistic versus substantial and the choice of convention does not usually affect the resulting database design

## 4.7: Implementing entities
### Selecting primary keys
- In the first step of logical design, entities become tables and attributes become columns. As these are specified, primary keys are selected. The primary keys must be unique and required (not NULL)
- Primary keys should be
  - *Stable*: Primary key values should not change. When a primary key value changes, statements specifying the old value must also change and the new primary key must cascade to matching foreign keys
  - *Simple*: Primary keys should be easy to type and store, small values are generally easier to type and store in an SQL `WHERE` clause
  - *Meaningless*: Primary keys should not contain descriptive information. Descriptive information can change, so primary keys containing this information are unstable

### Implementing strong entities
- A strong entity becomes a **strong table**. The primary key must be unique and required, and should be stable, simple, and meaningless
- Simple primary keys are best for strong tables, but a composite primary key may be used if a simple primary key is not available. An **artificial key** is a simple primary key created by the database designer, usually an integer that is generated automatically by the database when new rows are inserted. These keys are stable, simple, and meaningless

### Implementing weak identities
- A weak entity becomes a **weak table**, which has a foreign key that references the identifying table and implements the identifying relationship
- The primary key depends on the cardinality of the identifying relationship
  - A weak entity is often plural and the primary key is the composite of the foreign key and another column
  - Occasionally, the weak entity is singular, so the primary key is the foreign key only
- The foreign key usually has the following referential integrity actions
  - Cascade on primary key update and delete
  - Restrict on foreign key insert and update
- In table diagrams, an arrow indicates a foreign key, starting at the foreign key and points to the table containing the referenced primary key
- If a weak entity has several identifying relationships, the primary key includes one foreign key for each identifying relationship. The primary key can include an additional column if needed for uniqueness

### Implementing supertype and subtype entities
- A supertype entity becomes a **supertype table**. A supertype entity with an identifying attribute is implemented like a strong entity, which a supertype entity that has an identifying relationship, rather than an identifying attribute, is implemented like a weak entity
- A subtype entity becomes a **subtype table**, where the primary key is identical to the supertype primary key and the primary key is also a foreign key that references the supertype primary key
- The foreign key usually has the following referential integrity actions
  - Cascade on primary key update and delete
  - Restrict on foreign key insert and update
- Foreign key implements the IsA relationship between the subtype and supertype entities

### Database design
- The implement entities step creates an initial table design and specifies primary keys. If no suitable primary keys are available, an artificial primary key is created. This design continues to be augmented in subsequent steps
- Some implementation decisions are affected by the database system. For example, if it is simple to create artificial primary keys, a designer may choose to use these more often
- | Step | Activity                                                      |
  |------|---------------------------------------------------------------|
  | 5A   | Implement strong entities as tables.                          |
  | 5B   | Create an artificial key when no suitable primary key exists. |
  | 5C   | Implement weak entities as tables.                            |
  | 5D   | Implement supertype and subtype entities as tables.           |

## 4.8: Implementing relationships
### Implementing many-one relationships
- The "implement entities" step converts identifying relationships into foreign keys, the "implement relationships" step converts all other relationships into foreign keys or tables
- A many-one or one-many relationship becomes a foreign key
  - The foreign key goes in the table on the "many" side and refers to the table on the "one" side
  - If the entity on the "one" side is required, the foreign key column is also required
- The foreign key name is the name of the referenced primary key, with an optional prefix that is usually derived from the relationship name and clarifies the meaning of the foreign key

### Implementing one-one relationships
- A one-one relationship becomes a foreign key and the foreign key can go in the table on either side of the relationship (but usually is placed in the table with fewer rows, to minimize NULL values)
  - Foreign key refers to the table on the opposite side of the relationship
  - The foreign key column is unique
  - If the entity on the opposite side of the relationship is required, the foreign key column is also required
- The foreign key name is the name of the referenced primary key, with an optional prefix that is usually derived from the relationship name and clarifies the meaning of the foreign key

### Implementing many-many relationships
- A many-many relationship becomes a new weak table
  - The new table contains two foreign keys, referring to the primary keys of the related tables
  - The primary key of the new table is the composite of the two foreign keys
  - The new table is identified by the related tables, so primary key cascade and foreign key restrict rules are usually specified
- Occasionally, an attribute describes a many-many relationship and becomes a column of the weak table
- The new table name usually consists of the related table names, with an optional qualifier (usually derived from the relationship name)
- | Step | Activities                                                                     |
  |------|--------------------------------------------------------------------------------|
  | 6A   | Implement many-one relationships as a foreign key on the 'many' side.          |
  | 6B   | Implement one-one relationships as a foreign key in the table with fewer rows. |
  | 6C   | Implement many-many relationships as new weak tables.                          |

## 4.9: Implementing attributes
### Implementing plural attributes
- In the "implementing entities" step, entities become tables and attributes become columns. Singular entities are in the initial table, but plural attributes need to be moved to a new weak table
  - New table contains the plural attribute and a foreign key referencing the initial table
  - Primary key of the new table is the composite of the plural attribute and the foreign key
  - New table is identified by the initial table, so primary key cascade and foreign key restrict rules are specified
  - New table name consists of the initial table name followed by the attribute name
- If the plural attribute has a small, fixed maximum, the plural attribute could be implemented as multiple columns in the initial table, but this is usually not the best solution

### Implementing attribute types
- During conceptual design, a list of standard attribute types is established. During logical design, an SQL data type is defined for each attribute type, which is then documented in the glossary/data dictionary
- Each attribute name includes the standard attribute type as a suffix, the attribute type determines the data type of the corresponding column

### Implementing attribute cardinality
- Since plural attributes are implemented as singular columns, all columns are singular, with required or unique attributes becoming required or unique columns. Like attributes, columns are presumed optional and not unique unless followed by R or U in the table diagram
- Relationship cardinality determines constraints on foreign key columns
  - If the table *referenced by* the foreign key implements a *required* entity, the column is required
  - If the table *containing* the foreign key implements a *singular* entity, the column is unique
- Table diagrams are implemented as `CREATE TABLE` statements
  - `NOT NULL` is specified for required columns
  - `UNIQUE` is specified for unique columns
  - `PRIMARY KEY` is specified for primary key columns
- Composite unique columns and composite primary keys can be specified in a column definition clause, they require an additional clause in the `CREATE TABLE` statement

### Database design
- The "implementing attributes" step specifies columns, column constraints, and data types. Plural attributes become new weak tables. Unique and required attributes are implemented with `UNIQUE`, `NOT NULL`, and `PRIMARY KEY` keywords
- After the "implementing attributes" step, the database is completely specified as `CREATE TABLE` statements.
- A final step, "review tables for third normal form" ensures the tables do not contain redundant data and fine-tunes the design as necessary
- | Step | Activities                                                                        |
  |------|-----------------------------------------------------------------------------------|
  | 7A   | Implement plural attributes as new weak tables.                                   |
  | 7B   | Specify cascade and restrict rules on new foreign keys in weak tables.            |
  | 7C   | Specify column data types corresponding to attribute types.                       |
  | 7D   | Enforce relationship and attribute cardinality with UNIQUE and NOT NULL keywords. |

## 4.10: First, second, and third normal form
### Functional dependence
- Column A **depends on** column B, meaning each B value is related to at most one A value. This relationship, "A depends on B" is denoted "B $\to$ A"
- Dependence of one column on another is called **functional dependence** and reflects business rules
- Functional dependence cannot be inferred from values in a table at a single point in time, as future updates may change current assumptions or patterns
- Functional dependence is not the only type of dependence, **multivalued dependence** and **join dependence** entail dependencies between three or more columns (but these are uncommon)

### Normal forms
- **Redundancy** is the repetition of related values in a table, this can cause database management problems. For example, if a value is updated, redundancies may mean it needs to be updated in many places, which makes queries slow and complex. Additionally, if all copies are not updated uniformly, they may become inconsistent
- **Normal forms** are rules for designing tables with less redundancy and are numbered first through fifth. An additional form, Boyce-Codd is an improved version of third normal form
- ![Normal form hierarchy](images/figure-4.10.1.png)
- Redundancy occurs when a dependence is on a column that is not unique. Boyce-Codd normal form eliminates all dependencies on non-unique columns and, in practice, is the most important normal form
- Fourth and fifth normal forms eliminate multi-valued and join dependencies, respectively. Since these multi-valued and join dependencies are complex and uncommon, these fourth/fifth normal forms are primarily theoretical

### First normal form
- Every cell of a table contains exactly one value. A table is in **first normal form** when, in addition to those values, the table has a primary key
  - *In a first normal form table, every non-key column depends on the primary key.* Each primary key value appears in exactly one row, and each non-key cell contains exactly one value. So each primary key value is related to exactly one non-key value.
  - *A first normal form table has no duplicate rows.* Every row contains a different primary key value and therefore every row is different.
- In practice, databases are allowed to violate this form with duplicate rows and no primary keys

> ### Alternative definitions of first normal form
> First normal form is commonly defined several ways
> - The table has a primary key
> - Every non-key column depends on the primary key
> - The table cannot have duplicate rows
> - Every cell contains exactly one value
> 
> The first three definitions are equivalent, but the last is different. The last definition is true of any relational table, and allows for duplicate rows and no primary key

### Second normal form
- A table is in **second normal form** when all non-key columns depend on the whole primary key, which means that a non-key column cannot depend on part of a composite primary key. A table with a simple primary key is automatically in second normal form
- A table in second normal form is also in first normal form, by definition

### Third normal form
- Redundancy can occur in a second normal form table when a non-key column depends on another non-key column. Informally, a table is in **third normal form** when all non-key columns depend on the entire key and nothing else
- A table in third normal form is also in first and second normal forms, by definition

