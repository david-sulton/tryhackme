# SQL Fundamentals
![image](https://github.com/user-attachments/assets/62752799-3b5a-4ddd-b0e3-4a6ec43ac944)

Learning Objectives

    Understand what databases are, as well as key terms and concepts
    Understand the different types of databases 
    Understand what SQL is
    Understand and be able to use SQL CRUD Operations
    Understand and be able to use SQL Clauses Operations
    Understand and be able to use SQL Operations
    Understand and be able to use SQL Operators
    Understand and be able to use SQL Functions

## Introducing Databases

Okay, so you’ve been told just how important they are. Now, it's time to understand what they are in the first place. As mentioned in the introduction, databases are so ubiquitous that you very likely interact with systems that are using them. Databases are an organised collection of structured information or data that is easily accessible and can be manipulated or analysed. That data can take many forms, such as user authentication data (such as usernames and passwords), which are stored and checked against when authenticating into an application or site (like TryHackMe, for example), user-generated data on social media (Like Instagram and Facebook) where data such as user posts, comments, likes etc are collected and stored, as well as information such as watch history which is stored by streaming services such as Netflix and used to generate recommendations. 

I’m sure you get the point: databases are used extensively and can contain many different things. It’s not just massive-scale businesses that use databases. Smaller-scale businesses, when setting up, will almost certainly have to configure a database to store their data. Speaking of kinds of databases, let’s take a look now at what those are.

## Different Types of Databases

Now it makes sense that something is used by so many and for (relatively) so long that there would be multiple types of implementations. There are quite a few different types of databases that can be built, but for this introductory room, we are going to focus on the two primary types: relational databases (aka SQL) vs non-relational databases (aka NoSQL). 

Relational databases: Store structured data, meaning the data inserted into this database follows a structure. For example, the data collected on a user consists of first_name, last_name, email_address, username and password. When a new user joins, an entry is made in the database following this structure. This structured data is stored in rows and columns in a table (all of which will be covered shortly); relationships can then be made between two or more tables (for example, user and order_history), hence the term relational databases.

Non-relational databases: Instead of storing data the above way, store data in a non-tabular format. For example, if documents are being scanned, which can contain varying types and quantities of data, and are stored in a database that calls for a non-tabular format. Here is an example of what that might look like: 
```
 {
    _id: ObjectId("4556712cd2b2397ce1b47661"),
    name: { first: "Thomas", last: "Anderson" },
    date_of_birth: new Date('Sep 2, 1964'),
    occupation: [ "The One"],
    steps_taken : NumberLong(4738947387743977493)
}

```
In terms of what database should be chosen, it always comes down to the context in which the database is going to be used. Relational databases are often used when the data being stored is reliably going to be received in a consistent format, where accuracy is important, such as when processing e-commerce transactions. Non-relational databases, on the other hand, are better used when the data being received can vary greatly in its format but need to be collected and organised in the same place, such as social media platforms collecting user-generated content.
Tables, Rows and Columns

Now that we’ve defined the two primary types of databases, we’ll focus on relational databases. We’ll start by explaining tables, rows, and columns. All data stored in a relational database will be stored in a table; for example, a collection of books in stock at a bookstore might be stored in a table named “Books”. 

An illustration of a database table with rows and columns. The table has labeled columns at the top representing different data fields, such as 'ID', 'Name', and 'Published Date'. Each row below the headers contains data entries corresponding to these columns, forming individual records. The structure emphasizes how data is organized in a grid-like format, with each row representing a record and each column representing a specific attribute of the data.

When creating this table, you would need to define what pieces of information are needed to define a book record, for example, “id”, “Name”, and “Published_date”. These would then be your columns; when these columns are being defined, you would also define what data type this column should contain; if an attempt is made to insert a record into a database where the data type does not match, it is rejected. The data types that can be defined can vary depending on what database you are using, but the core data types used by all include Strings (a collection of words and characters), Integers (numbers), floats/decimals (numbers with a decimal point) and Times/Dates. 

Once a table has been created with the columns defined, the first record would be inserted into the database, for example, a book named “Android Security Internals” with an id of “1” and a publication date of “2014-10-14”. Once inserted, this record would be represented as a row.
Primary and Foreign Keys

Once a table has been defined and populated, more data may need to be stored. For instance, we want to create a table named “Authors” that stores the authors of the books sold in the store. Here is a very clear example of a relationship. A book (stored in the Books table) is written by an author (stored in the Authors table). If we wanted to query for a book in our story but also have the author of that book returned, our data would need to be related somehow; we do this with keys. There are two types of keys:

An illustration comparing a Primary Key and a Foreign Key in database tables. On the left, a table is shown with a highlighted column labeled 'Primary Key,' which uniquely identifies each record in that table. On the right, another table is displayed with a highlighted column labeled 'Foreign Key,' which references the Primary Key from the first table. Arrows connect the Foreign Key to the Primary Key, emphasizing the relationship between the two tables, where the Foreign Key enforces referential integrity by linking related data across tables.

Primary Keys: A primary key is used to ensure that the data collected in a certain column is unique. That is, there needs to be a way to identify each record stored in a table, a value unique to that record and is not repeated by any other record in that table. Think about matriculation numbers in a university; these are numbers assigned to a student so they can be uniquely identified in records (as sometimes students can have the same name). A column has to be chosen in each table as a primary key; in our example, “id” would make the most sense as an id has been uniquely created for each book where, as books can have the same publication date or (in rarer cases) book title. Note that there can only be one primary key column in a table.

Foreign Keys: A foreign key is a column (or columns) in a table that also exists in another table within the database, and therefore provides a link between the two tables. In our example, think about adding an “author_id” field to our “Books” table; this would then act as a foreign key because the author_id in our Books table corresponds to the “id” column in the author table. Foreign keys are what allow the relationships between different tables in relational databases. Note that there can be more than one foreign key column in a table.

## What is SQL?
Now, all of this theoretically sounds great, but in practice, how do databases work? How would you go and make your first table and populate it with data? What would you use? Databases are usually controlled using a Database Management System (DBMS). Serving as an interface between the end user and the database, a DBMS is a software program that allows users to retrieve, update and manage the data being stored. Some examples of DBMSs include MySQL, MongoDB, Oracle Database and Maria DB. 

The interaction between the end user and the database can be done using SQL (Structured Query Language). SQL is a programming language that can be used to query, define and manipulate the data stored in a relational database. 

## The Benefits of SQL and Relational Databases

SQL is almost as ubiquitous as databases themselves, and for good reason. Here are some of the benefits that come with learning and using to use SQL:

    It's fast: Relational databases (aka those that SQL is used for) can return massive batches of data almost instantaneously due to how little storage space is used and high processing speeds. 

    Easy to Learn: Unlike many programming languages, SQL is written in plain English, making it much easier to pick up. The highly readable nature of the language means users can concentrate on learning the functions and syntax.

    Reliable: As mentioned before, relational databases can guarantee a level of accuracy when it comes to data by defining a strict structure into which data sets must fall in order to be inserted.

    Flexible: SQL provides all kinds of capabilities when it comes to querying a database; this allows users to perform vast data analysis tasks very efficiently.


## Database Statements
CREATE DATABASE

If a new database is needed, the first step you would take is to create it. This can be done in SQL using the CREATE DATABASE statement. This would be done using the following syntax:
Terminal

           
mysql> CREATE DATABASE database_name;

        

Run the following command to create a database named thm_bookmarket_db:

 
Terminal

           
mysql> CREATE DATABASE thm_bookmarket_db;

        

 
SHOW DATABASES
Now that we have created a database, we can view it using the SHOW DATABASES statement. The SHOW DATABASES statement will return a list of present databases. Run the statement as follows:
Terminal

           
mysql> SHOW DATABASES;

        

In the returned list, you should see the database you have just created and some databases that are included by default (mysql, information_scheme, performance_scheme and sys), which are used for various purposes that enable mysql to function. Also present are various tables needed for this lesson.
USE DATABASE
Once a database is created, you may want to interact with it. Before we can interact with it, we need to tell mysql which database we would like to interact with (so it knows which database to run subsequent queries against). To set the database we have just created as the active database, we would run the USE statement as follows (make sure to run this on your machine):
Terminal

           
mysql> USE thm_bookmarket_db;

        

DROP DATABASE
Once a database is no longer needed (maybe it was created for test purposes, or is no longer required), it can be removed using the DROP statement. To remove a database, we would use the following statement syntax (although, in our case, we want to keep our database, so no need to run this one yourself!):
Terminal

           
mysql> DROP database database_name;

        

 
Table Statements
Now that you can create, list, use, and remove databases, it's time to examine how we would populate those databases with tables and interact with those tables. 
CREATE TABLE
Following the logic of the database statements, creating tables also uses a CREATE statement. Once a database is active (you have run the USE statement on it), a table can be created within it using the following statement syntax:
Terminal

           
mysql> CREATE TABLE example_table_name (
    example_column1 data_type,
    example_column2 data_type,
    example_column3 data_type
);

        

As you can see, there is a little more involved here. In the Databases 101 task, we covered how and when a table is created; it must be decided what columns will make up a record in that table, as well as what data type is expected to be contained within that column. That is what is represented by this syntax here. In the example, there are 3 example columns, but SQL supports many (over 1000). Let's try populating our thm_bookmarket_db with a table using the following statement:
Terminal

           
mysql> CREATE TABLE book_inventory (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    book_name VARCHAR(255) NOT NULL,
    publication_date DATE
);

        

This statement will create a table book_inventory with three columns: book_id, book_name and publication_date. book_id is an INT (Integer) as it should only ever be a number, AUTO_INCREMENT is present, meaning the first book inserted would be assigned book_id 1, the second book inserted would be assigned a book_id of 2, and so on. Finally, book_id is set as the PRIMARY KEY as it will be the way we uniquely identify a book record in our table (and a primary must be present in a table). 
Book_name has the data type VARCHAR(255), meaning it can use variable characters (text/numbers/punctuation) and a limit of 255 characters is set and NOT NULL, meaning it cannot be empty (so if someone tried to insert a record into this table but the book_name was empty it would be rejected. Publication_date is set as the data type DATE.
SHOW TABLES 
Just as we can list databases using a SHOW statement, we can also list the tables in our currently active database (the database on which we last used the USE statement). Run the following command, and you should see the table you have just created:
Terminal

           
mysql> SHOW TABLES;

DESCRIBE 
If we want to know what columns are contained within a table (and their data type), we can describe them using the DESCRIBE command (which can also be abbreviated to DESC). Describe the table you have just created using the following command:
Terminal

           
mysql> DESCRIBE book_inventory;

        

This will give you a detailed view of the table like so:
DROP Syntax

           
mysql> DESCRIBE book_inventory;
+------------------+--------------+------+-----+---------+----------------+
| Field            | Type         | Null | Key | Default | Extra          |
+------------------+--------------+------+-----+---------+----------------+
| book_id          | int          | NO   | PRI | NULL    | auto_increment |
| book_name        | varchar(255) | NO   |     | NULL    |                |
| publication_date | date         | YES  |     | NULL    |                |
+------------------+--------------+------+-----+---------+----------------+
3 rows in set (0.02 sec)

ALTER 
Once you have created a table, there may come a time when your need for the dataset changes, and you need to alter the table. This can be done using the ALTER statement. Let’s now imagine that we have decided that we actually want to have a column in our book inventory that has the page count for each book. Add this to our table using the following statement:
Terminal

           
mysql> ALTER TABLE book_inventory
ADD page_count INT;

        

The ALTER statement can be used to make changes to a table, such as renaming columns, changing the data type in a column or removing a column. 
DROP 
Similar to removing a database, you can also remove tables using the DROP statement. We don’t need to do this, but the syntax you would use for this is:
Terminal

           
mysql> DROP TABLE table_name;







    
