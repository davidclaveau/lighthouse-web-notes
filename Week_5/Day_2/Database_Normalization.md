# Database Normalization

* Normalization means our database has reduced duplicate data.

## Denormalized Database

* We want to avoid storing the same information in several places.

* For example, having a `cohort_name` in our `students` table.
  * Because `cohort_name` is repeated for multiple students, that means we would be best to split up this dataset into multiple tables
  * It is *normal* to create more tables when normalizing a database

## Normalization in Data

### First Normal Form (1NF)

* First normal form means that we make sure that no table contains multiple values per row
  * Each row should be unique and contain single values specific to the column

* Each table should be organized into rows, and each row should have a primary key that distinguises it as unique.

### Second Normal Form

* To achieve second normal form, it would be helpful to split out information that's relational to the another subset of information
  * Example: table of students and their pets
    * Heather has a cat and a dog, so we can have two rows for Heather specifying both pets, but we have duplicate data as Heather would appears twice in the same table
    * We should move `pets` into its own table

### Third Normal Form

* Third normal form would suggest making sure each non-key element in each table provides information about the key in the row.

* It is useful to include a unique primary key that distinguishes each data row from the others.

#### Fourth Normal Form

* Fourth normal form means that we show the relationship between tables as its own table.
  * For the students and pets example, one thing that could require fourth normal form is if a pet had shared ownership between students.
    * This means we associate the pet_id with the student_id in a third table `pets-students` to show these relationships

