#Booktown, USA

For each question below, find the approriate SQL query to obtain the information requested. Create a `.txt` or `.md` file that contains all of your answers.

##Getting Started

To get started we'll need to import the booktown.sql file.

1. Fork and clone this repository
2. `cd` into the repository
3. use the command `psql -f booktown.sql`
4. type `psql` to open your psql console
5. type \list to ensure the booktown database was successfully completed
6. type `\c booktown` to connect to the booktown database
7. type `\d` to see a list of all the tables in the booktown database
8. type `\d [TABLE_NAME]` to see information about columns and their types for a specific table. You should see output like below:

```
booktown=# \d books
       Table "public.books"
   Column   |  Type   | Modifiers
------------+---------+-----------
 id         | integer | not null
 title      | text    | not null
 author_id  | integer |
 subject_id | integer |
Indexes:
    "books_id_pkey" PRIMARY KEY, btree (id)
    "books_title_idx" btree (title)
```

###Additionally...

Your life will be made easier with a GUI PostgreSQL client. We downloaded these during the installfest. Open up **Postico** if you have a Mac, or **pgAdmin** if you have Linux.

If you're missing the PostgreSQL client, download [Postico here](https://eggerapps.at/postico/) if you have a Mac, or [pgAdmin here](http://www.pgadmin.org/) if you have Linux.

1. Postico asks for a lot of information to begin with. The defaults are fine. Leave everything alone and just press connect.
2. If you don't see the booktown database after connecting you may need to move up a directory. Press the "localhost" under the back and forward buttons.
3. Double click on the `booktown` database to connect.
4. See the list of tables in the database (alternate_stock, authors, book_backup...)
5. Double click a table to see it's contents
6. Double click SQL Terminal to get to a text box where you can write and execute some queries.

![Postico new localhost connection](images/postico/00-postico-localhost-connection.jpg)
![Postico databases](images/postico/01-postico-databases.jpg)

## Queries

Complete the following exercises to practice using SQL.

###Order
1. Find all subjects sorted by subject

<!-- SELECT * FROM subjects ORDER BY subject ASC; -->

2. Find all subjects sorted by location

<!-- SELECT * FROM subjects ORDER BY location ASC; -->

###Where
1. Find the book "Little Women"

<!-- SELECT * FROM books WHERE title = 'Little Women'; -->

2. Find all books containing the word "Python"

<!-- SELECT * FROM books WHERE title LIKE '%Python%'; -->

3. Find all subjects with the location "Main St" sort them by subject

<!-- SELECT * FROM subjects WHERE location = 'Main St' ORDER BY subject ASC; -->


###Joins

* Find all books about Computers list ONLY book title

<!-- SELECT title FROM books JOIN subjects ON books.subject_id = subjects.id WHERE subjects.id = 4; -->

* Find all books and display a result table with ONLY the following columns
	* Book title

<!-- SELECT title FROM books; -->

	* Author's first name

<!-- SELECT authors.first_name FROM authors JOIN books ON books.author_id = authors.id; -->

	* Author's last name

<!-- SELECT authors.last_name FROM authors JOIN books ON books.author_id = authors.id; -->

	* Book subject

<!-- SELECT subjects.subject FROM subjects JOIN books ON books.subject_id = subjects.id; -->
<!-- SELECT DISTINCT ON (subjects.subject) subjects.subject FROM subjects JOIN books ON books.subject_id = subjects.id; -->

* Find all books that are listed in the stock table
	* Sort them by retail price (most expensive first)

<!-- SELECT table_name FROM information_schema.columns WHERE column_name = 'isbn'; -->
<!-- SELECT * FROM stock JOIN editions ON stock.isbn = editions.isbn ORDER BY retail ASC; -->

	* Display ONLY: title and price

<!-- SELECT books.title, stock.retail FROM stock JOIN editions ON stock.isbn = editions.isbn JOIN books ON editions.book_id = books.id; -->

* Find the book "Dune" and display ONLY the following columns
	* Book title

<!-- SELECT title FROM books WHERE title = 'Dune'; -->

	* ISBN number

<!-- SELECT table_name FROM information_schema.columns WHERE column_name = 'isbn'; -->
<!-- SELECT editions.isbn FROM books JOIN editions ON books.id = editions.book_id WHERE books.title = 'Dune'; -->

	* Publisher name

<!-- SELECT publishers.name FROM books JOIN editions ON books.id = editions.book_id JOIN publishers ON editions.publisher_id = publishers.id WHERE books.title = 'Dune'; -->
<!-- SELECT DISTINCT ON (publishers.name) publishers.name FROM books JOIN editions ON books.id = editions.book_id JOIN publishers ON editions.publisher_id = publishers.id WHERE books.title = 'Dune'; -->

	* Retail price

<!-- SELECT table_name FROM information_schema.columns WHERE column_name = 'retail'; -->
<!-- SELECT stock.retail FROM stock JOIN editions ON stock.isbn = editions.isbn JOIN books ON editions.book_id = books.id WHERE books.title = 'Dune'; -->

* Find all shipments sorted by ship date display a result table with ONLY the following columns:
	* Customer first name

<!-- SELECT customers.first_name FROM customers JOIN shipments ON customers.id = shipments.customer_id ORDER BY shipments.ship_date ASC; -->

	* Customer last name

<!-- SELECT customers.last_name FROM customers JOIN shipments ON customers.id = shipments.customer_id ORDER BY shipments.ship_date ASC; -->

	* ship date

<!-- SELECT ship_date FROM shipments ORDER BY ship_date ASC; -->

	* book title

<!-- SELECT books.title FROM books JOIN editions ON books.id = editions.book_id JOIN shipments ON editions.isbn = shipments.isbn ORDER BY ship_date ASC; -->

###Grouping and Counting

1. Get the COUNT of all books

<!-- SELECT COUNT(*) FROM books; -->

* Get the COUNT of all Locations

<!-- SELECT table_name FROM information_schema.columns WHERE column_name = 'location'; -->
<!-- SELECT COUNT('subjects.location') FROM subjects; -->
<!-- SELECT COUNT(*) FROM subjects; -->
<!-- SELECT COUNT(*) FROM subjects WHERE location NOTNULL; -->
<!-- SELECT COUNT(location) FROM subjects; -->

* Get the COUNT of each unique location in the subjects table. Display the count and the location name. (hint: requires GROUP BY).

<!-- SELECT location, COUNT(location) FROM subjects WHERE location NOTNULL GROUP BY location; -->

* List all books. Display the book_id, title, and a count of how many editions each book has. (hint: requires GROUP BY and JOIN)





####YAY! You're done!!

---

## Licensing
1. All content is licensed under a CC-BY-NC-SA 4.0 license.
2. All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.
