---
layout: post
title: "[Notes] CSCI 585 Database Performance Tuning and Query Optimization
"
tags: [csci585, eng, note]
excerpt_separator: ---
---

*Credit to: Prof. Saty Raghavachary, CSCI 585, Spring 2020*

*Chapter 11*

**Outline**
- Basic database performance-tuning concepts
- How a DBMS processes SQL queries
- About the importance of indexes in query processing
- About the types of decisions the query optimizer has to make
- Some common practices used to write efficient SQL code
- How to formulate queries and tune the DBMS for optimal performance


## Database Performance-Tuning 

### Concepts
- Goal of database performance is to execute queries as fast as possible
- **Database performance tuning**: Set of activities and procedures that reduce response time of database system
- Fine-tuning the performance of a system requires that all factors must operate at optimum level with minimal bottlenecks

### Client and Server
- Client side
  - **SQL performance tuning**: Generates SQL query that returns correct answer in least amount of time
    - Using minimum amount of resources at server
- Server side
  - **DBMS performance tuning**: DBMS environment configured to respond to clients’ requests as fast as possible
    - Optimum use of existing resources



### DBMS Architecture
- All data in a database are stored in **data file**s
  - Data files automatically expand in predefined increments known as **extends**
- Data files are grouped in file groups or table spaces
  - **Table space** or **file group**: Logical grouping of several data files that store data with similar characteristics
- **Data cache** or **buffer cache**: Shared, reserved memory area
  - Stores most recently accessed data blocks in RAM
- **SQL cache** or **procedure cache**: Stores most recently executed SQL statements or PL/SQL procedures, Thus SQL commends don't needs to be recompile again.
- DBMS retrieves data from permanent storage and places them in RAM 
- **Input/output request**: Low-level data access operation that reads or writes data to and from computer devices
- Data cache is faster than working with data files
- Majority of performance-tuning activities focus on minimizing I/O operations

![Figure 11.1 - Basic DBMS Architecture](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/PerfTuning/pics/s006.png)

- *Listener*: The listener processed listens for clients' requests and handles the processing of the SQL requests to other DBMS process. Once a request is received, the listener passes the request to the appropriate user process.
- *User*: The DBMS creates a user process to manage each client session. Therefore, when you log on to the DBMS, you are assigned a user process. This process handles all requests you submit to the server. There are many user processes-at least on per logged-in client.
- *Scheduler*: The scheduler process organizes the concurrent execution of SQL requests.
- *Lock manager*: This process manages all locks placed on database objects, including disk pages
- *Optimizer*: The optimizer process analyzes SQL queries and finds the most efficient way to access the data.


### Database Query Optimization Modes
- Algorithms proposed for query optimization are based on:
  - Selection of the optimum order to achieve the fastest execution time
  - Selection of sites to be accessed to minimize communication costs
- Evaluated on the basis of:
  - Operation mode
  - Timing of its optimization
  - Type of information (used for optimization)


#### Classification of Operation Modes
- **Automatic query optimization:** DBMS finds the most cost-effective access path without user intervention
- **Manual query optimization:** Requires that the optimization be selected and scheduled by the end user or programmer


#### Classification Based on Timing of Optimization 
- **Static query optimization:** best optimization strategy is selected when the query is compiled by the DBMS
  - Takes place at compilation time
  - Best for embedded queries
- **Dynamic query optimization:** Access strategy is dynamically determined by the DBMS at run time, using the most up-to-date information about the database
  - Takes place at execution time; more processing o'head


#### Classification Based on Type of Information Used to Optimize the Query

- **Statistically based query optimization algorithm:** Statistics are used by the DBMS to determine the best access strategy
- Statistical information is generated by DBMS through:
  - **Dynamic statistical generation mode - auto eval**
  - **Manual statistical generation mode - via user**
- **Rule-based query optimization algorithm:** based on a set of user-defined rules to determine the best query access strategy

![Table 11.2 - Sample Database Statistics Measurements](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/PerfTuning/pics/s015.png)

### Query Processing
- Parsing
  - DBMS parses the SQL query and chooses the most efficient access/execution plan (including optimizations)
- Execution
  - DBMS executes the SQL query using the chosen execution plan (including fetch data form back end and calculate)
- Fetching
  - DBMS fetches the data and sends the result set back to the client

![Query Processing](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/PerfTuning/pics/s017.png)

#### SQL Parsing Phase
- Query is broken down into smaller units 
- Original SQL query is transformed into slightly different version of the original SQL code which is fully equivalent and more efficient
- **Query optimizer:** Analyzes SQL query and finds most efficient way to access data
- **Access plans:** DBMS-specific and translate client’s SQL query into a series of complex I/O operations
- If access plan already exists for query in SQL cache, DBMS reuses it
  - If not, optimizer evaluates various plans and chooses one to be placed in SQL cache for use

#### SQL Execution Phase
Note that 'execution' here refers to executing the access plan, ie. fetching/sending data from/to the backend database - it does not refer to executing your SQL query. Query execution occurs in the next step (the 'fetching' step).
- All I/O operations indicated in the access plan are executed
  - Locks are acquired
  - Data are retrieved and placed in data cache
  - Transaction management commands are processed

#### SQL Fetching Phase

- Rows of resulting query result set are returned to client
  - DBMS may use temporary table space to store temporary data
  - Database server coordinates the movement of the result set rows from the server cache to the client cache

