# Database Design

## Primary and Foreign Keys

* Primary Key: field values that uniquely identify a particular record within a table
* A PK must be unique (within the table) and never be null
* A PK's data type is usually auto-incrementing integer (`INTEGER` or `BIGINT`)
* A Foreign key is formed from field values that match a Primary Key stored in another table
* The Primary Key and Foreign Key MUST be the same data type
  * `SERIAL` and `INTEGER` are (deep under the hood) the *same* data type as `SERIAL` is an `INTEGER` within its definition

## Naming "Conventions"

* Table and field names are written in snake_case
* Table names are always pluralized
  * A table for *widgets* contains multiple widgets, so it's plural
* The primary key for each table will simply be called `id`
  * The vast majority of the time this will be the case, but a very small percentage may break this convention
* A foreign key's name is the **singular** of the primary key's table appended with `_id` (eg. `user_id` is the foreign key pointing to the `id` field in the `users` table)
  * `day_id` of the `days` table - which *day* of the table are we referencing? `day_id`.

## Data Types

* Each field in a table must have a data type defined for it
* The data type tells the database how much room to set aside to store the value and allows the database to perform type validation on data before insertion (to protect the data integrity of the table)
  * If speed is an importance, consider data types of a smaller size for small improvements in performance
* Choosing the perfect data type is less of a concern nowadays because disk space is now comparably cheap
  * Grappling with the natural tension between competing requirements
* We can change components of a table using `ALTER` to change datatype specifications

## Relationship Types

* One-to-One: One record in the first table is related to one (and only one) record in the second table
  * It's a pretty rare occurrence
  * Two separate tables where one row in one table corresponds to another row in another table - and it always should be there
  * Eg. users for a system, so we have a `users` table; some users are `administrators` - do we have two different tables? Not usually.
* One-to-Many: One record in the first table is related to zero or more records in the second table
  * Eg. `days` and `objectives` from our examples. We have `assignments` assigned to our `objectives` table
    * Many rows in our `objectives` table that correspond to *one day* in our `days` table (different things to do in `objectives` on day 22).
* Many-to-Many: One or more records in the first table are related to one or more records in the second table


