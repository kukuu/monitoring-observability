# Database Setup

```
const sqlite3 = require('sqlite3').verbose();
const db = new sqlite3.Database('conveyor.db');
```

- Imports the SQLite3 library (verbose mode for detailed errors).

- Creates/connects to a SQLite database file named conveyor.db.

## Table Creation

```

db.serialize(() => {
  db.run(`CREATE TABLE IF NOT EXISTS results (
    id INTEGER PRIMARY KEY,
    unusedA INTEGER,
    unusedB INTEGER,
    productsC INTEGER,
    timestamp DATETIME DEFAULT CURRENT_TIMESTAMP
  )`);
});
```

- db.serialize() ensures queries run sequentially.

- Creates a table named results (if it doesn’t exist) with:

i. id: Auto-incrementing primary key.

ii. unusedA/unusedB: Counters for unused components.

iii. productsC: Counter for assembled products.

iii. timestamp: Automatically records insertion time.


## Saving Results


```
function saveResults(unusedA, unusedB, productsC) {
  db.run(`INSERT INTO results (unusedA, unusedB, productsC) VALUES (?, ?, ?)`, 
    [unusedA, unusedB, productsC]);
}

```


- Inserts data into the results table with:

- Placeholders (?) to prevent SQL injection.

- Values passed as an array [unusedA, unusedB, productsC].

- Automatically adds the current timestamp (from DEFAULT CURRENT_TIMESTAMP).

## Key Purpose

- Tracks factory metrics:

i. Unused components (unusedA, unusedB).

ii. Products assembled (productsC).

iii. Timestamps for analysis over time.

## Example Usage

```
saveResults(5, 3, 10); // Logs: unusedA=5, unusedB=3, productsC=10

```
- This would insert a row into the results table with the provided values.

## Saving results to DB

To save the results from the logs (e.g., saveResults(5, 3, 10)) into the SQLite table and verify it worked via the command line, follow these steps:

saveToDB.js
1. Full JavaScript Code (with Error Handling)


```

const sqlite3 = require('sqlite3').verbose();
const db = new sqlite3.Database('conveyor.db');

// Create table (if not exists)
db.serialize(() => {
  db.run(`CREATE TABLE IF NOT EXISTS results (
    id INTEGER PRIMARY KEY,
    unusedA INTEGER,
    unusedB INTEGER,
    productsC INTEGER,
    timestamp DATETIME DEFAULT CURRENT_TIMESTAMP
  )`);
});

// Save results to the database
function saveResults(unusedA, unusedB, productsC) {
  db.run(
    `INSERT INTO results (unusedA, unusedB, productsC) VALUES (?, ?, ?)`,
    [unusedA, unusedB, productsC],
    function(err) {
      if (err) {
        console.error('Error saving results:', err.message);
      } else {
        console.log(`Saved to DB: unusedA=${unusedA}, unusedB=${unusedB}, productsC=${productsC}`);
      }
    }
  );
}


// Example usage (from logs)


saveResults(5, 3, 10); // Insert values into the table

// Close the DB connection after saving
db.close();

```

**2. How to Run & Verify**

Step 1: Save the Code

Save the code above to a file (e.g., saveToDB.js).

**Step 2: Install SQLite3**

Ensure you have the sqlite3 npm package installed:

```
npm install sqlite3
```
**Step 3: Run the Script**

Execute the script to save the data:

```
node saveToDB.js
```

Output:


Saved to DB: unusedA=5, unusedB=3, productsC=10

**Step 4: Verify in Command Line**

Use the SQLite CLI to check the table:

```
sqlite3 conveyor.db "SELECT * FROM results;"
```

Expected Output:


1|5|3|10|2024-07-20 14:30:00  # (timestamp will vary)


**Key Notes**

i. Error Handling: The callback in db.run() logs errors if insertion fails.

ii. Timestamp: Automatically added by SQLite (CURRENT_TIMESTAMP).

iii. Persistence: Data remains in conveyor.db even after script exits.
