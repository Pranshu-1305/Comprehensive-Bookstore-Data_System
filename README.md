# Comprehensive-Bookstore-Data_System
# 📚 Bookstore Database Project

A simple SQL-based project designed to manage and analyze data for a fictional bookstore.

---

## 🎯 Objective

The objective of this project is to:

- Design a basic relational database for managing bookstore operations.
- Practice writing SQL queries for data retrieval, reporting, and analysis.
- Understand basic and advanced SQL operations like `JOIN`, `GROUP BY`, `HAVING`, and aggregation functions.
- Demonstrate database skills useful for real-world applications or portfolio projects.

---

## 🗂️ Data Description

This database contains three main tables:

- **Customers**: Information about customers who purchase books.
- **Books**: Information about books available in the store.
- **Orders**: Transaction details about which customer bought which book and in what quantity.

Each table is connected logically:
- The `Orders` table links `Customers` and `Books` via foreign keys.
- Each order records the number of books sold and the total amount charged.

---

## 📁 Contents

- SQL Schema for:
  - `customers`
  - `books`
  - `orders`
- Basic SQL queries (select, filter, sort)
- Advanced SQL queries (aggregate functions, joins, grouping)
- Analysis on inventory, sales, and customer behavior

---

## 🛠️ Technologies Used

- SQL (PostgreSQL syntax, easily adaptable to MySQL or SQLite)
- Optional: Visualization tools like DBeaver or pgAdmin for browsing tables

---

## 🗺️ Schema Overview

### `customers`

| Column       | Type         | Description            |
|--------------|--------------|-------------------------|
| Customer_ID  | INT (PK)      | Unique customer ID      |
| Name         | VARCHAR(100)  | Customer's full name    |
| Email        | VARCHAR(100)  | Customer email address  |
| Phone        | VARCHAR(20)   | Customer phone number   |
| City         | VARCHAR(100)  | Customer's city         |
| Country      | VARCHAR(200)  | Customer's country      |

### `books`

| Column         | Type           | Description              |
|----------------|-----------------|---------------------------|
| Book_ID        | INT (PK)         | Unique book ID            |
| Title          | VARCHAR(100)     | Title of the book         |
| Author         | VARCHAR(100)     | Author of the book        |
| Genre          | VARCHAR(100)     | Genre of the book         |
| Published_Year | INT              | Year book was published   |
| Price          | NUMERIC(10,2)    | Price of the book         |
| Stock          | INT              | Number of books available |

### `orders`

| Column        | Type            | Description                   |
|---------------|-----------------|-------------------------------|
| Order_ID      | INT (PK)         | Unique order ID               |
| Customer_ID   | INT (FK)         | Customer who placed the order |
| Book_ID       | INT (FK)         | Book that was ordered         |
| Order_Date    | DATE             | Date the order was placed     |
| Quantity      | INT              | Number of books ordered       |
| Total_Amount  | NUMERIC(10,2)     | Total cost of the order       |

---

## 📊 Query Highlights

- 📘 Retrieve books by genre, year, and price.
- 📦 View available stock and manage inventory.
- 🛒 Analyze customer orders and sales patterns.
- 💵 Calculate total revenue and top spenders.
- 🏆 Identify best-selling books and popular genres.

---
## How to create?

CREATE TABLE customers(
Customer_ID int primary key,
Name varchar(100),
Email varchar(100),
Phone varchar(20),
City varchar(100),
Country varchar(200)
);

create table books (
Book_ID int primary key,
Title varchar(100),
Author varchar(100),
Genre varchar(100),
Published_Year int,
Price numeric(10,2),
Stock int
);

create table orders(
order_id int primary key,
customer_id int,
book_id int,
order_date date,
quentity int,
totle_amount numeric(10,0)
);

