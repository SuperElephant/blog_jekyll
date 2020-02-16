---
layout: post
title: "[Notes] CSCI 585 DB Data modeling"
tags: [csci585, eng, note]
excerpt_separator: ---
---

**Outline**
- Data modeling and importance
- Basic data-modeling building blocks
- Business rules and the influence towards database design
- Evolution of major data models
- Emerging alternative data models and the need they fulfill
- models classified by their level of abstraction

---

## Standard Database Concepts
- **Schema**: Conceptual organization of the entire database as viewed by the database administrator
- **Subschema**: Portion of the database seen by the application programs that produce the desired information from the data within the database
- **Data manipulation language (DML)**: Environment in which data can be managed and is used to work with the data in the database
- **Schema data definition language (DDL)**:Enables the database administrator to define the schema components 
- *SQL does both of DML and DDL*

## Data Modeling and Data Models
- **Data modeling**: Iterative and progressive process of creating a specific data model for a determined problem domain.
- **Data models**: Simple representations of complex real-world data structures. Useful for supporting a specific problem domain
- **Model**: Abstraction of a real-world object or event

### Importance of Data Models
- Are a communication tool 
- Give an overall view of the database
- Organize data for various users
- Are an abstraction for the creation of good database


## Data Model Basic Building Blocks 
- **Entity**: Unique and distinct object used to collect and store data
  - **Attribute**: Describes an association among entities
- **Relationship**: Describes an association among entities 
  - **One-to-many (1:M)**
  - **Many-to-Many (M:N or M:M)**
  - **One-to-one (1:1)**
- **Constraint**: Set of rules to ensure data integrity 


## Business Rules
- Brief, precise, and unambiguous description of a policy, procedure, or principle
- Enable defining the basic building blocks
- Describe main and distinguishing characteristics

### Sources of Business Rules
- Company managers 
- Policy makers
- Department managers
- Written documentation
- Direct interviews with end users

### Reasons for Identifying and Documenting Business Rules
- Help standardize company's view of data
- Communications tool between users and sesigners
- Allow designer to:
  - Understand the nature, role, scope of data, and business processes
  - Develop appropriate relationship participation rules and constraints
  - Create an accurate data model

### Translating Business Rules into Data Model Components
- Nouns translate into entities
- Verbs translate into relationship among entities
- Relationship are bidirectional
- Questions to identity the relationship type
  - How many instances of B are related to one instance of A?
  - How many instances of A are related to one instance of B?

#### Naming Conventions
- Entity names - Required to:
  - Be descriptive of the objects in the business environment 
  - Use terminology that is familiar to the **users**
- Attribute name - Required to be descriptive of the data represented by the attribute
- Proper naminh:
  - Facilitates communication between parties
  - Promotes self-documentation


## Evolution of major models

### Hierarchical modeling

