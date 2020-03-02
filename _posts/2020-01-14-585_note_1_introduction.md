---
layout: post
title: "[Notes] CSCI 585 DB Introduction"
tags: [csci585, eng, note]
excerpt_separator: ---
---

*Credit to: Prof. Saty Raghavachary, CSCI 585, Spring 2020*

**Outline**
- Data V.S. Information 
- Database, types of databases, and why they are valuable assets for decision making 
- Importance of database design
- How modern data bases evoled from file systems
- Flows in file system data menagement 
- DB system components
- DBMS main functions

---

## Data V.S. Information 

### Data
- Row facets: Not yet been processed to reveal the meaning 
- Building blocks of infromation 
- Data management: Generation, storage, and retrieval of data

### Information
- Produced by processing data
- Reveal the meaning of data
- Enable Knowledge creation
- Should be accurate, relevant, and timely to enable good decision making


## Database
- Shared, integrated computer structure that stores a collection of
  - End-user data - Raw facts of interest to end user
  - **Metadata:** Data about data, which the end-user data are integrated and managed (describe data characteristics and relationships)

### Database management system (DBMS)
  - Collection of programs
  - Manages the database structure
  - Controls access to data stored in the database
  - Intermediary between the user and the database
  - Enables data to be shared
  - Presents the end user with an integrated view of the data
  - Receives and translate application requests in to operations required to fulfill the requests
  - Hide database's internal complexity from the application progeams and users

![DBMS](/assets/img/Screen&#32;Shot&#32;2020-02-02&#32;at&#32;22.25.49.png)

#### Advantage
- Better data integration and less data inconsistency
  - **Data inconsistency:** Different versions of the same data appear in different places
- Increased end-user productivity 
- Imporoved:
  - Data sharing
  - Data security 
  - Data access
  - Decision making 
    - **Data quality:** Promoting accuracy, validity, and timeliness of data


### Types of Database

#### Based on user count
- **Single-uer database:** Supports one user at a time
  - **Desktop database:** Runs on PC
- **Multiuser database:** Supports multiple users at the same time
  - **Workgroup databases:** Supports a small number of users or a specific department 
  - **Enterprise data bases:** Supports many user across many departments

#### Based on location
- **Centralized database:** Data is located at a single site
- **Distributed database:** Data is distributed across different sites
- **Cloud database:** Created and maintained using cloud data services that provide defined performance measures for the database (it is distributed but with one more part involve, i.e. some companies run massive data center)

#### Based on content
- **General-purpose databases:** Contains a wide variety of data used in multiple disciplines
- **Discipline-specific databases:** Contains data focused on specific subject areas

#### Based on data currency (write/read)
- **Operational database:** Designed to supprot a company's day-tp day operations
- **Analytical database:** Store historical data and business metrics used exclusively for tactical of strategic decision making
  - **Data warehouse**: Store data in a formate optimized for decision support
- **online analytical processing (OLAP):** Enable retrieving, processing, and modeling data from the data warehouse
- **Business intelligence:** Capture and processes business data to generate information that support decision making

#### Based ib tge structure on of contained data
- **Unstructured data:** It exists in their original state
- **Structured data:** It results from formatting; Structure is applied based on type of processing to be performed
- **Semistructured data:** Processed to some extent
- **Extensible Markup Language (XML):** Represents dat elements in textual format
- **JSON:** Much better then XML

## Evolution of File System Data Processing
- **Manual File Systems:** Accomplished through a system of file folders and filing cabinets
- **Computerized File Systems:** Data processing specialist: Created a computer-based system that would track data and produce required reports
- **File System Redux:** End-User Productivity Tools: Includes spreadsheet programs such as MS Excel

### Basic File Terminology
- **Data**: Raw facts, such as phon number, birth dates... Data have little meaning unless they have been organized in some logical manner.
- **Field**: A Character of group of characters that has a specific meaning. A field is used to define and store data.
- **Record**: A logically connected set of one or more fields that describes a person, place, of thing. e.g., the fields that constitute a record for a customer might consist of the name, address, phone etc.
- **File**: A collection of related records.

### Problems with File System Data Processing
- Lengthy development times
- Difficulty of getting quick answers
- Complex system administration
- Lack of security and limited data sharing
- Extensive programming
- **Structural dependence**: Access to a file is dependent on its own structure. All file system programs are modified to conform to a new file structure
- **Structural independence**: File structure is changed with out affecting the application's ability to access the data
- Data dependence: data access changes when data storage characteristics change
- Data independence: Data storage characteristics is changed without affecting the program's ability to access the data
- Practical significance of data dependence is difference between logical and physical format

## Data Redundancy
- Unnecessarily storing same data at different places
- **Island of information**: Scattered data locations. Increases the probability of having different versions of the same data

### Implications
- Poor data security 
- Risk of data inconsistency 
- Increased likelihood of data-entry errors when complex entries are made in different files
- **Data anomaly:** Develops when not all of the required changes in the redundant data are made successfully 
  - Update Anomalies
  - Inserting Anomalies
  - Deletion Anomalies


## Database Systems
- Logically related data sore in a single logical  data repository 
  - Physically distributed among multiple storage facilities
  - DBMS eliminates most of file system's problems 
- Current generation DBMS software:
  - Stores data structures, relationships between structures, and access paths
  - Defines, stores, and manages all access path and components

### DBMS Functions
- Data dictionary management
  - Data dictionary: Stores definitions of the data elements and their relationships
- Data storage management
  - Performance tuning: Ensures efficient performance of the database in terms of storage and access speed 
- Data transformation and presentation
  - Transforms entered data to conform to required data structures
- Security management 
- Enforces user security and data privacy 
- Multiuser access control
  - Sophisticated algorithms ensure that multiple users can access the database concurrently without compromiseing its integrity
- Backup and recovery management
  - Enables recovery of the database after a failure
- Data integrity management 
  - Minimize redundancy and maximizes consistency 
- Database access languages and application programming interfaces
  - Query language: Let the user specify what must be done without having to specify how
  - Structure Query Language (SQL) De facto query language and data access standard supported by the majority of DBMS vendors
- Database communication interfaces
  - Accept end-user requests via multiple, different network environment 

### Disadvantage of DS
- Increased costs
- Management complexity 
- Maintaining currency
- Vendor dependence 
- Frequent upgrade/replacement cycles