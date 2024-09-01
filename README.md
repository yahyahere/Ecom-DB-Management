# EcomMangement
The E-Commerce Database Management System (DBMS) is designed to efficiently manage the data of an online retail store. The project focuses on the core functionalities required to run an e-commerce platform, such as managing customer information, product details, orders, and payments, along with ensuring data integrity and security.

E-Commerce-Database-Management-System
As a part of our course, I created this project for Database Management Systems (DBMS). This project includes both theoretical concepts and implementation in SQL.

Contents
Project Description
Basic Structure
Functional Requirements
Entity-Relationship (ER) Diagram and Constraints
Relational Database Schema
Implementation
Creating Tables
Inserting Data
Pre-requisite
MySQL
Project Description
The E-Commerce Database Management System (DBMS) is designed to efficiently manage the data of an online retail store. The project focuses on the core functionalities required to run an e-commerce platform, such as managing customer information, product details, orders, and payments, along with ensuring data integrity and security.

Relational Database Schema - E-Commerce - ER Diagram

ER Diagram

Create Schema (Database) in MySQL
sql
Copy code
CREATE SCHEMA ecommerce_db;
Create Tables
Customer Table

sql
Copy code
CREATE TABLE ecommerce_db.Customer (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE NOT NULL,
    dob DATE,
    phone_number VARCHAR(15),
    registration_date DATE DEFAULT CURRENT_DATE
);
Product Category Table

sql
Copy code
CREATE TABLE ecommerce_db.ProductCategory (
    category_id INT PRIMARY KEY AUTO_INCREMENT,
    category_name VARCHAR(100) UNIQUE NOT NULL,
    description TEXT
);
Supplier Table

sql
Copy code
CREATE TABLE ecommerce_db.Supplier (
    supplier_id INT PRIMARY KEY AUTO_INCREMENT,
    supplier_name VARCHAR(100) NOT NULL,
    contact_number VARCHAR(15),
    total_revenue DECIMAL(10, 2)
);
Address Table

sql
Copy code
CREATE TABLE ecommerce_db.Address (
    address_id INT PRIMARY KEY AUTO_INCREMENT,
    house_number VARCHAR(10),
    street_name VARCHAR(100),
    city VARCHAR(50),
    state VARCHAR(50),
    postal_code VARCHAR(10),
    country VARCHAR(50),
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id) ON DELETE CASCADE
);
Product Table

sql
Copy code
CREATE TABLE ecommerce_db.Product (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    stock_quantity INT NOT NULL,
    brand VARCHAR(50),
    category_id INT,
    supplier_id INT,
    FOREIGN KEY (category_id) REFERENCES ProductCategory(category_id) ON DELETE SET NULL,
    FOREIGN KEY (supplier_id) REFERENCES Supplier(supplier_id) ON DELETE SET NULL
);
ShoppingCart Table

sql
Copy code
CREATE TABLE ecommerce_db.ShoppingCart (
    cart_id INT PRIMARY KEY AUTO_INCREMENT,
    total_amount DECIMAL(10, 2),
    total_items INT,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id) ON DELETE CASCADE
);
Order Table

sql
Copy code
CREATE TABLE ecommerce_db.Orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10, 2),
    order_status ENUM('Shipped', 'Processing', 'Delivered', 'Cancelled') DEFAULT 'Processing',
    shipping_date DATETIME,
    customer_id INT,
    cart_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id) ON DELETE SET NULL,
    FOREIGN KEY (cart_id) REFERENCES ShoppingCart(cart_id) ON DELETE SET NULL
);
OrderDetails Table

sql
Copy code
CREATE TABLE ecommerce_db.OrderDetails (
    order_detail_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT,
    product_id INT,
    unit_price DECIMAL(10, 2),
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES Product(product_id) ON DELETE CASCADE
);
Payment Table

sql
Copy code
CREATE TABLE ecommerce_db.Payment (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    payment_method ENUM('Credit Card', 'Debit Card', 'PayPal', 'Bank Transfer'),
    payment_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    order_id INT,
    amount_paid DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id) ON DELETE SET NULL
);
Review Table

