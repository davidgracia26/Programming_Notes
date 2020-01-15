Notes for the book SQL in 10 Minutes by Ben Forta

# SQL in 10 Minutes

### Lesson 1: Understanding SQL
> **Database:** A container (usually a file or set of files) to store organized data.

> **Table:** A structured list of data of a specific type.

> **Schema:** Information about database and table layout and properties.

> **Column:** A single field in a table. All tables are made up of one or more columns.

> **Datatype:** A type of allowed data. Every table column has an associated datatype that restricts (or allows) specific data in that column.

> **Row:** A record in a table.

> **Primary Key:** A column (or set of columns) whose values uniquely identify every row in a table.

### Lesson 2: Retrieving Data

> **Keyword:** A reserved word that is part of the SQL language. Never name a table or a column using a keyword.

Return One Column
```SQL
SELECT prod_name
FROM Products;
```

Return Three Columns
```SQL
SELECT prod_id, prod_name, prod_price
FROM Products;
```

Return All Columns
```SQL
SELECT *
FROM Products;
```

Return Distinct Rows
```SQL
SELECT DISTINCT vend_id
FROM Products;
```

Limit Results with MSSQL
```SQL
SELECT TOP 5 prod_name
FROM Products;
```

Inline Comments
```SQL
SELECT prod_name --this is a comment
FROM Products;
```

Multiline Comments
```SQL
/*Commented Out
Code*/
SELECT prod_name
FROM Products;
```

### Lesson 3: Sorting Retrieved Data
> **Clause:** SQL statments are made up of clauses, some required and some optional. A clause usually consists of a keyword and supplied data. An example of this is the SELECT statement's FROM clause.

Sorting Data by One Column
```SQL
SELECT prod_name
FROM Products
ORDER BY prod_name;
```

Sorting by Multiple Columns
```SQL
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price, prod_name;
```

Sorting by Column Position
```SQL
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY 2,3;
-- 2,3 references the second and third columns in the select list
```

Specifying Sort Direction
```SQL
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC

SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC, prod_name
```

### Lesson 4: Filtering Data
WHERE Clause
```SQL
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```

**Table for WHERE Clause Operators**

| Operator | Description |
| -------- | ----------- |
| =        | Equality    |
| <>       | Non-equality|
| !=       | Non-equality|
| <        | Less than   |
| <=       | Less than or equal to|
| !<       | Not less than|
| >        | Greater than|
| >=       | Greater than or equal to|
| !>       | Not greater than|
| BETWEEN  | Between two specified values|
| IS NULL  | Is a NULL value|

Checking Against a Single Value
```SQL
SELECT prod_name, prod_price
FROM Products
WHERE prod_price < 10;

SELECT prod_name, prod_price
FROM Products
WHERE prod_price <= 10;
```

Checking for Nonmatches
```SQL
SELECT vend_id, prod_name
FROM Products
WHERE vend_id <> 'DLL01';

SELECT vend_id, prod_name
FROM Products
WHERE vend_id != 'DLL01';
```

Checking for a Range of Values
```SQL
SELECT prod_name, prod_price
FROM Products
WHERE prod_price BETWEEN 5 AND 10;
```

Checking for No Value
> **Null:** No value, as opposed to a field containing 0, or an empty string, or just spaces.

```SQL
SELECT prod_name
FROM Products
WHERE prod_price IS NULL;
```

### Lesson 5: Advanced Data Filtering
> **Operator:** A special keyword used to join or change clauses within a WHERE clause. Also known as logical operators.

```SQL
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' AND prod_price <= 4;
```

> **AND:** A keyword used in a WHERE clause to specify that only rows matching all the specified conditions should be retrieved.

```SQL
SELECT prod_name, prod_price
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01';
```

> **OR:** A keyword used in a WHERE clause to specify that any rows matching either of the specified conditions should be retrieved.

