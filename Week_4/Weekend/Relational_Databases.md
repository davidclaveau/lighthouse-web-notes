# Relational Databases

* When storing your data, you should consider a few important concepts:
  * Size
  * Ease of Updating
  * Accuracy
  * Security
  * Redundancy
  * Importance

* This is what makes **databases** so important, as they can do all of these things reliably.

* A relational database is a type of database that organizes data into tables, and links them, based on defined relationships. These relationships enable you to retrieve and combine data from one or more tables with a single query.

---

[Relational Databases for Dummies](https://code.tutsplus.com/tutorials/relational-databases-for-dummies--net-30244)

* Relational databases should look to cull any repetition across *columns*, and should only have one value per field.
  * If you have a data field for `following_username` on Twitter, then this might have multiple names for multiple accounts. In this case, we would want to break this dataset into another database.
  * Removing repetitive data across columns is often called **the first normal form (1NF)**.

* When removing repetitive or duplicate information across *rows*, then you would want to split out the data further where we can sort by a data field, such as time.
  * One example would be for duplicate tweets by the same person, but *at different times*. We can break out their `tweets`, `created_at`, and username and create the relationship with a different table that contains their `username` and additional information.
  * Removing repetitive data across *rows* is often called **the second normal form (2NF)**.

* When data is split across several databases, then we need to create a relationship between the data.
  * The way to draw links between tables is to first give each row in a table a *unique identifier*, termed a **primary key**, and then reference that primary key in the other table to which you want to link.
  * A data field that's a primary key in one database can be used in another database as a **foreign key** to find data *related* to that user/dataset/etc.
  * With our example of duplicate tweets at different times, we split tweets out by tweet, tweet time, and username. To create a primary key for this database, we couldn't do tweet or time as those might be similar to other users (same tweet, same time tweeted), and the username is the foreign key.
    * Therefore, we should look to create a primary key with a unique ID to identify each row of this dataset.

> A primary key can be anything from a username to a unique, numerical ID. However, if a user changes their username, that can affect how data is pulled from other tables as the foreign key would also change. Something to keep in mind when creating keys in databases.

* For our *followers* database, we have a `from_user` and `to_user` data. While neither is useful for identifying the other, they can *both* be used as the primary key.
  * Likewise, they're both foreign keys to access the *users* table as they can be used to identify each other.

* This entire process is called **normalization** and its output is data that is cleanly organized according to the relational model.

---

* On the free and open source side, **MySQL**, **SQLite**, and **PostgreSQL** are three widely used solutions.

  * MySQL is used at just about every Internet company you have heard of. In context of this article, Twitter uses MySQL to store their users' tweets.

  * SQLite is common in embedded systems. iOS and Android let developers use SQLite to manage their app's private database. Google Chrome uses SQLite to store your browsing history, cookies, and your thumbnails on the "Most visited" page.

  * PostgreSQL is also a widely used RDBMS. Its PostGIS extension supplements PostgreSQL with geospatial functions that make it useful for mapping applications. A notable user of PostgreSQL is OpenStreetMap.

* Once you've downloaded and set up an Relational Database Management Software (RDBMS) on your system, the next step is to create a database and tables inside of it in order to insert and manage your relational data. The way you do this is with **Structured Query Language** (SQL)

---

## CRUD in SQL

* Create a database, named "development"
```
1 CREATE DATABASE development;
```

* Create a table named "users"
```
1 CREATE TABLE users (
2  full_name VARCHAR(100),
3  username VARCHAR(100)
4 );
```

* RDBMSs require that each *column* in a table is given a **data type**. So we assign the "full_name" and "username" columns the data type `VARCHAR` which is a string that can vary in width. The max length has been set to be 100. A full list of data types can be found [here](https://en.wikipedia.org/wiki/SQL#Data_types).

* Insert a record (the **Create** operation in CRUD)
```
1 INSERT INTO users (full_name, username)
2 VALUES ("Boris Hadjur", "_DreamLead");
```

* Retrieve all tweets belonging to `@_DreamLead` (the **Retrieve** operation in CRUD)
```
1 SELECT text, created_at FROM tweets WHERE username="_DreamLead";
```

* Update a user's name (the **Update** operation in CRUD)
```
1 UPDATE users
2 SET full_name="Boris H"
3 WHERE username="_DreamLead";
```

* Delete a user (the **Delete** operation in CRUD)
```
1 DELETE FROM users
2 WHERE username="_DreamLead";
```

---

## Entity Relationship Diagram (ERD)

* Is a graphical representation of the data requirements of a database.
  * Putting all the components of a database into a "box and line" format.

* Entity, Attribute, Primary Keys, Relationships, and Cardinality

* **Entity** represents a person, place, or thing that you want to track in a database
  * This will become a table in the database
  * Each occurrence of an entity is considered a *entity instance*

* **Attribute** describes various characteristics about an individual entity.
  * These will become the *columns* of the table.
  * These attributes will provide additional information to the rows of the table.

* **Primary Key** is an attribute or group of attributes that uniquely identifies an instance of an entity.
  * A **composite key** is using two attributes to create a unique identifier

* **Relationship** describes how one or more entitites interact with each other.
  * This is usually described with a verb.
  * Eg. a `student` *has* a `phone_number`

* **Cardinality** is the count of instances that are allowed or necessary between entity relationships.
  * *Minimum* cardinality represents the minimum number of instances that are required in the relationship. The fewest number of rows we can have in a relationship.
  * *Maximumm* cardinality is the maximum that we can have.
  * **Crow's foot notation**
    * One Mandatory (--||-) at least one and only one instance
    * Many Mandatory (---|<) at least one but can have several instances
    * One Optional (---O+) don't need to have an instance, but only one instance if you do
    * Many Optional (---O<) don't need to have an instance, but if you do you can have several instances

* A many-to-many relationship would produce several conflicts and errors if you were to attempt it.
  * In this case, you would need to create a **bridge table**.
  * We can combine the data from both tables while keeping it organized

---

## Using SQL

* We start by creating the table using `CREATE TABLE` and then we specify the name of the table, and the *columns* within the table. We would add the attribute of the column and the data type, such as `INTEGER` or `TEXT`
  * We also need to specify the `PRIMARY KEY` when we create the table.

```sql
/** Grocery list: 
Bananas (4)
Peanut Butter (1)
Dark Chocolate Bars (2)
**/

CREATE TABLE groceries (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER);
```

* Then we can start adding items (*rows*) to the database. We start with `INSERT INTO` and specify the database, then we add `VALUES` followed by the `id`, the `name` and the `quantity` we specified as attributes in the previous example.

* We can use `SELECT * FROM groceries;` to show the data in full.
  * the `*` is used to select all, otherwise we would specify the column name.

```sql
INSERT INTO groceries VALUES (1, "Bananas", 4);
INSERT INTO groceries VALUES (2, "Peanut Butter", 1);
INSERT INTO groceries VALUES (3, "Dark chocolate bars", 2);
SELECT * FROM groceries;
```

### Querying the Table

* We can use `ORDER BY` in our queries to sort the table by the column

```sql
SELECT * FROM groceries ORDER BY quantity;
```

* We can also sort by certain specification using `WHERE`

```sql
SELECT * FROM groceries WHERE quantity > 3 ORDER BY quantity;
```

* Aggregate function in SQL is an easy way to get averages, sums, and max/min from our databases.

* We can use function like `SUM()` or `MAX()` to grab the data from our table.

```sql
SELECT SUM(quantity) FROM groceries;
```

* We can use the `GROUP BY` term to also group the `SUM`s by `aisle` in the database

```sql
SELECT SUM(quantity) FROM groceries GROUP BY aisle;
```

* We can also compare the `SUM(quantity)` column that we pull out to the aisle numbers column by specifyling `aisle` after `SELECT` and separating it with a comma.

```sql
SELECT aisle, SUM(quantity) FROM groceries GROUP BY aisle;
```