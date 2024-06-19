# Relationships between tables

The power of relational databases comes from *relationships* between tables.

We can split up our data into separate tables, each one describing some kind of group of data. For instance, we have created above two tables, one of which contains employee name, age, and department, and addresses, but they are not connected, or joined, to each other. Currently the data is *unrelated* between tables.

We want to be able to relate the *employees* to an *address*.

Definitions:

* **Relation**  - set of tuples, called a Table

* **Relationship**  - associations between tables created using join statements to retrieve data

Using more than one table allows us to change avoid duplication in our data – so we only ever have one copy of any fact

Let’s use the examples of Students we used in the first part – this time we will split out Addresses into its own table.

## Kinds of Relationship

There are different ways we can relate data in one table to another:

* One to One - there is exactly one match in another table related to this 

Example, you might only have one department you belong to

* One to Many - there are many matches in another table to this  

Example, you might have many addresses you want things to be delivered to

* Many to Many - there are many matches to each other in both tables

Example, there are multiple students on a course, and a course has multiple students

## Relationship – One-To-one

Let’s take our previous Student table and split it in two.
Put name and date of birth details in the Student table
Split out address details into a new Address table

To link where a student lives to their name, we need a relationship between these tables

We can use a simple one-to-one relation. Every student has exactly one address.

To do that we need two new things:
1. An *id* column in the address table which uniquely identifies an address
2. A new column in the Students table which holds the *address_id* of the address they live at

![](/img/sql19.png)

Can you see how that works?
`John McGill` lives at *address_id* `1`. We can look that up in the Address table and find the details of `Elm Street, SP1 1AB`

This is called a JOIN across tables.

The `id` is called a *PRIMARY KEY* for rows in the Address table. It must uniquely identify each row.

The `address_id` column in the Student table is called a *FOREIGN KEY*

A FOREIGN KEY is simply something that can be found as a Primary Key in another table

We can now use this in our SQL to *join* the tables and get the students data with all their associated address information:

```sql
SELECT *
FROM students
LEFT JOIN addresses ON students.address_id = addresses.id;
```

A `LEFT JOIN` returns all rows from the left table (the table specified before the JOIN keyword) and the matched rows from the right table (the table specified after the JOIN keyword). If there is no match found in the right table, NULL values are returned for the columns of the right table.

> :thought_balloon: FOREIGN KEY-PRIMARY KEY pairs are how all relations are constructed

## Exercises

1. Create a new database called `university`. (To exit your current connection if you are still in the academy db, use `\c postgres` first).

2. Create the students and address tables as specified above.

3. Create the JOIN to return all students and their address.

## QUIZ : One-to-one

![](/img/sql20.png)

If things go well and Mary Johnson moves in to live with John McGill, what change would we need to make to the Student table?

## Answer

![](/img/sql21.png)

[A: student_number 105 address_id changes to 2 from 1]

## How could we solve this?

> * We have many students
> * We have many seminars
> * Each student attends many seminars
> * Each seminar has many students

> How do we store which seminars a student is enrolled on?
>  * Hint:  **one**  column can only have  **one**  value in  **one**  row

So here we have a common problem, complex data relationships.

We have many students who may attend many seminars, but we also need to know how many students there are at each seminar. How do we store this?

Answers coming up!

---

## RELATIONSHIPS : One-To-Many / Many-To-Many

More complex data needs more complex relationships.

This is how we can model Students that enrol on Seminars.
One student can attend many seminars – and each seminar will have many students.

![](/img/sql22.png)

This is a classis MANY-TO-MANY relationship.
We do it by breaking it down into a ONE-TO-MANY and a MANY-TO-ONE relationship.

Look at Student-Enrolment. That’s a ONE-TO-MANY. It needs:
* StudentID in the student table to identify each student
* Student_number foreign key IN THE ENROLMENT table

This is different to one-to-one; it is “backwards”. We can have many rows in the Enrolment table all linking back to the same student:

![](/img/sql23.png)

The Enrolment table is called a LINK TABLE as it links together the Student and Seminar tables.
Have a look at the bottom half of this diagram. It shows actual rows.

Look at the top row of the Enrolment tables. It says that student 100 is on seminar C1000

If you look at the fourth row down, you can see that another student is on seminar C1000. This time student 110.

Seminar C1000 has students 100 and 110 on it.

You can also find out that :
* Seminar C1030 has student 110 on it
* Student 100 has enrolled for two seminars, C1000 and C1011

Naming the link table is REALLY HARD – You need a NOUN that describes what the relationship is. Good luck!

Relationships allow us to run QUERIES on our data like this. That’s powerful.

## RELATIONSHIPS : KEYS

A little more on keys. Every relationship is simply a reference to a PRIMARY KEY in some table. It’s that simple.

![](/img/sql24.png)

PRIMARY KEYS uniquely identify a row in a table, which means they uniquely identify some “thing” in the real world.

FOREIGN KEYS tell us how to link two tables together. If a row has a foreign key to some other table, we can JOIN.

## What happens when we delete rows?

Those tables might refer to rows that don’t exist anymore

What should we do?

## RELATIONSHIPS : Referential integrity

Here’s a question:
*What happens when we delete a row that some other table is linking to?*

A: We can no longer look up the linked data

We have broken *REFERENTIAL INTEGRITY* – the name given to making sure all our links work

There are 5 main ways to deal with that:
1. HIDDEN FLAG – use an extra column to say “do not include this row in results”
2. CASCADE DELETE – When we delete a row, delete rows in other tables that link to it
3. DO NOTHING – this can lead to a PHANTOM ROW which is unreachable from any other data
4. SET NULL – set the foreign key to NULL to indicate we don’t have anything to link to
5. SET DEFAULT – use a default value, if that makes sense -  eg a Head Office Telephone number, if we delete a local phone number

![](/img/sql25.png)

## Example - Airbnb

Finally for this section, here’s an example of how powerful these techniques can be.

These are the tables, columns and relationships you would need to build an AirBnB clone.

![](/img/sql26.png)

It features around bookings, which are between hosts and users.
A booking has a Place in some City, in a Country.

Each booking can have one-to-many Reviews attached to it.

That’s all modelled here in this ENTITY RELATION DIAGRAM or *ERD*.

Powerful, isn’t it?

---

Okay, now we have relationships between tables, it's time to come to our last part.

[>> Data Manipulation Language and Data Definition Language](/chapter6/chapter6.md)