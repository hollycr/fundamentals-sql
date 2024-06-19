# SQL Basics

Right, now we know a bit about databases and tables, we need to get our hands dirty. For this we will be using PostgreSQL (or just Postgres).

First we need to install it. Fortunately other wonderful people on the internet have provided [a handy installation guide](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql/).

> Don't forget the password you set!

Right. now that we have Postgres installed, we can to log in. Open a new Terminal and type:

```cmd
psql -U postgres
```

You will be asked for the password. Enter that and you've logged in to your postgres databse engine! You'll see the *prompt* has changed to 

```cmd
postgres=#
```

> You can see all the databases by typing `\l` to *list* the databases.

## Your first database

Okay, so far we haven't actually done much. There isn't even a database we want to use yet (remember, this is just your *view* of all the databases in your Postgres instance). So we'll want to create our own database. To do this we need to use some SQL.

SQL stands for Structured Query Language, and is the language of relational databases. It has a special syntax, or keywords, to tell it what to do. Whilst you create databases less often then tables, the syntax is very similar!

To create a database we need to use the keyword `CREATE` followed by the type of thing we want to create, in this case `DATABASE` and then the name of our database, followed by a semi-colon `;`.

Now, whilst SQL doesn't care if you type `CREATE` as `create` or `DATABASE` as `database` (it's case-insensitive for these things) it *does* care about the case for your database *name* and that you end the command with a semi-colon `;`.

Let's create our own database called `academy`:

```cmd
CREATE DATABASE academy;
```

When you press enter you should see Postgres confirm the creation by it printing `CREATE DATABASE` to the terminal:

```cmd
postgres=# CREATE DATABASE academy;
CREATE DATABASE
```

### Connecting to your database

Okay, so let's check our database has been created using the `\l` command, and that we can see `academy` listed.

Now, we can't do anything with that database yet, until we connect to it. Fortunately in Postgres that's super-simple as well using `\c <db_name>`. So for us to connect to the `academy` db, you type:

```cmd
\c academy
```

And you will see you command prompt change to:

```cmd
academy-#
```

Great! Let's now do something more exciting!!

## Create, Read, Update, Delete

There are four main things we want to be able to do with databases:

1. Create (C):

* This operation involves adding new data to the database, for example creating a new table or creating a new record in a table by inserting values into it.

2. Read (R):

* This operation involves retrieving existing data from the database. For example, you read data from a table by *querying* for certain data within it.

3. Update (U):

* This operation involves modifying existing data in the database. As an example, update an existing record in a table by changing the values of certain *columns*.

4. Delete (D):

* This operation involves removing existing data from the database. An example would be deleting records from a table based on certain conditions or delete all records from a table.

Together these are known a *CRUD* and form the basis of most operations on any persistent storage.

> Here's an example. When you first register with a website, it will *create* a record with your data. You might then *update* your address, or maybe add a *new* delivery address. In the end you might decide you no longer like their persistent advertising, and *delete* your account.

## CREATE

Tables are where your data is stored. You define tables with the `CREATE TABLE` statement, specifying column names and data types. Let's go ahead and do that:

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    department VARCHAR(100)
);
```

This creates a table named `employees` with columns `id`, `name`, `age`, and `department`.

* `id SERIAL PRIMARY KEY,` this line creates the `id` column and tells the database engine that we want to use the `id` column as our `PRIMARY KEY`. For now, you can think of this as a *index* field that auto-increments every time we add a new row. So it starts at 1 the first time we insert a row, 2 the next time, and so on.

> We cover primary keys a bit later.

* `name VARCHAR(100),` this line creates the `name` column and says it contains a string of characters up to 100 characters long.

* `age INT,` this line creates the `age` column and gives it a type of integer. Only whole numbers can be stored in this field.

* `department VARCHAR(100)` finally we create the `department` column, an tell it that it will contains a string of up to 100 characters.

### INSERT INTO

Now that we have a table, it's not much use without some data in it. As part of the C in CRUD, this is about *creating* new data. Let's create some data using the `INSERT INTO` statement.

```sql
INSERT INTO employees (name, age, department) VALUES ('John Doe', 30, 'Finance');
```

Excellent. We've now got an employee named `John Doe` who is aged 30, in the IT department.

##### Wait, why didn't we insert the ID?

You may have noticed we *didn't* include the `id` field here. That's because we defined it as an auto-incrementing field when we created the table, so the database takes care of that for us when we insert data into it.

## READ

Okay, now here's the bit you mainly do a lot of, which is reading data using a *query*. For that we use the `SELECT` statement:

```sql
SELECT * FROM employees;
```

This has a special syntax using a *wildcard* - the asterisk `*`. This is special shorthand for "give me all the data from this table - all the columns and all the rows".

Hopefully this syntax makes sense - `SELECT <something> FROM <table_name>`.

### Filtering queries with WHERE

We can also *filter* data to return only data that matches certain conditions. To prove this, we need to insert a new user. Execute the following command to create another row:

```sql
INSERT INTO employees (name, age, department) VALUES ('Jane Doe', 31, 'IT');
```

Now we can query our data again using the `SELECT * FROM employees;`, which will show you two rows.

To get only ONE of those rows back, we can add filters, using a `WHERE` clause:

```sql
SELECT * FROM employees WHERE department = 'IT';
```

This command will return one row from the table, that of Jane Doe, as that's the only employee whose department matches the WHERE clause.

Again, this syntax should be fairly readable as `SELECT <everything> FROM <table_name> WHERE <column_name> = <value>`

> TASK: Can you select all data from the table where the age is less than 31?
>
> you can use `<` `<=` `>=` `>` and `=` in SQL WHERE clauses!

### Filtering for certain columns

If we only want certain data back from our table, such as the employee name, we can specify the column name (or names) we want. This is very useful when you're only interested in a few columns data, as tables can be very large!

```sql
SELECT name FROM employees WHERE department = 'IT';

