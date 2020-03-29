---
layout: post
title: "[Notes] CSCI 585 DB Relational Modeling"
tags: [csci585, eng, note]
excerpt_separator: ---
---

*Credit to: Prof. Saty Raghavachary, CSCI 585, Spring 2020*

**outline**
- That the relational database model offers a logical view of data
- About the relational model’s basic component: relations
- That relations are logical constructs composed of rows (tuples) and columns (attributes)
- That relations are implemented as tables in a relational DBMS
- About relational database operators, the data dictionary, and the system catalog
- How data redundancy is handled in the relational database model
- Why indexing is important

---

## A Logical View of Data
- Relational database model enables logical representation of the data and its relationships
- Logical simplicity yields simple and effective database design methodologies 
- Facilitated by the creation of data relationships based on a logical construct called a relation 

![Characteristics of a Relational Table](/assets/img/s3_4.png)

## Keys
- Consist of one or more attributes that determine other attributes
- Used to: 
  - Ensure that each row in a table is uniquely identifiable
  - Establish relationships among tables and to ensure the integrity of the data
- **Primary key (PK)**: Attribute or combination of attributes that uniquely identifies any given row

## Determination
- State in which knowing the value of one attribute makes it possible to determine the value of another
- Is the basis for establishing the role of a key 
- Based on the relationships among the attributes 

## Dependencies
'Full' functional dependence is a "good" thing.
- **Functional dependence**: Value of one or more attributes determines the value of one or more other attributes
  - **Determinant**: Attribute whose value determines another 
  - **Dependent**: Attribute whose value is determined by the other attribute
- **Full functional dependence**: Entire collection of attributes in the determinant is necessary for the relationship  

e.g.
- STU_ID[determinant] ->[functionally determines] STU_LNAME[dependent]
- STU_ID,STU_LNAME -> GPA is NOT a 'full functional dependency' because the determinant contains an extra (unwanted) attr (STU_LNAME)
- STU_LNAME,STU_FNAME -> GPA is a 'full functional dependency' (assuming lastname,firstname is unique)

## Types of Keys 
- **Composite key**: Key that is composed of more than one attribute
- **Key attribute**: Attribute that is a part of a key
- **Entity integrity**: Condition in which each row in the table has its own unique identity 
  - All of the values in the primary key must be unique
  - No key attribute in the primary key can contain a null 

- **Referential integrity**: Every reference to an entity instance by another entity instance is valid. Whereas entity integrity has to do with a single table, referential integrity relates to two tables (loosely, 'don't allow invalid pointers').

![Relational Database Keys](/assets/img/s9_4.png)

- **primary (foreign) keys** are a subset of candidate keys are a subset of superkeys (note - superkeys could be 'wasteful', ie. contain superfluous, non-needed attrs)
- simple keys vs compound keys vs composite keys
- **natural keys** - keys that are created from real-world entities (eg. for a US resident, their SSN could be a natural key)
- **surrogate keys** (just make up brand new unique keys)(opposite to the natural keys)
- secondary, or 'alternate' keys

You can read a bit more keys [here](http://www.agiledata.org/essays/keys.html).
## Null
- **Null**: Absence of any data value that could represent:
  - An unknown attribute value 
  - A known, but missing, attribute value  
  - A inapplicable condition 

### Ways to Handle Nulls
- **Flags**: Special codes used to indicate the absence of some value 
- NOT NULL constraint - Placed on a column to ensure that every row in the table has a value for that column
- UNIQUE constraint - Restriction placed on a column to ensure that no duplicate values exist for that column


## Relational Algebra
- Theoretical way of manipulating table contents using relational operators
- **Relvar**: Variable that holds a relation
  - Heading contains the names of the attributes and the body contains the relation
- Relational operators have the property of closure
  - **Closure**: Use of relational algebra operators on existing relations produces new relations (table(s) in, table out, ie. "closure")
  
What if a table were a datatype (similar to an int, Vec3D, ComplexNumber, etc)?! Specifically, what operations could be perform on them (eg. similar to addition, square root on doubles)?!

## Operations on tables
There are (only) EIGHT 'relational set operators' (defined by Ed Codd, at IBM, in 1970), which are all used to operate ("perform relational algebra") on tables: Select, Project, Union, Intersect, Difference, Product, Join, Divide.

This is no exaggeration: these operators are the basis for SQL and the entire relational DB industry!


### Relational Set Operators
- Select (Restrict)
  - Unary operator that yields a horizontal subset of a table
  ![Select](/assets/img/34_4.png)
- Project
  - Unary operator that yields a vertical subset of a table
  ![Project](/assets/img/35_4.png)
- Union
  - Combines all rows from two tables, excluding duplicate rows
  - Union-compatible: Tables share the same number of columns, and their corresponding columns share compatible domains
  ![Union](/assets/img/36_4.png)
- Intersect
  - Yields only the rows that appear in both tables
  - Tables must be union-compatible to yield valid results
  ![Intersect](/assets/img/37_4.png)

### Relational Set Operators
- Difference 
  - Yields all rows in one table that are not found in the other table
  - Tables must be union-compatible to yield valid results 
  ![Difference](/assets/img/38_4.png)
- Product 
  - Yields all possible pairs of rows from two tables
  - Relational Set Operators
  ![Product](/assets/img/39_4.png)
- Join
  - Allows information to be intelligently combined from two or more tables 
  ![Two Tables That Will Be Used in JOIN Illustrations](/assets/img/310_4.png)
- Divide
  - Uses one 2-column table as the dividend and one single-column table as the divisor
  - Output is a single column that contains all values from the second column of the dividend that are associated with every row in the divisor
  ![Divide](/assets/img/316_4.png)

#### Types of Joins 
- **Natural join**: Links tables by selecting only the rows with common values in their common attributes, A natural join links tables by selecting from two tables, only those rows that have common (identical) values for common attributes.
  - These three steps result in a natural join: create product, select, project.
  - **Join columns**: Common columns 
- **Equijoin**: Links tables on the basis of an equality condition that compares specified columns of each table
- **Theta join**: Extension of natural join, denoted by adding a theta subscript after the JOIN symbol 	
- **Inner join**: Only returns matched records from the tables that are being joined
- **Outer join**: Matched pairs are retained and unmatched values in the other table are left null 
  - **Left outer join**: Yields all of the rows in the first table, including those that do not have a matching value in the second table 
    - e.g., Output all rows of the left (CUSTOMER) table, including ones for which there are no matching values in the join column in the other (AGENT) table
  - **Right outer join**: Yields all of the rows in the second table, including those that do not have matching values in the first table 
    - e.g., Output all rows of the right (AGENT) table, including ones for which there are no matching values in the join column in the other (CUSTOMER) table

## Data Dictionary and the System Catalog 
- **Data dictionary**: Description of all tables in the database created by the user and designer 
  - A data dictionary is metadata about tables (only);
- **System catalog**: System data dictionary that describes all objects within the database
  - a system catalog, that includes (is a supeset of, although confusingly, the two are conflated in RL) the data dictionary, and more. 
- Homonyms and synonyms must be avoided to lessen confusion
  - **Homonym**: Same name is used to label different attributes 
  - **Synonym**: Different names are used to describe the same attribute asa