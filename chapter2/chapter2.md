# There are many database engine

There are many kinds of Database systems we can use.

* Some cost money, some are free of charge.

* Some are open source, some not

They split up into groups depending how they work with data:

![](/img/sql2.png)


> BJSS clients often use Microsoft, Postgres, Oracle


## Types of Database

Different kinds of data are best suited to different kinds of database

Let’s look at the main types available today

### Database models : Key-Value

The simplest kind of database is a key-value store
THINK: session data for a website – just throw whatever user data we need as a “lump of data”, store against a session key.

You take your data - like your records of a Customer and their orders - and store that whole lot against a key. The key will be the Customer ID you've given that customer.

![](/img/sql5.png)

You can store data and look it up. 

To modify it, you have to fetch the whole lot of data, change it in memory, then store it all back.

These offer fast performance, as they don't do much. Example:  **Redis**

### Database models: Document

A Document database is the next step up from a key-value store.

You still store 'a lump' of data. But you don't use a key. Instead, you can query for any attribute of that data.

Store ‘whole documents’ as JSON, XML, BSON, RDF etc
  * Documents are mainly hierarchical tree data structures consisting of maps, collections, arrays, scalar values.

![](/img/sql8.png)

In the example above, we could return all documents where "city equals Bristol" - and we would get our document back. 

Example:  **MongoDB**


### Database models: GRaph

![](/img/sql11.png)

A Graph database is a different kind entirely.

It stores the mathematical idea of a graph - a bunch of nodes that can be joined together by vertices. 

The most obvious use is to store social graphs - each node has data about a single person. Each connection stores data about how those people are connected and facts relating to their relationship.

These facts inside the nodes and attached to the relationships are stored in the database.

### Database models: Time-series

A specialist kind of database is the Time Series database.

A lot of engineering data streams from sensors, and needs to capture both the data point and the time it was observed. 

Particle detectors at CERN are one example
Other good uses are server metrics, application monitoring and IoT events

These databases are good at time-based queries and aggregating time-series data

It could be : server metrics, application performance monitoring, network data, sensor data (IoT), events, clicks, TICK data – equity markets etc.

Example:  **Graphite**

| timestamp | deviceid | value |
| :-: | :-: | :-: |
| 2020-09-03 17:29:14.091219 | 1 | 90.0 |
| 2020-09-03 17:29:14.969014 | 2 | 75.0 |
| 2020-09-03 17:29:16.615036 | 3 | 81.0 |

---

### STILL RELEVANT: Relational Databases (2022)

![](/img/sql14.jpg)

The most widely used kind of database is the relational database, and the graph hasn't changed much over the years, the big guns are still Oracle, MySQL, Microsoft and Postgres.

---

[>> Relational Databases - building blocks](/chapter3/chapter3.md)