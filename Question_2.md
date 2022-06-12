# Question 2

For this question you’ll need to use SQL. [Follow this
link](https://www.w3schools.com/SQL/TRYSQL.ASP?FILENAME=TRYSQL_SELECT_ALL)
to access the data set required for the challenge. Please use queries to
answer the following questions. Paste your queries along with your final
numerical answers below.

------------------------------------------------------------------------

## a. How many orders were shipped by Speedy Express in total?

First step: 
Join tables Orders and Shippers to create a table with the order ID and the Shipper name of each order

Second step:
Count the Orders associated to the Shipper “Speedy Express”

Result: **Speedy Express had 54 Orders**

Query:


	WITH OrdersShipper AS
	(SELECT Orders.OrderID,
	        Shippers.ShipperName
	FROM [Orders]
	INNER JOIN [Shippers] ON Orders.ShipperID = Shippers.ShipperID)
	SELECT ShipperName, 
	       COUNT (OrderID) AS TotalOrders
	FROM OrdersShipper
	WHERE ShipperName = 'Speedy Express'




------------------------------------------------------------------------

## b. What is the last name of the employee with the most orders?

We need to get the information of how many orders each employee has made, naming the employee by their last name.

Tables Orders and Employees can be used to relate the info of the employee last name to each order. Then, we can group the orders by employee and count them. Finally find the last name of the employee with the maximum number of orders.

First Step:
Create a common table expression (CTE) by joining tables Orders and Shippers to get the order ID and the Shipper name of each order.

Second Step:
Group the orders by Employee and count them.

Third Step:
Find the employee with the maximum number of orders

Result: **Peacock** 

Query:

	WITH OrdersEmployees AS
	(SELECT Orders.OrderID,
        	Employees.LastName,
        	Employees.EmployeeID
	FROM [Orders]
	INNER JOIN [Employees] ON Orders.EmployeeID = Employees.EmployeeID),
	OrdersPerEmployee AS
	(SELECT LastName,
	        EmployeeID,
    	COUNT (OrderID) AS NumerOfOrders
	FROM OrdersEmployees
	GROUP BY EmployeeID)
    	
	SELECT LastName,
    	       MAX(NumerOfOrders) AS TotalNumberOfOrders
    	FROM OrdersPerEmployee

------------------------------------------------------------------------

## c. What product was ordered the most by customers in Germany?

First Step:
Tables Customers and Orders can be used to relate the order ID to the Country the order was made by joining them on the Customer ID. 

Second Step
Use the prior CTE and OrdersDetails table to get the Product ID and the quantity of products ordered from each country and filter only the ones ordered in Germany.

Third Step:
From the Table Products is possible to get the products name and relate it to the quantity ordered using the Product ID. Finally the product with Maximum quantity can be selected.

RESULT:  **Boston Crab Meat**

Query:


	WITH CustomersOrders AS
	(SELECT Customers.CustomerId,
    		Customers.Country As CustomerCountry,
      		Orders.OrderID
	FROM Customers
	INNER JOIN Orders ON Customers.CustomerId = Orders.CustomerID),
    
	ProductsQuantity AS
	(SELECT CustomersOrders.CustomerCountry,
	        OrderDetails.ProductID,
       	OrderDetails.Quantity,
       	SUM(Quantity) AS TotalProductsGermany
	FROM CustomersOrders
	INNER JOIN OrderDetails ON CustomersOrders.OrderID = OrderDetails.OrderID
	WHERE CustomerCountry = "Germany"
	GROUP BY ProductID)
        
	SELECT Products.ProductName,
	       MAX(TotalProductsGermany)
	FROM Products
	INNER JOIN ProductsQuantity ON Products.ProductId = ProductsQuantity.ProductID






------------------------------------------------------------------------