```SQL
SELECT prod_name, prod_price
FROM Products
WHERE (vend_id = 'DLL01' OR vend_id = 'BRS01')
AND prod_price >= 10;
```

```SQL
SELECT prod_name, prod_price
FROM Products
WHERE vend_id in ('DLL01', 'BRS01')
ORDER BY prod_name;
```

> **IN:** A keyword used in a WHERE clause to specify a list of values to be matched using an OR comparison.

> **NOT:** A keyword used in a WHERE clause to negate a condition.

```SQL
SELECT prod_name
FROM Products
WHERE NOT vend_id = 'DLL01'
ORDER BY prod_name;

-- same as

SELECT prod_name
FROM Products
WHERE vend_id <> 'DLL01'
ORDER BY prod_name;
```

### Lesson 6: Using Wildcard Filtering

> **Wildcards:** Special characters used to match parts of a value

> **Search pattern:** A search condition made up of literal text, wildcard characters, or any combination of the above

> **Predicates:** When is an operator not an operator? When it is a "predicate". Technically, LIKE is a predicate, not an operator. The end result is the same, just be aware of this term in case you run across it in SQL documentation or manuals.

The Percent Sign (%) Wildcard - % means match any number of occurrences of any character, zero included
```SQL
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE 'Fish%';

SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '%bean bag%';
```

The Underscore Wildcard (\_)
```SQL
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '__ inch teddy bear';
-- two underscores are used and only bring back those record with two values, 8 would not match for example but 12 will
```

The Brackets Wildcard ([])
```SQL
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%'
ORDER BY cust_contact
-- Brings back names that start with J OR M. % is needed otherwise it would only look for one character names
```

```SQL
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[^JM]%'
ORDER BY cust_contact
-- Brings back names that DO NOT start with J OR M. % is needed otherwise it would only look for one character names

-- Equivalent statement as above
SELECT cust_contact
FROM Customers
WHERE NOT cust_contact LIKE '[JM]%'
ORDER BY cust_contact
```

### Lesson 7: Creating Calculated Fields

> **Field:** Essentially means the same thing as column and often used interchangeably, although database columns are typically called columns and the term fields is usually used in conjuction with calcuated fields.

> **Concatenate:** Joining values together (by appending them to each other) to form a single long value.

```SQL
SELECT vend_name + ' (' + vend_country + ')'
FROM Vendors
ORDER BY vend_name;

-- Equivalent to for Oracle

SELECT vend_name || ' (' || vend_country || ')'
FROM Vendors
ORDER BY vend_name;

-- Equivalent to for MySQL or MariaDB

SELECT Concat(vend_name, ' (', vend_country, ')')
FROM Vendors
ORDER BY vend_name;
```

Trimming
```SQL
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')'
FROM Vendors
ORDER BY vend_name;

--LTRIM() and TRIM() are also available
```

Aliases
```SQL
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')'
      AS vend_title
FROM Vendors
ORDER BY vend_name;

-- Equivalent to for Oracle

SELECT vend_name || ' (' || vend_country || ')'
      AS vend_title
FROM Vendors
ORDER BY vend_name;

-- Equivalent to for MySQL or MariaDB

SELECT Concat(vend_name, ' (', vend_country, ')')
      AS vend_title
FROM Vendors
ORDER BY vend_name;
```

Performing Mathematical Calculations
```SQL
SELECT prod_id,
       quantity,
       item_price,
       quantity*item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008;
```

SQL Mathematical Operators

| Operator | Description |
|----------|-------------|
|+|Addition|
|-|Subtraction|
|*|Multiplication|
|/|Division|

### Lesson 8: Using Data Manipulation Functions

> **Portable:** Code that is written so that it will run on multiple systems.

* Text functions are used to manipulate strings of text (for example, trimming or padding values and converting values to upper and lowercase)
* Numeric functions are used to perform mathematical operations on numeric data (for example, returning absolute numbers and performing algebraic calculations)
* Date and time functions are used to manipulate date and time values and to extract specific components from these values (for example, returning differences between dates, and checking date validity)
* System functions return information specific to the DBMS being used (for example, returning user login information)

