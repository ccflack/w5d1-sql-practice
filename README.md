# w5d1-sql-practice

*EXPLORER MODE*

1. How many users are there?

  `SELECT COUNT (*) FROM users;`

  50

2. What are the 5 most expensive items?

  `SELECT title, price FROM items ORDER BY price DESC LIMIT 5;`

  title|price
  Small Cotton Gloves|9984
  Small Wooden Computer|9859
  Awesome Granite Pants|9790
  Sleek Wooden Hat|9390
  Ergonomic Steel Car|9341

3. What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)

  `SELECT title, price FROM items WHERE category = "books" ORDER BY price LIMIT 1;`

  title|price
  Ergonomic Granite Chair|1496

  `SELECT title, price FROM items WHERE category LIKE "%books%" ORDER BY price LIMIT 1;`

  title|price
  Ergonomic Granite Chair|1496

4. Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?

  `SELECT users.first_name, users.last_name FROM users INNER JOIN addresses ON (users.id = addresses.user_id) WHERE addresses.street = '6439 Zetta Hills';`

  first_name|last_name
  Corrine|Little

  `SELECT street, city, state, zip FROM addresses INNER JOIN users ON (addresses.user_id = users.id) WHERE users.first_name = 'Corrine' AND users.last_name = 'Little'`

  street|city|state|zip
  6439 Zetta Hills|Willmouth|WY|15029
  54369 Wolff Forges|Lake Bryon|CA|31587

5. Correct Virginie Mitchell's address to "New York, NY, 10108".

  `UPDATE addresses SET city = 'New York', state = 'NY', zip = '10108' WHERE user_id IN (SELECT id FROM users WHERE users.first_name = 'Virginie' AND users.last_name = 'Mitchell');`

6. How much would it cost to buy one of each tool?

  `SELECT SUM(price) FROM items WHERE category LIKE "%tool%";`

  SUM(price)
  46477

7. How many total items did we sell?

  `SELECT SUM(quantity) FROM orders;`
  SUM(quantity)
  2125


8. How much was spent on books?

  `SELECT SUM(orders.quantity * items.price) FROM orders INNER JOIN items ON (orders.item_id = items.id) WHERE items.category LIKE '%book%';`
  SUM(orders.quantity * items.price)
  1081352

9. Simulate buying an item by inserting a User for yourself and an Order for that User.

  `INSERT INTO users (first_name, last_name, email) VALUES ("Chris", "Flack", "ccflack@me.com");`
  `SELECT id, first_name, last_name, email FROM users WHERE email = "ccflack@me.com";`

  id|first_name|last_name|email
  51|Chris|Flack|ccflack@me.com

  `INSERT INTO orders (user_id, item_id, quantity, created_at) VALUES (51, 28, 2, CURRENT_TIMESTAMP);`
  `SELECT * FROM orders WHERE user_id = 51;`

  id|user_id|item_id|quantity|created_at
  378|51|28|2|2016-10-24 19:33:06

*ADVENTURE MODE*

1. What item was ordered most often? Grossed the most money?

  `SELECT title, SUM(orders.quantity) FROM items INNER JOIN orders ON (items.id = orders.item_id) GROUP BY title ORDER BY SUM(orders.quantity) DESC LIMIT 1;`

  title|SUM(orders.quantity)
  Incredible Granite Car|72

  `SELECT items.title, SUM(items.price * orders.quantity) FROM items INNER JOIN orders ON (items.id = orders.item_id) GROUP BY title ORDER BY SUM(items.price * orders.quantity) DESC LIMIT 1;`

  title|SUM(items.price * orders.quantity)
  Incredible Granite Car|525240

2. What user spent the most?

  `SELECT first_name, last_name, SUM(items.price * orders.quantity) FROM users INNER JOIN orders ON (users.id = orders.user_id) INNER JOIN items ON (orders.item_id = items.id) GROUP BY users.id ORDER BY SUM(items.price * orders.quantity) DESC LIMIT 1;`

  first_name|last_name|SUM(items.price * orders.quantity)
  Hassan|Runte|639386

3. What were the top 3 highest grossing categories?

   `SELECT items.category, SUM(items.price * orders.quantity) FROM items INNER JOIN orders ON (items.id = orders.item_id) GROUP BY items.category ORDER BY SUM(items.price * orders.quantity) DESC LIMIT 3;`

  category|SUM(items.price * orders.quantity)
  Music, Sports & Clothing|525240
  Beauty, Toys & Sports|449496
  Sports|448410

*EPIC MODE*

![alt tag](https://imgur.com/a/mfTqF)