SELECT name, age FROM employees WHERE department = 'IT';
```

## UPDATE

When we want to change existing data, we use the `UPDATE` keyword:

```sql
UPDATE employees SET age = 31 WHERE name = 'John Doe';
```

You'll notice we use a `WHERE` clause here. That's because we only want to update a single user. If you don't add a `WHERE` clause to your `UPDATE` statement, then it will update *all* the rows in the database!

NOTE: You wouldn't normally do this by the user name, as in large companies there are often people with the same name and it would update both employees! Instead you'd use the *primary key*:

```sql
UPDATE employees SET age = 31 WHERE id = 1;
```

You can also update multiple fields at once:

```sql
UPDATE employees SET age = 31, name = 'New Name' WHERE id = 1;
```

## DELETE

Unsurprisingly, you delete users using the `DELETE` keyword:

```sql
DELETE FROM employees WHERE id = 1;
```

Again, hopefully the syntax makes sense here, basic SQL is pretty straightforward!

If you want to delete everything from a table:

```sql
DELETE FROM employees;
```

## UPDATE Part 2 - altering our table structure

As part of the Update bit of CRUD, we'll often want to be able to alter our tables, by adding new columns, or removing ones we no longer need.

### Add a column

Let's assume we forgot to add an `email` column:

```sql
ALTER TABLE employees ADD COLUMN email VARCHAR(100);
```

Using the `ALTER` keyword, we say we want to alter a `TABLE`, then give it the name of the table (employees).

Then we tell it the *type* of alteration we want, in this case we want to add a column so we use `ADD COLUMN`, give it the name of the column we want to add (email), and finally its data type `VARCHAR(100)`.

### Modify a column

We can also modify a column (and add constraints like NOT NULL, but we won't be covering that here).

Maybe we found out that emails can be longer than 100 characters, and up to 255. We'd alter our existing column like so:

```sql
ALTER TABLE employees ALTER COLUMN email TYPE VARCHAR(255);
```

### Remove a column

Finally, we realise that we don't even need the email column! In SQL when we want to remove something entirely, we use the `DROP` keyword:

```sql
ALTER TABLE employees DROP COLUMN email;
```

## Exercises

1. Create a new table named `addresses`, with the columns `id` are primary key, `address_line_1`, `address_line_2`, `town`, and `postcode`, with the right VARCHAR length you choose.

2. Add three new, unique addresses to the table:

| address_line_1 | address_line_2 | town      | postcode  |
|----------------|----------------|-----------|-----------|
| 123 Main St    | Apt 101        | Anytown   | KL12 3CD  |
| 456 Elm St     | Door 6         | Newtown   | XY45 6EF  |
| 789 Oak St     | Suite 202      | Othertown | KL78 9GH  |


3. Add a new column `county` to the table.

4. Update all the addresses to be in the *same* county.

5. Delete any addresses where the postcode *starts* with "KL" using `LIKE`: https://www.w3schools.com/sql/sql_like.asp

---

Okay, so now we've got those basics down, we can actually look at the power of relational databases!

[>> Relationships between tables](/chapter5/chapter5.md)