Text Manipulation Functions

```SQL
SELECT vend_name, UPPER(vend_name) as vend_name_upcase
FROM Vendors
ORDER BY vend_name;
```

Commonly Used Text Manipulation Functions

|Function|Description|
|--------|-----------|
|LEFT() (or use substring function)| Returns characters from left of string|
|LENGTH() (also DATALENGTH()) or LEN())|Returns the length of a string|
|LOWER() (LCASE() if using Access)|Converts string to lowercase|
|LTRIM()|Trims white space from left of string|
|RIGHT() (or use substring function)|Returns characters from right of string|
|RTRIM()|Trims white space from right of string|
|SOUNDEX()|Returns a strings SOUNDEX value|
|UPPER() (UCASE() if using Access)|Converts string to uppercase|

Date and Time Manipulation Functions - VERY DB specific

```SQL
SELECT order_num
FROM Oders
WHERE DATEPART(yy, order_date) = 2012;
```

Numeric Manipulation Functions

Commonly Used Numeric Manipulation Functions

|Function|Description|
|--------|-----------|
|ABS()|Returns a number's absolute value|
|COS()|Returns the trigonometric cosine of a specified angle|
|EXP()|Returns the exponential value of a specific number|
|PI()|Returns the value of PI|
|SIN()|Returns the trigonometric sine of a specified angle|
|SQRT()|Returns the square root of a specified number|
|TAN()|Returns the trigonometric tangent of a specified angle|

### Lesson 9: Summarizing Data

> **Aggregate Functions:** Functions that operate on a set of rows to calculate and return a single value

The AVG() Function
```SQL
SELECT AVG(prod_price) as avg_price
FROM Products

SELECT AVG(prod_price) as avg_price
FROM Products
WHERE vend_id = 'DLL01';
```

The COUNT() Function
```SQL
-- * looks for null values too
SELECT COUNT(*) as num_cust
FROM Customers;

-- this query excludes null
SELECT COUNT(cust_email) as num_cust
FROM Customers;
```

The MAX() Function
```SQL
SELECT MAX(prod_price) as max_price
FROM Products;
```

The MIN() Function
```SQL
SELECT MIN(prod_price) as min_price
FROM Products;
```

The SUM() Function
```SQL
SELECT SUM(quantity) as items_ordered
FROM OrderItems
WHERE ord_num = 20005;
```

```SQL
SELECT SUM(item_price*quantity) as total_price
FROM OrderItems
WHERE order_num = 20005;
```

Aggregates on Distinct Values
```SQL
SELECT AVG(DISTINCT prod_price) as avg_price
FROM Products
WHERE vend_id = 'DLL01';
```

Combining Aggregate Functions
```SQL
SELECT COUNT(*) as num_items
      MIN(prod_price) as price_min
      MAX(prod_price) as price_max
      AVG(prod_price) as price_avg
FROM Products;
```

### Lesson 10: Grouping Data

Creating Groups

```SQL
SELECT vend_id, COUNT(*) as num_prods
FROM Products
GROUP BY vend_id;
```

Filtering Groups
```SQL
SELECT cust_id, COUNT(*) as orders
FROM Orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;
```

```SQL
SELECT vend_id, COUNT(*) as num_prods
FROM Products
WHERE prod_price >= 4
GROUP BY vend_id
HAVING COUNT(*) >= 2;
```

```SQL
SELECT ord_num, COUNT(*) as items
FROM OrderItems
GROUP BY ord_num
HAVING COUNT(*) >= 3
ORDER BY items, ord_num;
```

### Lesson 11: Working with Subqueries

> **Query:** Any SQL statement. However, the term is usually used to refer to SELECT statements.

Subquery SELECT statements can only return a single column.

