---
layout: post
title: "[Notes] CSCI 585 DB Entity Relationship (ER) Modeling"
tags: [csci585, eng, note]
excerpt_separator: ---
---

*Credit to: Prof. Saty Raghavachary, CSCI 585, Spring 2020*

**Online**
- The main characteristics of entity relationship components
- How relationships between entities are defined, refined, and incorporated into the database design process
- How ERD components affect database design and implementation
- That real-world database design often requires the reconciliation of conflicting goals

---

## Entity Relationship Model (ERM)
- Basis of an entity relationship diagram (ERD) 
- ERD depicts the:
  - Conceptual database as viewed by end user
  - Database’s main components
    - Entities
    - Attributes
    - Relationships
- Entity - Refers to the entity set and not to a single entity occurrence

### Attributes
- Characteristics of entities
- **Required attribute**: Must have a value, cannot be left empty
- **Optional attribute**: Does not require a value, can be left empty
- Domain - Set of possible values for a given attribute
- **Identifiers**: One or more attributes that uniquely identify each entity instance.

An identifier is also called a KEY, or PRIMARY KEY - **this is one of the 'key' concepts in all of database theory!!** We'll talk much more about keys later.


![The Attributes of the Student Entity: Chen and Crow’s Foot]({{site.baseurl}}/assets/img/s4.png)

Notice: the bold attributes in crow's foot model means required attribute

- **Composite identifier**: Primary key composed of **more than one attribute**
- **Compound attribute**: Attribute that can be subdivided to yield additional attributes
- **Simple attribute**: Attribute that cannot be subdivided
- **Single-valued attribute**: Attribute that has only a single value
- **Multi-valued attributes**: Attributes that have many values   



![A Multivalued Attribute in an Entity]({{site.baseurl}}/assets/img/s6.png)


- **Multi-valued attributes**: Attributes that have many values and require creating:
  - Several new attributes, one for each component of the original multivalued attribute
  - A new entity composed of the original multivalued attribute’s components
- **Derived attribute**: Attribute whose value is calculated from other attributes
  - Derived using   an algorithm 


![Depiction of a Derived Attribute]({{site.baseurl}}/assets/img/s8.png)
![Advantages and Disadvantages of Storing Derived Attributes]({{site.baseurl}}/assets/img/s9.png)

## Relationships
- Association between entities that always operate in both directions
- **Participants**: Entities that participate in a relationship
- **Connectivity**: Describes the relationship classification (1:1), (1:M), (M:N)
- **Cardinality**: Expresses the minimum and maximum number of entity occurrences associated with one occurrence of related entity

![Connectivity and Cardinality in an ERD]({{site.baseurl}}/assets/img/s11.png)

Connectivity: 1:1, 1:M or M:N (three diff ways by which two entities are related).

Cardinality: (min,max) for 1:1, 1:M or M:N (eg. 1:1 can have (1,0) as its cardinality, 1:M can have (0,4) as its cardinality). Sometimes, min is called 'modality' (and max is cardinality). The 'inside' symbols denotes min, and the outside ones, max.

Confusingly, the # rows in a table is ALSO called table's cardinality (and, # of columns is called the table's degree).

Also confusingly, 1:1, 1:M, M:N are called 'cardinality ratios'!


### Existence Dependence (between tables)
- Existence dependence
  - Entity exists in the data base only when it is associated with another related entity occurrence
- Existence independence
  - Entity exists apart from all of its related entities
  - Referred to as a **strong entity** or **regular entity** or **identities entity**

Existence independence implies a strong entity; but, existence dependence (alone, ie. by itself) does NOT imply a weak entity (there needs to be one more condition, based on 'relationship strength', for it to become 'weak').

An entity B is "existent dependent" on another entity A, if, a row in B can only exist when its FK is NOT NULL, ie. a corresponding entry exists in A.