select * from customers;
select * from books;
select * from orders;
--Basic Queries
 -- Retrieve all books in the "Fiction" genre
 select * from books
 where gener = 'Fiction';
 -- Find books published after the year 1950
 select * from books
 where published_year>1950;
 -- List all customers from the Canada
 select * from customers
 where country='Canada';
 
 -- Show orders placed in November 2023
 select * from orders
   where order_date between '2023-11-01' and '2023-11-30'
 -- Retrieve the total stock of books available
 select sum(stock) as total_stock
 from books;
 -- Find the details of the most expensive book
select * from books Order by Price desc;
select * from books order by price desc limit 2;
 
 -- Show all customers who ordered more than 1 quantity of a book
 select * from orders
 where quentity>1;
 -- Retrieve all orders where the total amount exceeds $20
 select * from orders
 where totle_amount>20;
 -- List all genres available in the Books table
 select * from books
 where stock<1;
 select Distinct gener from books;
 -- Find the book with the lowest stock
 select * from books order by stock >0;
 -- Calculate the total revenue generated from all o
 
 select sum(totle_amount) as revanue_of_orders
 from orders;
 --Advance query
 --Retrieve the total number of books sold for each genre
select * from orders;
select b.gener, sum(o.quentity) as totle_books_sold
from Orders o
join Books b on o.book_id = b.book_id
group by b.gener;
 
 -- Find the average price of books in the "Fantasy" genre
 select Avg(price) as average_price
 from Books where gener = 'Fantasy';


 select * from books;
 -- List customers who have placed at least 2 orders
select o.customer_id, count(order_id) as order_count
from orders o
group by customer_id 
having count (order_id)>2;
--join two id

select o.customer_id, c.name,
count (o.Order_id)
as order_count
from orders o
join customers c on o.customer_id=c.customer_id
group by o.customer_id, c.name
having count (order_id)>=2;

select * from customers;
select * from orders;
 -- Find the most frequently ordered book
 --maximum
 select book_id, count(order_id) as order_count
 from orders
 group by book_id
 order by order_count desc limit 1;
--with name
 select o.book_id, b.title, count(o.order_id) as order_count
 from orders o
 join books b on o.book_id=b.book_id
 group by o.book_id, b.title
 order by order_count desc limit 5;
 -- Show the top 3 most expensive books of 'Fantasy' Genre  
 select * from books
 where gener = 'Fantasy'
 order by price desc limit 4;
 
 --Retrieve the total quantity of books sold by each author
select b.author, sum (o.quentity) as total_books_sold
from orders o
join books b on o.book_id=b.book_id
group by b.author;

 -- List the cities where customers who spent over $30 are located
 select distinct c.city, totle_amount from orders o
 join customers c on o.customer_id=c.customer_id
 where o.totle_amount>30;
 -- Find the customer who spent the most on orders
 select c.customer_id, c.name, sum (o.totle_amount)
 as total_spent
 from orders o
 join customers c on o.customer_id=c.customer_id
 group by c.customer_id, c.name
 order by total_spent desc;
 -- Calculate the stock remaining after fulfilling all order
 select b.book_id, b.title, b.stock
 from books b
 left join orders o on b.book_id=o.book_id
 group by b.book_id;
 
 select * from books;
 --import from aggrigation,
 select b.book_id, b.title, b.stock, coalesce (sum(quentity),0)
 as order_quentity from books b
 left join orders o on b.book_id=o.book_id
 group by b.book_id;
 
 


## 🚀 Usage

1. Import the schema into your SQL database.
2. Insert sample or real data.
3. Run provided queries to explore and analyze.
4. Modify or extend queries as per your project needs.

---

## 📌 Notes

- Queries are clean and beginner-friendly.
- Minor adjustments may be needed for different SQL engines (like MySQL or SQLite).
- Designed for educational and portfolio purposes.

---

## 📝 Conclusion

This project provides a foundational example of how SQL can be used to design, manage, and analyze a real-world business scenario — in this case, a bookstore.  
It covers essential database concepts such as table creation, relationships through foreign keys, data aggregation, and meaningful insights through complex queries.

Thanks for exploring this project! 🚀
