# Storage Engine

- Is a component within a Database Management System (DBMS) that abstracts underlying media such as file system or memory.

## Data layout

The data layout in a DBMS could be one of the following...

### Row oriented

- Each record is laid out one after the other. For example a table with columns Name, Designation, EmailID, Salary will be laid out as follows...

```code
John, Programmer, john.doe@gmail.com, 50
Jane, Manager, jane.doe@gmail.com, 100
```

- Suitable when the query pattern is to **read entire row** for processing.

- Used by most relational databases.

### Column oriented

- The columns are stored together in separate files.

```code
- John, Jane
- Programmer, Manager
- 50, 100
```

- Very optimal for operations such as sum() , avg() etc, since all the participating columns are located together.

- Also lends itself well to compression.

### Column family 

- Store a **group** of columns together.

- A Column Oriented database stores NULL in the absence of value. However a column family database does not do it which is very handy when the data model is sparse.

### On Disk Layout

- For an RDBMS the on disk layout cannot be a binary search tree.

- In the best case of a perfectly balanced tree, finding an element would require log(N) seeks. Disk seek is slow because of the mechanical movements involved - Disk spinning and a head moving over the sector.