Eg. if A is EMPLOYEE and B is DEPENDENT, a dependent (eg. child) in B can only exist if there is a corresponding employee (eg. Dad) in A. THIS ALONE DOES NOT MAKE 'B' A WEAK ENTITY!

#### Relationship Strength
- Weak (non-identifying) relationship
  - Primary key of the related entity does not contain a primary key component of the parent entity

![A Weak (Non-Identifying) Relationship between COURSE and CLASS]({{site.baseurl}}/assets/img/48.png)
- Strong (identifying) relationships
  - Primary key of the related entity contains a primary key component of the parent entity

![A Strong (Identifying) Relationship between COURSE and CLASS]({{site.baseurl}}/assets/img/49.png)

#### Weak Entity
A weak entity needs to satisfy two conditions: 
1. existence dependence, 
2. strong (identifying/owning) relationship with a parent.

Note that a weak entity implies existence dependence, but existence dependence does not imply a weak entity!

Note weak entity $\rightarrow$ a strong ("owning" or "identifying") relationship.

Removing the controlling (owning) entity's key from a weak entity's PK will result in **duplicates** for remaining PK(s) - THAT is what makes it 'weak'.
- Conditions 
  - Existence-dependent 
  - Has a primary key that is partially or totally derived from parent entity in the relationship
- Database designer determines whether an entity is weak based on business rules

![A Weak Entity in an ERD]({{site.baseurl}}/assets/img/410.png)
![A Weak Entity in a Strong Relationship]({{site.baseurl}}/assets/img/411.png)

### Relationship Participation
- Optional participation
  - One entity occurrence does not require a corresponding entity occurrence in a particular relationship
- Mandatory participation
  - One entity occurrence requires a corresponding entity occurrence in a particular relationship


![Crow’s Foot Symbols]({{site.baseurl}}/assets/img/Picture1.png)
![CLASS is Optional to COURSE]({{site.baseurl}}/assets/img/413.png)
![COURSE and CLASS in a Mandatory Relationship]({{site.baseurl}}/assets/img/414.png)

### Relationship Degree
- Indicates the number of entities or participants associated with a relationship
- **Unary relationship**: Association is maintained within a single entity 
  - **Recursive relationship**: Relationship exists between occurrences of the same entity set
- **Binary relationship**: Two entities are associated
- **Ternary relationship**: Three entities are associated

![Three Types of Relationship Degree]({{site.baseurl}}/assets/img/415.png)
![An ER Representation of Recursive Relationships]({{site.baseurl}}/assets/img/417.png)
### Associative Entities
- Also known as composite or bridge entities
- Used to represent an M:N relationship between two or more entities
- Is in a 1:M relationship with the parent entities
  - <u>Composed of the primary key attributes of each parent entity</u>
- May also contain additional attributes that play no role in connective process


![Converting the M:N Relationship into Two 1:M Relationships]({{site.baseurl}}/assets/img/423.png)

![A Composite Entity in an ERD]({{site.baseurl}}/assets/img/425.png)

## Developing an ER Diagram
- Create a detailed narrative of the organization’s  description of operations
- Identify business rules based on the descriptions
- Identify main entities and relationships from the business rules
- Develop the initial ERD
- Identify the attributes and primary keys that adequately describe entities
- Revise and review ERD
  

<!-- Figure 4.26 - The First Tiny College ERD Segment
Figure 4.27 - The Second Tiny College ERD Segment 
Figure 4.28 - The Third Tiny College ERD Segment 
Figure 4.29 - The Fourth Tiny College ERD Segment 
Figure 4.30 - The Fifth Tiny College ERD Segment 
Figure 4.31 - The Sixth Tiny College ERD Segment 
Figure 4.32 - The Seventh Tiny College ERD Segment 
Figure 4.33 - The Eighth Tiny College ERD Segment 
Figure 4.34 - The Ninth Tiny College ERD Segment 
Table 4.4 - Components of the ERM
Database Design Challenges: Conflicting Goals
Figure 4.38 - Various Implementations of the 1:1 Recursive Relationship -->
