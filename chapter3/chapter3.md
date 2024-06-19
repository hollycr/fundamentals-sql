# Relational Database : Building blocks

Relational databases were invented by IBM in the 1970s by Ted Codd and have been used ever since.

Every relational database has the same foundations.

Data is stored in tables, which themselves break down into rows and columns, each with a special meaning.

The easiest way to understand it is to break down an example.

## Basic table

Data is organized in Tables in a relational database.

This is an example of a database table storing student data.
The table stores data about all students.

![](/img/sql16.png)

You can see the table is divided up into three rows – one row per student

Each row is divided up into columns – one column for each fact about a student

In the first row, a student called `John McGill` lives at `Elm Street` with postcode `SP1 1AB`.

Each row has an identical structure – it contains the same pieces of data about a different student.

The second row resembles the first, but holds data about `Mary Johnson` instead of `John McGill`

![](/img/sql17.png)

It is this regular nature that helps us fetch and process data using code.

A table can have unlimited rows, limited only by practical matters like speed and system memory.

## Column Definitions

Columns are used to add meaning to each piece of data in a row.

It has a name that explains what the data means, and a data type. You can find [more on data types here](https://www.w3schools.com/sql/sql_datatypes.asp), and we'll use some later. This data type lets our database store and search for data efficiently.

![](/img/sql18.png)

Columns can be given other attributes - sometimes known as constraints - as well:
* NOT NULL means we must supply a value
* DEFAULT is the value we should store in that column if nothing is provided

> :thought_balloon: Why is everything in UPPER CASE? No reason – it is convention from ancient days. 
>
> Any case can be used. 

---

Now we've learnt a bit about what databases are and the various types available. Now we're going to look at Structured Query Language (SQL) which allows us to create tables, insert and update data, and query for data.

[>> SQL Basics](/chapter4/chapter4.md)