```SQL
SELECT cust_name, cust_contact
FROM Customers
WHERE cust_id in (SELECT cust_id
                  FROM orders
                  WHERE order_um in (SELECT order_num
                                    FROM OrderItems
                                    WHERE prod_id = 'RGAN01'));
```

```SQL
SELECT cust_name, cust_state,
      (SELECT COUNT(*)
      FROM Orders
      WHERE Orders.cust_id = Customers.cust_id) as orders
FROM Customers
ORDER BY cust_name;
```

### Lesson 12: Joining Tables

> **Scale:** Able to handle an increasing load without failing. A well-designed database or application is said to scale well.

```SQL
SELECT vend_name, prod_name, prod_price
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id;
```

> **Cartesian Product:** The results returned by a table relationship without a join condition. The number of rows retrieved will be the number of rows in the first table multiplied by the number of rows in the second table.

Cartesian Products are returned by a *Cross join*.

```SQL
-- Returns matched rows based on no particular order. Will possibly provide incorrect data.

SELECT vend_name, prod_name, prod_price
FROM Vendors, Products
```

Inner Joins

*A join based on the testing of equality between two tables*

```SQL
SELECT vend_name, prod_name, prod_price
FROM Vendors INNER JOIN Products
ON Vendors.vend_id = Products.vend_id;
```

```SQL
SELECT vend_name, prod_name, prod_price, quantity
FROM Vendors, Products, OrderItems
WHERE Products.vend_id = Vendors.vend_id
AND OrderItems.prod_id = Products.prod_id
AND order_num = 20007;
```

```SQL
--Referring to an example in the previous chapter
SELECT cust_name, cust_contact
FROM Customers, Orders, OrderItems
WHERE Customers.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num
AND prod_id = 'RGAN01';
```

### Lesson 13: Creating Advanced Joins

Using Table Aliases
```SQL
SELECT cust_name, cust_contact
FROM Customers AS C, Orders AS O, OrderItems AS OI
WHERE C.cust_id = O.cust_id
AND OI.order_num = O.order_num
AND prod_id = 'RGAN01';
```

Self Joins

```SQL
SELECT cust_id, cust_name, cust_contact
FROM Customers
WHERE cust_name = (SELECT cust_name
                   FROM Customers
                   WHERE cust_contact = 'Jim Jones');
-- Becomes
SELECT c1.cust_id, c1.cust_name, c1.cust_contact
FROM Customers AS c1, Customers AS c2
WHERE c1.cust_name = c2.cust_name
AND c2.cust_contact = 'Jim Jones';
```

Natural Joins

*A join that removes duplicate columns*

```SQL
SELECT C.*, O.order_num, O.order_date, OI.prod_id, OI.quantity, OI.item_price
FROM Customers AS C, Orders AS O, OrderItems AS OI
WHERE C.cust_id = O.cust_id
AND OI.order_num = O.order_num
AND prod_id = 'RGAN01';
```

Outer Join

*A join that includes table rows that have no associated rows in the related table*

```SQL
--Retrieves a list of all customers and their orders
SELECT Customers.cust_id, Orders.order_num
FROM Customers INNER JOIN Orders
ON Customers.cust_id = Orders.cust_id;
```

```SQL
--Retrieves a list of all customers including those who have no orders
SELECT Customers.cust_id, Orders.order_num
FROM Customers LEFT OUTER JOIN Orders
ON Customers.cust_id = Orders.cust_id;

-- equivalent
SELECT Customers.cust_id, Orders.order_num
FROM Orders RIGHT OUTER JOIN Customers
ON Customers.cust_id = Orders.cust_id;
```

```SQL
-- Includes unrelated rows from both tables
SELECT Customers.cust_id, Orders.order_num
FROM Customers FULL OUTER JOIN Orders
ON Customers.cust_id = Orders.cust_id;
```