At first, data was stored in individual files (transitioned from paper). 
The next improvement was a 'hierarchical DB model', where data was structured in the form of a tree [similar to a modern filesystem]. Data, in the form of nodes, are linked in a tree-like fashion. To traverse the tree, we need to know the underlying format ('class hierarchy, to make an analogy with classes and objects), and the actual path [eg. to relate A1 and D2, we need to traverse A1->B1->C3>D2]. (Notice, add or delete node may break the link, i.e., **structure dependent**)
![Hierarchical modeling](/assets/img/hierModel.jpg)
Hierarchies are good for '1:M' [tree], but not 'M:N' [graph or multiple inheritance].

- Manage large amounts of data for complex manufacturing projects
- Represented by a upside-down tree which contains segments
  - **Segments**: Equivalent of a file system's record type
- Depicts a set of one-to-many (1:M) relationships

#### Advantages
- Promotes data sharing
- Parent/child relationship promptes conceptual simplicity and data integrity
- Database security is provided and enforced by DBMS
- Efficient with 1:M relationship

#### Disadvantages
- Requires knowledge of physical data storage characteristics
- Navigational system requires knowledge of hierarchical path
- Changes in structure require changes in all application programs
- Implementation limitations 
- No data definition 
- Lack of standards


### Network modeling
A network model is better than a hierarchical one, because it can capture M:N [in addition to the above, another example is 'products and orders']. (structure dependence)
![network modeling](/assets/img/Netw.jpg)

- Represent complex data relationships 
- Improve database performance and impose a database standard
- Depicts both one-to-many (1:M) and Many-to-many (M:N) relationship

#### Advantage
- Conceptual simplicity
- Handles more relationship types 
- Data access is flexible 
- Data owner/member relationship promotes data integrity
- Conformance to standards
- Includes data definition language (DDL) and data manipulation language (DML)

#### Disadvantage
- System complexity limits efficiency 
- Navigational system yields complex implementation, application development, and management
- Structural changes require changes in all application programs

### The Relational Model
- Produced an automatic transmission database that replaced standard transmission databases 
- Based on a relation
  - **Relation** or **table**: Matrix composed of intersecting tuple and attribute
    - **Tuple** : Rows
    - **Attribute**: Columns
- Describes a precise set of data manipulation constructs

#### Advantages
- Structural independence is promoted using independent tables
- Tabular view improves conceptual simplicity
- Ad hoc query capability is based on SQL
- Isolates the end user from physical-level details 
- Improves implementation and management simplicity

#### ~~Disadvantages~~
- ~~Requires substantial hardware and system software overhead~~
- ~~Conceptual simplicity gives untrained people the tools to use a good system poorly~~ 
- ~~May promote information problems~~

#### Relational Database Management System(RDBMS) 
- Performs basic functions provided by the hierarchical and network DBMS systems
- Makes the relational data model easier to understand and implement
- Hides the complexities of the relational model from the user

#### SQL-Based Relational Database Application
- End-user interface
  - Allows end user to interact with the data
- Collection of tables stored in the database
  - Each table is independent from another
  - Rows in different tables are related based on common values in common attributes
- SQL engine
  - Executes all queries

### The Entity Relationship Model (E-R)
- Graphical representation of entities and their relationships in a database structure
- **Entity relationship diagram (ERD)**
  - Uses graphic representations to model database components
- **Entity instance or entity occurrence**
  - Rows in the relational table
- **Connectivity**: Term used to label the relationship types

#### Advantages
- Visual modeling yields conceptual simplicity
- Visual representation makes it an effective communication tool
- Is integrated with the dominant relational model
#### ~~Disadvantages~~
- ~~Limited constraint representation~~
- ~~Limited relationship representation~~
- ~~No data manipulation language~~
- ~~Loss of information content occurs when attributes are removed from entities to avoid crowded displays~~

![The ER Model Notations](/assets/img/24.jpg)

More notations: Additional reading: [here](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/DataModeling/more/notations.pdf) is information on, and comparison between, four ER notations: Chen, Crow, Rein85, IDEFIX.


### O-O databases
Also called 'object stores', these dbs offer(ed) a way to store ("persist") objects on disk. The objects (entity instances) are instanced from classes (entities), like with standard OO programming practice.

#### Advantages:
- 'cleaner' design - objects mimic real-world counterparts
- inheritance and encapsulation possible
- richer datatypes (attributes) available
- good for CAD, multimedia..

#### Drawbacks:
- harder to query (compared to relational DBs) - no straightforward way to build and traverse relations between objects
- relations are simpler in certain situations

The RDBMS community collectively ignored this development..


### O-R databases
These are a compromise between RDBs and OODBs - they feature an O-O front-end over a relational architecture. Interfacing applications do so in an O-O way, and queries/modifications are translated to/from relational form ("ORM").

#### Benefits:
- easy to access the data from an O-O application
- queries can be simpler (can use objects' structure)

#### Drawback:
- performance can be poor on account of the two-way translation


![Evolution of Data Models](/assets/img/36.jpg)
Data models have evolved - from 'hierarchical' (very rigid) to 'NoSQL' (VERY flexible).

## Models classified by their level of abstraction

### Layered data abstraction
![Layered data abstraction](/assets/img/37.jpg)

### The External Model 
An external model is a collection of 'fragmented', 'from the stakeholders' POV', modeling of a database.
- End users’ view of the data environment
- ER diagrams are used to represent the external views
- **External schema**: Specific representation of an external view

![External Model](/assets/img/39.jpg)

### The Conceptual Model 
- Represents a global view of the entire database by the entire organization
- **Conceptual schema**: Basis for the identification and high-level description of the main data objects
- Has a macro-level view of data environment
- Is software and hardware independent
- **Logical design**: Task of creating a conceptual data model

![Conceptual Model for Tiny College](/assets/img/41.jpg)

### The Internal Model
An internal model specifies what type of modeling (eg. relational, NoSQL...) to use for storing the data.
- Representing database as seen by the DBMS mapping conceptual model to the DBMS
- **Internal schema**: Specific representation of an internal model
- Uses the database constructs supported by the chosen database
- Is software dependent and hardware independent
- **Logical independence**: Changing internal model without affecting the conceptual model
![Internal Model for Tiny College](/assets/img/43.jpg)

### The Physical Model
The physical model specifies actual data storage specifics (file format, APIs...).
- Operates at lowest level of abstraction
- Describes the way data are saved on storage media such as disks or tapes
- Requires the definition of physical storage and data access methods
- Relational model aimed at logical level
- Does not require physical-level details
- Physical independence: Changes in physical model do not affect internal model