# Local SQLite Database Manager

## Overview

This project provides a Python class, `local_sql_db`, to simplify working with SQLite databases. It includes features for connecting to a database, creating tables, inserting data, querying data, and managing schema information. The class is designed to handle common SQLite operations with minimal configuration.

## Features

-   **Automatic database creation**: Creates a `.db` file if it doesn't already exist.
    
-   **Table management**:
    
    -   Create tables with dynamic schemas.
        
    -   Check if a table exists.
        
    -   Retrieve table schema.
        
-   **Data manipulation**:
    
    -   Insert single or multiple rows of data.
        
    -   Query data with parameterized SQL.
        
-   **Utility functions**:
    
    -   Close database connections.
        

## Class Methods

### Initialization

```
local_sql_db(db_path="local_database.db")
```

-   Initializes the database connection.
    
-   Automatically appends `.db` to the file name if not provided.
    

### `connect()`

Establishes a connection to the SQLite database.

### `create_table(table_name, schema)`

Creates a table with the specified schema.

-   **Arguments**:
    
    -   `table_name` (str): Name of the table.
        
    -   `schema` (dict): Dictionary of column names and data types.
        
-   **Returns**:
    
    -   `True` if the table is created successfully.
        
    -   `False` if the table already exists.
        

Example:

```
db_manager.create_table(
    "users",
    {
        "id": "INTEGER PRIMARY KEY AUTOINCREMENT",
        "name": "TEXT NOT NULL",
        "email": "TEXT UNIQUE NOT NULL",
        "created_at": "DATETIME DEFAULT CURRENT_TIMESTAMP"
    }
)
```

### `table_exists(table_name)`

Checks if a table exists in the database.

-   **Arguments**:
    
    -   `table_name` (str): Name of the table to check.
        
-   **Returns**: `True` if the table exists, `False` otherwise.
    

### `get_schema(table_name)`

Retrieves the schema of a table in the format expected by `create_table`.

-   **Arguments**:
    
    -   `table_name` (str): Name of the table.
        
-   **Returns**: Dictionary with column names as keys and data types as values.
    

### `insert_data(table_name, data)`

Inserts data into a table.

-   **Arguments**:
    
    -   `table_name` (str): Name of the table.
        
    -   `data` (list[dict]): List of dictionaries representing rows to insert.
        

Example:

```
db_manager.insert_data(
    "users",
    [
        {"name": "James", "email": "james@example.com"},
        {"name": "Sam", "email": "sam@example.com"}
    ]
)
```

### `query_data(query, params=None, return_dict_list=True)`

Executes a SELECT query and returns the results.

-   **Arguments**:
    
    -   `query` (str): The SQL query.
        
    -   `params` (tuple, optional): Parameters for the query.
        
    -   `return_dict_list` (bool): If `True`, returns results as a list of dictionaries. Otherwise, returns a list of tuples.
        
-   **Returns**: Query results.
    

Example:

```
results = db_manager.query_data("SELECT * FROM users WHERE name = ?", ("James",))
for row in results:
    print(row)
```

### `run_query(query, params=None, many=False)`

Executes an arbitrary SQL query.

-   **Arguments**:
    
    -   `query` (str): The SQL query.
        
    -   `params` (tuple or list of tuples, optional): Parameters for the query.
        
    -   `many` (bool): If `True`, executes multiple queries in a batch.
        

Example:

```
insert_query = "INSERT INTO users (id, name, email) VALUES (?, ?, ?)"
params = [
    (1, "Alice", "alice@fake.com"),
    (2, "Bob", "bob@fake.com")
]
db_manager.run_query(insert_query, params=params, many=True)
```

### `close()`

Closes the database connection.

## Example Usage

```
if __name__ == "__main__":
    db_manager = local_sql_db("example.db")

    # Create a table
    db_manager.create_table(
        "users",
        {
            "id": "INTEGER PRIMARY KEY AUTOINCREMENT",
            "name": "TEXT NOT NULL",
            "email": "TEXT UNIQUE NOT NULL",
            "created_at": "DATETIME DEFAULT CURRENT_TIMESTAMP"
        }
    )

    # Insert some data
    db_manager.insert_data(
        "users",
        [
            {"name": "James", "email": "james@example.com"},
            {"name": "Sam", "email": "sam@example.com"}
        ]
    )

    # Query the data
    results = db_manager.query_data("SELECT * FROM users WHERE name = ?", ("James",))
    for row in results:
        print(row)

    # Close the database connection
    db_manager.close()
```

## Requirements

-   Python 3.x
    

## License

This project is licensed under the MIT License.