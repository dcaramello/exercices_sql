# Exercices de programmation SQL

Vous trouverez dans ce document une série d'exercices qui vont vous obliger à réaliser des requêtes sql de plus en plus complexes.

Pour réaliser ces exercices, vous utiliserez une base de données libre, mise à disposition par la w3school sur laquelle vous pouvez réaliser des requêtes pour vous entraîner.

[Aller à la base de données](https://www.w3schools.com/sql/trysql.asp?filename=trysql_asc)

Le contexte : la base de données est celle d'une entreprise de vente de produits. Vous avez été engagé comme développeur pour réaliser un espace d'administration complet permettant une gestion et une visualisation aisée de la base de données de l'entreprise. Pour ce faire vous aller devoir réaliser un ensemble de requêtes.

## Exercices 1

Ecrivez les requêtes pour :
* Afficher le contenu de la table Employees
        SELECT * FROM employess

* Afficher le contenu de la table Products
        SELECT * FROM Products

* Afficher le contenu de la table OrderDetails
        SELECT * FROM OrderDetails


## Exercices 2
Ecrivez les requêtes pour :
* Ajouter un client dans la table Customers
        INSERT INTO Customers(CustomerName, ContactName, Address, City, PostalCode, Country)
        VALUES ('Die Hard', 'Brice Willous', 'Nakatomi Plaza', 'Los angeles', '90000', 'USA')

* Ajouter un produit dans la table Products
        INSERT INTO products (ProductName, SupplierID, CategoryID, Unit, Price)
        VALUES ('chef Bob', '5', '1', '10 boxes x 5 bags', '20')

* Supprimer le client ajouté dans table Customers
        DELETE FROM Customers
        WHERE CustomerName = 'Die Hard'

* Supprimer le produit ajouté dans la table Products
        DELETE FROM Products
        WHERE ProductName  = 'chef Bob'

* Modifier les détails de la commande 10252, id des détails 14 dans la table OrderDetails pour passer sa quantité de 40 à 20
        UPDATE OrderDetails
        SET Quantity = 20
        WHERE OrderDetailID = 14

* Modifier le nom, la ville et le pays du client d'id 50
        UPDATE Customers
        SET CustomerName = 'Alfonse Brown', City = 'Le Havre', Country = 'France'
        WHERE CustomerID = 50

## Exercices 3

Ecrivez les requêtes pour :
* Afficher tous les clients français
        SELECT * FROM Customers
        WHERE Country = 'France'

* Afficher tous les clients français vivant à Paris
        SELECT * FROM Customers
        WHERE Country = 'France'AND City = 'Paris'

* Afficher tous les clients français vivant à Paris ou Lille
        SELECT * FROM Customers
        WHERE Country = 'France' AND City = 'Paris' OR City ='Lille'

* Afficher uniquement le nom, l'unité et le prix des produits de la catégorie boissons et du fournisseur Bigfoot Breweries
        SELECT ProductName, Unit, Price FROM Products
        WHERE CategoryID = 1 AND SupplierID = 16

## Exercices 4
Ecrivez les requêtes pour :
* Afficher le nom et le prénom des 3 premiers employés
        SELECT *
        FROM Employees LIMIT 0, 3

* Afficher le nom et le prénom des 3 derniers employés
        SELECT * FROM Employees
        ORDER BY EmployeeID DESC
        LIMIT 0, 3

* Afficher le nom des produits et leur prix du plus cher au moins cher pour tous ceux dont le prix est compris entre 10 et 30 euros
        SELECT ProductName, Price
        FROM Products
        ORDER BY Price AND Price <= 10 OR Price >=30

* Afficher le nom, le fournisseur, la catégorie et le prix de tous les produits qui ont les fournisseurs suivants : Exotic Liquid, Specialty Biscuits, Ltd , Plutzer Lebensmittelgroßmärkte AG et les catégories suivantes : Boissons et Confection, en les classant du moins cher au plus cher.
        SELECT ProductName, SupplierID, CategoryID, Price
        FROM Products
        WHERE SupplierID IN(1, 8, 12) AND CategoryID IN(1, 3)
        ORDER BY Price DESC

## Exercices 5
Ecrivez les requêtes pour :
* Afficher le nom et la description des catégories en majuscules
        SELECT UPPER(CategoryName), UPPER(Description)
        FROM Categories

* Afficher le nom et la description des catégories en majuscules en leur donnant pour allias "NameToUpper" et "DescriptionToUpper"
        SELECT UPPER(CategoryName)
                AS NameToUpper,
              UPPER(Description)
                AS DescriptionToUpper
        FROM Categories

* Afficher tous les clients dont la longueur du nom est comprise entre 20 et 30 caractères.
        SELECT * FROM Customers
        WHERE LENGTH(CustomerName)
        BETWEEN 20 AND 30

* Afficher le prix moyen des produits avec pour allias "PrixMoyen"
        SELECT AVG(Price) AS PrixMoyen FROM Products

* Afficher le prix moyen des produits arrondi à 2 décimales
        SELECT ROUND(AVG(Price), 2) AS PrixMoyen FROM Products

* Afficher le prix moyen des produits de la catégorie condiments
        SELECT AVG(Price) FROM Products
        WHERE CategoryID = 2

* Afficher le nombre de commandes passées par le client portant l'id 25
        SELECT COUNT(*) FROM Orders
        WHERE CustomerID = 25

## Exercice 6 (jointures)
Ecrivez les requêtes pour :
* Obtenir depuis la table Orders l'id et la date de la commande ainsi que le nom du client et son pays d'origine
        SELECT OrderId, OrderDate, CustomerName, Country
        FROM Orders
        INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID

* Afficher en plus des informations précédentes le nom et le prénom de l'employé ayant traité la commande
        SELECT OrderId, OrderDate, CustomerName, Country, LastName, FirstName
        FROM Orders
        INNER JOIN Customers
          ON Orders.CustomerID = Customers.CustomerID
        INNER JOIN Employees
          ON Orders.EmployeeID = Employees.EmployeeID

* Afficher en plus des informations précédentes la quantité de produit commandés, le nom du produit et le prix à l'unité du produit.
        SELECT ord.OrderId, ord.OrderDate, CustomerName, Country, LastName, FirstName,Quantity, ProductName, Price, Unit
        FROM Orders as ord
        INNER JOIN Customers
        ON ord.CustomerID = Customers.CustomerID
        INNER JOIN Employees
        ON ord.EmployeeID = Employees.EmployeeID
        INNER JOIN OrderDetails
        ON ord.OrderID = OrderDetails.OrderID
        INNER JOIN Products
        ON OrderDetails.ProductID = Products.ProductID

* Afficher en plus des informations précédentes une colonne "Amount" contenant le prix total de la commande arrondi à deux décimales
        SELECT ord.OrderId, ord.OrderDate, CustomerName, Country, LastName, FirstName,Quantity, ProductName, Price, Unit, Quantity * Price AS Amout
        FROM Orders as ord
        INNER JOIN Customers
        ON ord.CustomerID = Customers.CustomerID
        INNER JOIN Employees
        ON ord.EmployeeID = Employees.EmployeeID
        INNER JOIN OrderDetails
        ON ord.OrderID = OrderDetails.OrderID
        INNER JOIN Products
        ON OrderDetails.ProductID = Products.ProductID