```SQL
SELECT Customers.cust_id,
       COUNT(Orders.order_num) AS num_ord
FROM Customers INNER JOIN Orders
ON Customers.cust_id = Orders.cust_id
GROUP BY Customers.cust_id;
```

```SQL
SELECT Customers.cust_id,
       COUNT(Orders.order_num) AS num_ord
FROM Customers LEFT OUTER JOIN Orders
ON Customers.cust_id = Orders.cust_id
GROUP BY Customers.cust_id;
```

### Lesson 14: Combining Queries

Combined Queries/Unions

A statement that is a combined query can also be done using where conditions.

```SQL
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL', 'IN', 'MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';

--Equivalent
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL', 'IN', 'MI')
OR cust_name = 'Fun4All';

--Union removes duplicates
```

```SQL
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL', 'IN', 'MI')
UNION ALL
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';

--Union ALL keeps duplicates
--WHERE clause version of this statement cannot obtain the duplicate records
```

```SQL
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IL', 'IN', 'MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All'
ORDER BY cust_name, cust_contact;
```

UNION EXCEPT and INTERSECT exist but joins are better at doing those actions.

### Lesson 15: Inserting Data

Inserting Complete Rows

```SQL
INSERT INTO Customers
VALUES ('1000000006',
        'Toy Land',
        '123 Any Street',
        'New York',
        'NY',
        '11111',
        'USA',
        NULL,
        NULL);
-- Very unsafe way to do things. Column order should not be assumed.
```

```SQL
INSERT INTO Customers(cust_id,
                      cust_name,
                      cust_address,
                      cust_city,
                      cust_state,
                      cust_zip,
                      cust_country,
                      cust_contact,
                      cust_email)
VALUES ('1000000006',
        'Toy Land',
        '123 Any Street',
        'New York',
        'NY',
        '11111',
        'USA',
        NULL,
        NULL);
-- Safer way to add to a table.
```

Inserting Partial Rows

```SQL
INSERT INTO Customers(cust_id,
                      cust_name,
                      cust_address,
                      cust_city,
                      cust_state,
                      cust_zip,
                      cust_country)
VALUES ('1000000006',
        'Toy Land',
        '123 Any Street',
        'New York',
        'NY',
        '11111',
        'USA');
-- The nulls are omitted.
```

Inserting Retrieved Data

```SQL
INSERT INTO Customers(cust_id,
                      cust_name,
                      cust_address,
                      cust_city,
                      cust_state,
                      cust_zip,
                      cust_country)
SELECT (cust_id,
        cust_name,
        cust_address,
        cust_city,
        cust_state,
        cust_zip,
        cust_country)
FROM CustNew;
```

Copying From One Table to Another

```SQL
SELECT *
INTO CustCopy
FROM Customers;

-- Equivalent to
CREATE TABLE CustCopy AS
SELECT * FROM Customers;
```

### Lesson 16: Updating and Deleting Data

Updating Data

```SQL
UPDATE Customers
SET cust_email = 'kim@thetoystore.com'
WHERE cust_id = '1000000005';
```

```SQL
UPDATE Customers
SET cust_contact = 'Sam Roberts',
    cust_email = 'sam@toyland.com'
WHERE cust_id = '1000000006';
```

```SQL
UPDATE Customers
SET cust_email = NULL
WHERE cust_id = '100000005';
```

Deleting Data

```SQL
DELETE FROM Customers
WHERE cust_id = '100000006';
```

If you want to quickly delete all data from a table use the TRUNCATE TABLE command instead of dropping the WHERE from a DELETE statement.

### Lesson 17: Creating and Manipulating Tables

```SQL
CREATE TABLE Products 
(
      prod_id     CHAR(10)          NOT NULL,
      vend_id     CHAR(10)          NOT NULL,
      prod_name   CHAR(254)         NOT NULL,
      prod_price  DECIMAL(8,2)      NOT NULL,
      prod_desc   VARCHAR(1000)     NULL 
);
```

