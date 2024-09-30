# BigQuery 

1. Its an OLAP tool, serverless and no tiers
2. Separaton of computing and storage, distributed memory shuffle


## Bigquery Architecture

1. Stores data in column oriented format
2. Each big data table is stored as capacitor internal
3. Colossus is google distributed file system and each data center has its own colossus cluster
4. Jupiter acts as communication channel between colossus and big query at speed of 1 Pega byte at 5 bandwidth per second
5. Borg allocates resources to jobs, Dremel executes and manages query
6. Transitition of query -> Root -> Mixers -> Leaf Nodes -> Slots
7. GCP uses projects as top level structure, followed by dataset, table

## Bigquery Unstructured Data Structures

In Google BigQuery, **arrays** and **structs** are two important data types that allow more complex data structures, and they come with specific functions to manipulate and work with them. Let’s break them down:

---

### 1. **Arrays in BigQuery**

An **array** in BigQuery is a collection of values of the same data type. Arrays are useful for storing multiple related values in a single field, such as a list of numbers, strings, or even more complex types.

#### Key Characteristics of Arrays:
- Arrays are **1-dimensional**.
- All elements in an array must be of the same type (e.g., an array of integers or strings).
- Arrays can contain **null** values.

#### Syntax for Arrays:
```sql
ARRAY<element_type>
```

##### Example:
```sql
SELECT ARRAY[1, 2, 3] AS numbers_array;
```

#### Functions for Working with Arrays:

- **ARRAY_AGG()**: Aggregates values into an array.
  ```sql
  SELECT ARRAY_AGG(column_name) FROM table_name;
  ```

- **ARRAY_TO_STRING()**: Converts an array to a string with a delimiter.
  ```sql
  SELECT ARRAY_TO_STRING([1, 2, 3], ',') AS array_to_string;
  ```

- **ARRAY_LENGTH()**: Returns the number of elements in an array.
  ```sql
  SELECT ARRAY_LENGTH([1, 2, 3]) AS array_length;
  ```

- **OFFSET() / ORDINAL()**: Access elements by position.
  - **OFFSET** is zero-based, while **ORDINAL** is one-based.
  ```sql
  SELECT array[OFFSET(0)] AS first_element; -- 0-based
  SELECT array[ORDINAL(1)] AS first_element; -- 1-based
  ```

- **ARRAY_CONCAT()**: Concatenates multiple arrays.
  ```sql
  SELECT ARRAY_CONCAT([1, 2], [3, 4]) AS concatenated_array;
  ```

- **UNNEST()**: Expands an array into a set of rows (flattening the array).
  ```sql
  SELECT element FROM UNNEST([1, 2, 3]) AS element;
  ```

##### Example Use Case:
Storing multiple values in one column (e.g., user interests):
```sql
SELECT ARRAY['sports', 'music', 'movies'] AS interests;
```

---

### 2. **Structs in BigQuery**

A **struct** (or record) in BigQuery is a collection of **key-value pairs** (or fields), where each field has a name and a type. Structs are useful for grouping related values together, especially when dealing with nested or hierarchical data.

#### Key Characteristics of Structs:
- Structs can contain multiple **heterogeneous** fields (i.e., fields with different data types).
- Fields within a struct can be accessed using dot notation.
- Structs can contain other structs, allowing for complex nested data structures.

#### Syntax for Structs:
```sql
STRUCT<field_name data_type, field_name data_type>
```

##### Example:
```sql
SELECT STRUCT("John Doe" AS name, 30 AS age, "New York" AS city) AS person;
```

#### Functions for Working with Structs:

- **STRUCT()**: Constructs a struct from specified values.
  ```sql
  SELECT STRUCT('John Doe' AS name, 30 AS age);
  ```

- **Dot Notation**: Accessing elements inside a struct.
  ```sql
  SELECT person.name FROM (SELECT STRUCT('John Doe' AS name, 30 AS age) AS person);
  ```

- **Flattening Structs**: When using `SELECT *`, BigQuery flattens the struct fields into separate columns.
  ```sql
  SELECT person.* FROM (SELECT STRUCT('John Doe' AS name, 30 AS age) AS person);
  ```

##### Example Use Case:
Storing user information (name, age, and address):
```sql
SELECT STRUCT('John Doe' AS name, 30 AS age, STRUCT('123 Main St' AS street, 'New York' AS city) AS address);
```

---

### Combining Arrays and Structs

Arrays and structs can be combined to create more complex data structures, such as arrays of structs or structs containing arrays.

##### Example: Array of Structs
```sql
SELECT ARRAY[
  STRUCT("John Doe" AS name, 30 AS age),
  STRUCT("Jane Smith" AS name, 25 AS age)
] AS people;
```

##### Example: Struct Containing an Array
```sql
SELECT STRUCT("John Doe" AS name, ARRAY[1001, 1002, 1003] AS order_ids) AS customer;
```

#### Use Case with `UNNEST` and `STRUCT`:
You can flatten arrays of structs using `UNNEST`:
```sql
WITH dataset AS (
  SELECT ARRAY[
    STRUCT('John' AS name, 30 AS age),
    STRUCT('Jane' AS name, 25 AS age)
  ] AS people
)
SELECT person.name, person.age
FROM dataset, UNNEST(people) AS person;
```

---


