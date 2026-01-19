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