```SQL
CREATE TABLE Orders
(
      order_num   INTEGER     NOT NULL,
      order_date  DATETIME    NOT NULL,
      cust_id     CHAR(10)    NOT NULL
)
```

```SQL
CREATE TABLE Vendors
(
      vend_id     CHAR(10)    NOT NULL,
      vend_name   CHAR(50)    NOT NULL,
      vend_address CHAR(50)           ,
      vend_city   CHAR(50)            ,
      vend_state  CHAR(5)             ,
      vend_zip    CHAR(10)            ,
      vend_country CHAR(50)           
);
```

Specifying Default Values

```SQL
CREATE TABLE OrderItems
(
      order_num   INTEGER     NOT NULL,
      order_item  INTEGER     NOT NULL,
      prod_id     CHAR(10)    NOT NULL,
      quantity    INTEGER     NOT NULL    DEFAULT 1,
      item_price  DECIMAL(8,2) NOT NULL
);
```

Updating Tables

```SQL
ALTER TABLE Vendors
ADD vend_phone CHAR(20);
```

```SQL
ALTER TABLE Vendors
DROP COLUMN vend_phone;
```

Deleting Tables

```SQL
DROP TABLE CustCopy;
```

### Lesson 18: Using Views

```SQL
SELECT cust_name, cust_contact
FROM Customers, Orders, OrderItems
WHERE Customers.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num
AND prod_id = 'RGAN01';

-- With a view named ProductCustomers
SELECT cust_name, cust_contact
FROM ProductCustomers
AND prod_id = 'RGAN01';
```

To remove a view the DROP statement is used. The syntax is DROP VIEW viewname. To overwrite (or update) a view you must first DROP it and then recreate it.

Using Views to Simplify Complex Joins

```SQL
CREATE VIEW ProductCustomers AS
SELECT cust_name, cust_contact, prod_id
FROM Customers, Orders, OrderItems
WHERE Customers.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num
```

Using Views to Reformat Retrieved Data

```SQL
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')' AS vend_title
FROM Vendors
ORDER BY vend_name;
```

```SQL
CREATE VIEW VendorLocations AS
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')' AS vend_title
FROM Vendors;
```

```SQL
SELECT *
FROM VendorLocations;
```

Using Views to Filter Unwanted Data

```SQL
CREATE VIEW CustomerEMailList AS
SELECT cust_id, cust_name, cust_email
FROM Customers
WHERE cust_email IS NOT NULL;
```

```SQL
SELECT *
FROM CustomerEMailList;
```

Using Views With Calculated Fields

```SQL
SELECT prod_id, quantity, item_price, quantity*item_price as expanded_price
FROM OrderItems
WHERE order_num = 20008;
```

```SQL
CREATE VIEW OrderItemsExpanded AS
SELECT prod_id, quantity, item_price, quantity*item_price as expanded_price
FROM OrderItems;
```

```SQL
SELECT *
FROM OrderItemsExpanded
WHERE order_num = 20008;
```

### Lesson 19: Working With Stored Procedures

Stored Procedures are collections of one or more SQL statements saved for future use.

The three benefits of them are simplicity, security, and performance.

Executing Stored Procedures

```SQL
-- AddNewProduct is not defined yet
EXECUTE AddNewProduct('JTS01',
                      'Stuffed Eiffel Tower',
                      6.49,
                      'Plush stuffed toy with the text La Tour Eiffel in red white and blue')
```

Creating Stored Procedures

```SQL
CREATE PROCEDURE MailingListCount
AS
DECLARE @cnt INTEGER
SELECT @cnt = COUNT(*)
FROM Customers
WHERE NOT cust_email IS NULL
RETURN @cnt;
```

```SQL
DECLARE @ReturnValue INT
EXECUTE @ReturnValue=MailingListCount;
SELECT @ReturnValue;
```