sql
Copy code
CREATE TABLE ecommerce_db.Review (
    review_id INT PRIMARY KEY AUTO_INCREMENT,
    review_text TEXT,
    rating INT CHECK (rating >= 1 AND rating <= 5),
    customer_id INT,
    product_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id) ON DELETE SET NULL,
    FOREIGN KEY (product_id) REFERENCES Product(product_id) ON DELETE SET NULL
);
Insert the Data
Customer Table

sql
Copy code
INSERT INTO ecommerce_db.Customer (first_name, last_name, email, dob, phone_number) 
VALUES 
('Alice', 'Johnson', 'alice.johnson@example.com', '1985-03-15', '1234567890'),
('Bob', 'Smith', 'bob.smith@example.com', '1990-07-22', '0987654321'),
('Charlie', 'Brown', 'charlie.brown@example.com', '1978-11-05', '1112223333');
Product Category Table

sql
Copy code
INSERT INTO ecommerce_db.ProductCategory (category_name, description)
VALUES 
('Electronics', 'Devices and gadgets including phones, laptops, and accessories'),
('Clothing', 'Men\'s and women\'s clothing and accessories'),
('Home Appliances', 'Household items including kitchen appliances, vacuums, etc.');
Supplier Table

sql
Copy code
INSERT INTO ecommerce_db.Supplier (supplier_name, contact_number, total_revenue)
VALUES 
('TechSupply Co.', '555-1234', 50000.00),
('FashionDirect', '555-5678', 30000.00),
('HomeGoods Ltd.', '555-9101', 15000.00);
Address Table

sql
Copy code
INSERT INTO ecommerce_db.Address (house_number, street_name, city, state, postal_code, country, customer_id)
VALUES 
('123', 'Elm Street', 'New York', 'NY', '10001', 'USA', 1),
('456', 'Oak Avenue', 'Los Angeles', 'CA', '90001', 'USA', 2),
('789', 'Pine Road', 'Chicago', 'IL', '60001', 'USA', 3);
Product Table

sql
Copy code
INSERT INTO ecommerce_db.Product (product_name, price, stock_quantity, brand, category_id, supplier_id)
VALUES 
('Smartphone', 699.99, 50, 'TechBrand', 1, 1),
('Laptop', 999.99, 30, 'TechBrand', 1, 1),
('T-shirt', 19.99, 100, 'FashionBrand', 2, 2),
('Microwave', 149.99, 25, 'HomeBrand', 3, 3);
ShoppingCart Table

sql
Copy code
INSERT INTO ecommerce_db.ShoppingCart (total_amount, total_items, customer_id)
VALUES 
(719.98, 2, 1),
(19.99, 1, 2),
(149.99, 1, 3);
Order Table

sql
Copy code
INSERT INTO ecommerce_db.Orders (total_amount, order_status, shipping_date, customer_id, cart_id)
VALUES 
(719.98, 'Shipped', '2024-09-01 14:00:00', 1, 1),
(19.99, 'Delivered', '2024-08-31 09:00:00', 2, 2),
(149.99, 'Processing', NULL, 3, 3);
OrderDetails Table

sql
Copy code
INSERT INTO ecommerce_db.OrderDetails (order_id, product_id, unit_price, quantity)
VALUES 
(1, 1, 699.99, 1),
(1, 2, 19.99, 1),
(2, 3, 19.99, 1),
(3, 4, 149.99, 1);
Payment Table

sql
Copy code
INSERT INTO ecommerce_db.Payment (payment_method, order_id, amount_paid)
VALUES 
('Credit Card', 1, 719.98),
('PayPal', 2, 19.99),
('Debit Card', 3, 149.99);
Review Table

sql
Copy code
INSERT INTO ecommerce_db.Review (review_text, rating, customer_id, product_id)
VALUES 
('Excellent smartphone, highly recommend!', 5, 1, 1),
'Comfortable t-shirt, good quality', 4, 2, 3),
'Microwave works well, but a bit noisy', 3, 3, 4);