#### Query Processing Bottlenecks
- Delay introduced in the processing of an I/O operation that slows the system
- Caused by the:
  - CPU - slow processor, rogue processes.
  - RAM - shared among running processes
  - Hard disk - disk speed, transfer rates
  - Network - bandwidth shared among clients
  - Application code - bad user code, poor db design


### Indexes and Query Optimization
The reason for using an index is simple - provided we incur an upfront cost of creating (computing) one, runtime lookup costs using the index are vastly cheaper than doing full searches through non-indexed rows.

Eg. given this book, find all the places that discuss table updating..
- Indexes
  - Help speed up data access
  - Facilitate searching, sorting, using aggregate functions, and join operations
  - Ordered set of values that contain the index key and pointers
  - More efficient than a full table scan	


#### Indexes types
- **Data sparsity**: Number of different values a column could have (low sparsity => index might be useless)
- Data structures used to implement indexes:
  - **Hash indexes**
  - **B-tree indexes**
  - **Bitmap indexes**
- DBMSs determine best type of index to use
Example of low sparsity: STU_SEX gender column in a table is either M or F, so indexing this is useless (we'd only have 2 keys, each with 1000s of records!). On the other hand, DOB is a column with relatively higher sparsity, and is worth creating an index for..

A hash index is based on an ordered list of hash values, computed from a key column, using a hashing algorithm - it is much faster to search through the hash values than search the columns. Each hash value then points to the actual (column) data.

A user query (eg. LNAME="Johnson") is converted to a hash 'key' which is then used to search through the pre-computed list of hash values and retrieve an exact match or a small set of values that are all stored with the same key (as a result of 'hash collision').

![B-tree index, bitmap index](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/PerfTuning/pics/indices.png)

#### Index Selectivity 
Index selectivity: a measure of how likely an index will be used in a query. So we strive to create indexes that have high selectivity..

Indexes are useful when:

- an indexable column occurs in a WHERE or HAVING search expression
- an indexable column appears in a GROUP BY or ORDER BY clause
- MAX or MIN is applied to an indexable column
- there is high data sparsity on an indexable column


Worth creating indexes on single columns that appear in WHERE, HAVING, ORDER BY, GROUP BY and join conditions.

- Measure of the likelihood that an index will be used in query processing
- Indexes are used when a subset of rows from a large table is to be selected based on a given condition
- Index cannot always be used to improve performance
- Function-based index: Based on a specific SQL function of expression (eg. EMP_SALARY+EMP_COMMISSION)

### Optimizer Choices
- **Rule-based optimizer:** Uses preset rules and points to determine the best approach to execute a query
- **Cost-based optimizer:** Uses algorithms based on statistics about objects being accessed to determine the best approach to execute a query

### Using Hints to Affect Optimizer Choices
- Optimizer might not choose the best execution plan (try to create them again)
  - Makes decisions based on existing statistics, which might be old
  - Might choose less-efficient decisions
- **Optimizer hints:** Special instructions for the optimizer, embedded in the SQL command text

![Table 11.5 - Optimizer Hints](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/PerfTuning/pics/s029.png)

### SQL Performance Tuning
- Evaluated from client perspective
  - Most current relational DBMSs perform automatic query optimization at the server end
  - Most SQL performance optimization techniques are DBMS-specific and thus rarely portable
- Majority of performance problems are related to poorly written SQL code

### Conditional Expressions
- Expressed within WHERE or HAVING clauses of a SQL statement
  - Restricts the output of a query to only rows matching conditional criteria
- Guidelines to write efficient conditional expressions in SQL code
  - Use simple columns or literals as operands
  - Numeric field comparisons are faster than character, date, and NULL comparisons
- Equality comparisons are faster than inequality comparisons (do equality first)
- Transform conditional expressions to use literals
- Write equality conditions first when using multiple conditional expressions
- When using multiple AND conditions, write the condition most likely to be false first
- When using multiple OR conditions, put the condition most likely to be true first
- Avoid the use of NOT logical operator


### Query Formulation
All of the above can be summarized like so: "know what you want, then find a good way to get it" [what do we want to return/compute/generate, how to best go about it (what SQL constructs to use, on what data)].
- Identify what columns and computations are required
- Identify source tables
- Determine how to join tables
- Determine what selection criteria are needed
- Determine the order in which to display the output


## DBMS Performance Tuning
- Managing DBMS processes in primary memory and the structures in physical storage
- DBMS performance tuning at server end focuses on setting parameters used for:
  - Data cache
  - SQL cache
  - Sort cache
  - Optimizer mode
- **In-memory database:** Store large portions of the database in primary storage 
- Recommendations for physical storage of databases:
  - Use RAID (Redundant Array of Independent Disks) to provide a balance between performance improvement and fault tolerance
  - Minimize disk contention
  - Put high-usage tables in their own table spaces
  - Assign separate data files in separate storage volumes for indexes, system, and high-usage tables
More info..
If you'd like more detail (MySQL-related, but the overall ideas are non-DB-specific), read [this](http://bytes.usc.edu/cs585/s20_db0ds1ml2agi/lectures/PerfTuning/misc/MySQLPerfOptimSec3.pdf) doc..
<!-- DBMS Performance Tuning
Take advantage of the various table storage organizations in the database
Index-organized table or clustered index table: Stores the end-user data and the index data in consecutive locations in permanent storage
Partition tables based on usage
Use denormalized tables where appropriate
Store computed and aggregate attributes in tables -->