```SQL
CREATE PROCEDURE NewOrder @cust_id CHAR(10)
AS
-- Declare variable for order number
DECLARE @order_num INTEGER
-- Get current highest order number
SELECT @order_num=MAX(order_num)
FROM Orders
-- Determine next order number
SELECT @order_num=@order_num+1
-- Insert new order
INSERT INTO Orders(order_num, order_date, cust_id)
VALUES(@order_num, GETDATE(), @cust_id)
-- Return order number
RETURN @order_num;
```
Different version of the same code
```SQL
CREATE PROCEDURE NewOrder @cust_id CHAR(10)
AS
-- Insert new order
INSERT INTO Orders(cust_id)
VALUES(@cust_id)
-- Return order number
SELECT order_num = @@IDENTITY;
```

### Lesson 20: Managing Transaction Processing

> **Transaction:** A block of SQL statements

> **Rollback:** The process of undoing specified SQL statements

> **Commit:** Writing unsaved SQL statements to the database tables

> **Savepoint:** A temporary placeholder in a transaction set to which you can issue a rollback (as opposed to rolling back an entire transaction)

Transaction processing is used to manage INSERT, UPDATE, and DELETE statements. You cannot rollback CREATE or DROP operations. These statements may be used in a transaction block, but if you perform a rollback they will not be undone.

```SQL
BEGIN TRANSACTION
...
COMMIT TRANSACTION

-- and SQL between these statements must be executed entirely or not at all
```

```SQL
-- Done within a transaction block
DELETE FROM Orders;
ROLLBACK;
```

```SQL
BEGIN TRANSACTION
DELETE OrderItems WHERE order_num = 12345
DELETE Orders WHERE order_num = 12345
COMMIT TRANSACTION
```

Savepoints

```SQL
SAVE TRANSACTION delete1;
```

```SQL
ROLLBACK TRANSACTION delete1;
```

Full Example
```SQL
BEGIN TRANSACTION
INSERT INTO Customers(cust_id, cust_name)
VALUES('10000000010', 'Toys Emporium')
SAVE TRANSACTION StartOrder;
INSERT INTO Orders(order_num, order_date, cust_id)
VALUES(20100, '2001/12/1', '10000000010');
IF @@ERROR <> 0 ROLLBACK TRANSACTION StartOrder;
INSERT INTO OrderItems(order_num, order_item, prod_id, quantity, item_price)
VALUES(20100, 1, 'BR01', 100, 5.49);
IF @@ERROR <> 0 ROLLBACK TRANSACTION StartOrder;
INSERT INTO OrderItems(order_num, order_item, prod_id, quantity, item_price)
VALUES(20100, 2, 'BR03', 100, 10.99);
IF @@ERROR <> 0 ROLLBACK TRANSACTION StartOrder;
COMMIT TRANSACTION
```

### Lesson 21: Using Cursors

> **Result Set:** The results retrieved by a SQL query

A cursor is a database query stored on the DBMS server - not a SELECT statement, but the result set retrieved by that statement. Once the cursor is stored, applications can scroll or browse up and down the data as needed.

They are used primarily by interactive applications in which users need to scroll up and down through screens of data, browsing or making changes.

Creating Cursors

```SQL
DECLARE CustCursor CURSOR
FOR
SELECT * FROM Customers
WHERE cust_email IS NULL;
```

Using Cursors

```SQL
OPEN CURSOR CustCursor;
```

```SQL
DECLARE @cust_id CHAR(10),
        @cust_name CHAR(50),
        @cust_address CHAR(50),
        @cust_city CHAR(50),
        @cust_state CHAR(5),
        @cust_zip CHAR(10),
        @cust_country CHAR(50),
        @cust_contact CHAR(50),
        @cust_email CHAR(255)
OPEN CustCursor
FETCH NEXT FROM CustCursor
      INTO @cust_id, @cust_name, @cust_address, @cust_city, @cust_state, @cust_zip, @cust_country, @cust_country, @cust_contact, @cust_email
-- Insert own implementation
WHILE @@FETCH_STATUS = 0
BEGIN

FETCH NEXT FROM CustCursor
      INTO @cust_id, @cust_name, @cust_address, @cust_city, @cust_state, @cust_zip, @cust_country, @cust_country, @cust_contact, @cust_email
END
CLOSE CustCursor
```

