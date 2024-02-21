Creating a clothing store, specializing in eco-friendly and sustainable fashion. To organize the inventory and offer essential information to customers, I've designed a table that stores details about each clothing item. This table includes the following columns:

* [x] ItemID (unique identifier for each clothing item)
* [x] Name (name of the clothing item)
* [x] Category (such as shirts, pants, dresses, etc.)
* [x] Size (available sizes for each item)
* [x] Price (cost of the clothing item)

My store will initially feature a selection of 15 items, covering a wide range of categories to cater to different tastes and needs, emphasizing sustainability and ethical manufacturing processes.

## SQL Table Creation and Insertion

First, let's create the table:

```sql
CREATE TABLE ClothingItems (
    ItemID INT PRIMARY KEY,
    Name VARCHAR(255),
    Category VARCHAR(100),
    Size VARCHAR(50),
    Price DECIMAL(5,2)
);
```

Next, I'll populate the ClothingItems table with 15 entries:

```sql
Copy code
INSERT INTO ClothingItems (ItemID, Name, Category, Size, Price) VALUES
(1, 'Eco-Friendly Cotton Tee', 'Shirts', 'M, L, XL', 25.00),
(2, 'Sustainable Wool Sweater', 'Sweaters', 'S, M, L', 60.00),
(3, 'Organic Linen Dress', 'Dresses', 'S, M', 45.00),
(4, 'Recycled Polyester Jacket', 'Jackets', 'M, L', 80.00),
(5, 'Bamboo Fabric Scarf', 'Accessories', 'One Size', 20.00),
(6, 'Hemp Denim Jeans', 'Pants', '30, 32, 34', 70.00),
(7, 'Modal Fabric Blouse', 'Shirts', 'S, M, L', 40.00),
(8, 'Lyocell Midi Skirt', 'Skirts', 'M, L', 50.00),
(9, 'Alpaca Wool Beanie', 'Accessories', 'One Size', 35.00),
(10, 'Organic Cotton Socks', 'Accessories', 'One Size', 15.00),
(11, 'Vegan Leather Belt', 'Accessories', 'S, M, L', 30.00),
(12, 'Tencel Lounge Pants', 'Pants', 'S, M, L, XL', 55.00),
(13, 'Recycled Cashmere Gloves', 'Accessories', 'One Size', 25.00),
(14, 'Biodegradable Raincoat', 'Jackets', 'M, L, XL', 95.00),
(15, 'Soy Fabric Tank Top', 'Shirts', 'S, M, L', 30.00);
```

## SQL Select Statements

To sort the clothing items by price in ascending order:

```sql
Copy code
SELECT * FROM ClothingItems ORDER BY Price ASC;
```

And to find out the average price of the clothing items in my store:

```sql
Copy code
SELECT AVG(Price) AS AveragePrice FROM ClothingItems;
```

These SQL statements enable me to manage the inventory of my eco-friendly clothing store efficiently and offer customers transparent information, such as organizing items based on price or showcasing the affordability of sustainable fashion.



