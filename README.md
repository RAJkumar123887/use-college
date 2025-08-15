# use-college
college
use college;

-- Customers
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name TEXT,
    City TEXT
);

-- Orders
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    Product TEXT,
    OrderDate DATE,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

-- scalr subquery
SELECT 
    Name,
    (SELECT COUNT(*) 
     FROM Orders 
     WHERE Orders.CustomerID = Customers.CustomerID) AS TotalOrders
FROM Customers;

-- subquery in where

SELECT Name
FROM Customers
WHERE CustomerID IN (SELECT CustomerID FROM Orders);

-- subquery in where(exists)
SELECT Name
FROM Customers
WHERE EXISTS (
    SELECT 1 FROM Orders 
    WHERE Orders.CustomerID = Customers.CustomerID
);

-- subquery in where
SELECT c.Name, o.OrderCount
FROM Customers c
JOIN (
    SELECT CustomerID, COUNT(*) AS OrderCount
    FROM Orders
    GROUP BY CustomerID
) o ON c.CustomerID = o.CustomerID;

-- filter customers with more than one order
SELECT Name
FROM Customers
WHERE CustomerID IN (
    SELECT CustomerID
    FROM Orders
    GROUP BY CustomerID
    HAVING COUNT(*) > 1
);