Closing Cursors

```SQL
CLOSE CustCursor
DEALLOCATE CURSOR CustCursor;
```

### Lesson 22: Understanding Advanced SQL Features

> **Constraints:** Rules govern how database data is inserted or manipulated.

A Primary Key is a special kind of constraint used to ensure that values in a column (or a set of columns) are unique and never change, in other words, a column (or columns) in a table whose values uniquely identify each row in the table.

```SQL
CREATE TABLE Vendors
(
      vend_id CHAR(10) PRIMARY KEY,
      -- other implementation
);
```

```SQL
ALTER TABLE Vendors
ADD CONSTRAINT PRIMARY KEY (vend_id)
```

A Foreign Key is a column in a table whose values must be listed in a primary key in another table. 

```SQL
CREATE TABLE Orders
(
      order_num INTEGER NOT NULL PRIMARY KEY,
      order_date DATETIME NOT NULL,
      cust_id     CHAR(10) REFERENCES Customers(cust_id)
);
```

```SQL
ALTER TABLE Orders
ADD CONSTRAINT
FOREIGN KEY (cust_id) REFERENCES Customers (cust_id)
```

Unique Constraints are used to ensure all data in a column (or set of columns) is unique.

*  A table can contain multiple unique constraints, but only one primary key is allowed per table.
* Unique constraint columns can contain NULL values
* Unique constraint columns can be modified or updated
* Unique constraint column values can be reused
* Unlike primary keys, unique constraints cannot be used to define foreign keys

The UNIQUE keyword can be used to define these constraints.

Check Constraints are used to ensure that data in a column (or set of columns) meets a set of criteria that you specify.

* Checking min or max values
* Specifying ranges
* Allowing only specific values

```SQL
CREATE TABLE OrderItems
(
      order_num INTEGER NOT NULL,
      quantity INTEGER NOT NULL CHECK (quantity > 0)
);
```

```SQL
ADD CONSTRAINT CHECK (gender LIKE '[MF]')
```

Understanding Indexes

Indexes are made up of one or more columns in a database and it helps you search quicker much like the index of a book.

* Indexes improve the performance of retrieval operations but degrade the performance of insertion, modification, and deletion.
* Index data can take up lots of storage space
* Not all data is suitable for indexing. Must be many unique values
* Indexes are used for data filtering and data sorting
* Multiple columns can be defined in an index. When searching for these multiple columns, the speed improves but using each individual column that is a part of the index will not see speed improvements.

```SQL
CREATE INDEX prod_name_ind
ON Products(prod_name);
```

Understanding Triggers

Triggers are special stored procedures that are executed automatically when specific database activiy occurs. They are tied to individual tables.

Within triggers, your code has access to the following:
* all new data in INSERT operations
* all new data and old date in UPDATE operations
* deleted data in DELETE operations

Common uses:
* ensuring data consistency
* performing actions on other tables based on changes to a table
* performing additional validation and rolling back data if needed
* calculating computed column values or updating timestamps

```SQL
CREATE TRIGGER customer_state
ON Customers
FOR INSERT, UPDATE
AS
UPDATE Customers
SET cust_state = Upper(cust_state)
WHERE Customers.cust_id = inserted.cust_id;
```

Constraints are faster than triggers.

Database Security

Some operations that are often secured:

* access to database administration features (creating, altering, dropping tables...)
* access to specific databases or tables
* the type of access (read-only, access to specific columns...)
* access to tables via views or stored procedures only
* creation of multiple levels of security, thus allowing varying degrees of access and control based on login
* restricting the ability to manage user accounts

Security is managed through the GRANT and REVOKE statements.

