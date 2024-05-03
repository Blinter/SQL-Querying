# Part I
### SQLZoo Progress

    + (Part II Tutorial 5)
    + Further Study: More on Subqueries (Tutorial 4)
  
```
https://sqlzoo.net/progress/960846961797
```
---
# Part II
**SQL Basics: Simple WHERE and ORDER BY**
```sql
SELECT * FROM people WHERE age>50 ORDER BY age DESC;
```
---
**SQL Basics: Simple SUM**
```sql
SELECT SUM(age) AS age_sum FROM people;
```
---
**SQL Basics: Simple MIN / MAX**
```sql
SELECT MIN(age) as age_min, MAX(age) as age_max FROM people;
```
---
**Find all active students**
```sql
SELECT IsActive FROM students WHERE IsActive;
```
**SQL Basics: Simple GROUP BY**
```sql
SELECT age,COUNT(age) AS people_count FROM people GROUP BY age;
```
---
**SQL Basics: Simple HAVING**
```sql
SELECT age, COUNT(*) AS total_people FROM people GROUP BY age HAVING COUNT(*)>=10;
```
---
# Part III
# queries_products.sql
---
## 1, 2, 3)
+ Add a product to the table with the name of "chair", 
  + price of 44.00, and can_be_returned of false.

+ Add a product to the table with the name of "stool", 
  + price of 25.99, and can_be_returned of true.

+ Add a product to the table with the name of "table", price of 124.00, 
  + and can_be_returned of false.

```sql
INSERT INTO
    products (name, price, can_be_returned)
VALUES
    ('chair', 44.00, false),
    ('stool', 25.99, true),
    ('table', 124.00, false);
```
---
## 4) Display all of the rows and columns in the table.

```sql
SELECT * FROM products;
```
---
## 5) Display all of the names of the products.

```sql
SELECT
    name
FROM
    products;
```
---
## 6) Display all of the names and prices of the products.

```sql
SELECT
    name,
    price
FROM
    products;
```
---
### 7) Add a new product - make up whatever you would like!

```sql
INSERT INTO
    products (name, price, can_be_returned)
VALUES
    ('book', 100.00, true);
```
---
### 8) Display only the products that `can_be_returned`

```sql
SELECT name, price FROM products WHERE can_be_returned;
```
---
### 9) Display only the products that have a price less than 44.00.

```sql
SELECT name, can_be_returned FROM products WHERE price < 44.00;
```
---
### 10) Display only the products that have a price in between 22.50 and 99.99.

```sql
SELECT name, can_be_returned FROM products WHERE price BETWEEN 22.50 and 99.99;
```
---
### 11) There's a sale going on: Everything is $20 off! Update the database accordingly.

```sql
UPDATE products SET price=price-20;
```
---
### 12) Because of the sale, everything that costs less than $25 has sold out.
###  Remove all products whose price meets this criteria.

```sql
DELETE FROM products WHERE price<25;
```
---
### 13) And now the sale is over. For the remaining products, increase their price by $20.

```sql
UPDATE products SET price=price+20;
```
---
### 14) There's been a change in company policy, and now all products are returnable

```sql
UPDATE products SET can_be_returned=true;
```
---

# Part IV
# Queries_PlayStore.sql

### Query 0
```sql
SELECT * FROM analytics;
```
---
### 1) Find the entire record for the app with an ID of `1880`.
```sql
SELECT app_name FROM analytics WHERE id=1880;
```
---
### 2) Find the ID and app name for all apps that were last updated on August 01, 2018.

```sql
SELECT id,app_name from analytics WHERE last_updated='08-01-2018';
```
---
### 3) Count the number of apps in each category, e.g. "Family | 1972".

```sql
SELECT category, COUNT(*) FROM analytics GROUP BY category;
```
---
### 4) Find the top 5 most-reviewed apps and the number of reviews for each.

```sql
SELECT app_name, reviews FROM analytics ORDER BY reviews DESC LIMIT 5;
```
---
### 5) Find the full record of the app that has the most reviews with a rating greater than equal to 4.8.
```sql
SELECT app_name, reviews, rating FROM analytics WHERE rating >= 4.8 LIMIT 1;
```
---
### 6) Find the average rating for each category ordered by the highest rated to lowest rated.

```sql
SELECT AVG(rating) as average, category FROM analytics GROUP BY category ORDER BY average DESC;
```
---
### 7) Find the name, price, and rating of the most expensive app with a rating that's less than 3

```sql
SELECT app_name, price, rating FROM analytics WHERE rating < 3 ORDER BY price DESC LIMIT 1;
```
---
### 8) Find all records with a min install not exceeding 50, that have a rating.

```sql
SELECT app_name,rating,min_installs FROM analytics WHERE 
min_installs > 0 AND 
min_installs <= 50 AND 
rating > 0 
ORDER BY rating DESC;
```
---
### 9) Find the names of all apps that are rated less than 3 with at least 10000 reviews.

```sql
SELECT app_name,rating,reviews FROM analytics WHERE rating<3 AND reviews>=10000;
```
---
### 10) Find the top 10 most-reviewed apps that cost between 10 cents and a dollar.

```sql
SELECT app_name,price,reviews FROM analytics WHERE price BETWEEN 0.10 AND 1.00 ORDER BY reviews DESC LIMIT 10;
```
---

### 11) Find the most out of date app. Option 1: with a subquery

```sql
SELECT app_name,last_updated FROM analytics x WHERE last_updated < ALL(SELECT last_updated FROM analytics y WHERE x.app_name != y.app_name);
SELECT app_name,last_updated FROM analytics x ORDER BY last_updated ASC LIMIT 1;
SELECT * FROM analytics 
  WHERE last_updated = (SELECT MIN(last_updated) FROM analytics);
```
---
### 12) Find the most expensive app Option 1: with a subquery

```sql
SELECT app_name,price FROM analytics x WHERE price > ALL(SELECT price from analytics y WHERE x.app_name<>y.app_name);
```

### Option 2: without a subquery
```sql
SELECT app_name,price FROM analytics ORDER BY price DESC LIMIT 1;
```
---
### 13) Count all the reviews in the Google Play Store.

```sql
SELECT SUM(reviews) FROM analytics;
```
---
### 14) Find all the categories that have more than 300 apps in them.

```sql
SELECT category,COUNT(*) FROM analytics GROUP BY category HAVING COUNT(*)>300;
```
---

### 15) Find the app that has the highest proportion of reviews to min_installs, among apps that have been installed at least 100,000 times. Display the name of the app along with the number of reviews, the min_installs, and the proportion.

```sql
SELECT app_name, reviews, min_installs, min_installs/reviews AS proportion FROM analytics WHERE min_installs>=100000 ORDER BY proportion DESC;
```
---
### Further Study 1) Find the name and rating of the top rated apps in each category, among apps that have been installed at least 50,000 times.

--180 results
```sql
SELECT COUNT(*) FROM analytics
  WHERE (rating, category) in (
    SELECT MAX(rating), category FROM analytics
      WHERE min_installs >= 50000
      GROUP BY category
    );
```

```sql
SELECT app_name, rating, category FROM analytics
  WHERE (rating, category) in (
    SELECT MAX(rating), category FROM analytics
      WHERE min_installs >= 50000
      GROUP BY category
    )
  ORDER BY category;
```
33 Rows, one category per top rated app
```sql
WITH ranked_apps AS 
    (SELECT app_name, category, rating, min_installs, 
        ROW_NUMBER() OVER (PARTITION BY category ORDER BY rating DESC) 
        AS ranking
    FROM analytics WHERE 
        min_installs >= 50000 AND rating > 0)
SELECT app_name, rating, category, min_installs 
    FROM ranked_apps WHERE ranking = 1;
```

```sql
SELECT app_name, rating, category FROM (SELECT app_name, category, rating, ROW_NUMBER() OVER (PARTITION BY category ORDER BY rating DESC) AS ranking FROM analytics WHERE min_installs >= 50000) AS ranked_apps WHERE ranking = 1;
```
---
### Further Study 2) Find all the apps that have a name similar to "facebook".

```sql
SELECT app_name FROM analytics WHERE app_name ~* 'facebook';
SELECT app_name FROM analytics WHERE app_name ILIKE '%facebook%';
```
---
### Further Study 3) Find all the apps that have more than 1 genre.

```sql
SELECT COUNT(*) FROM analytics WHERE array_length(genres,1) > 1;
SELECT * FROM analytics WHERE array_length(genres,1) > 1;
SELECT app_name, genres, array_length(genres,1) AS genre_count FROM analytics WHERE array_length(genres,1) > 1 ORDER BY array_length(genres,1) DESC;
SELECT * FROM analytics WHERE array_length(genres, 1) = 2;
SELECT COUNT(*) FROM analytics WHERE array_length(genres, 1) = 2;
```
---
### Further Study 4) Find all the apps that have education as one of their genres.

```sql
SELECT app_name,genres,category FROM analytics WHERE '{Education}' <@ genres;
SELECT * FROM analytics WHERE genres @> '{"Education"}';
SELECT COUNT(*) FROM analytics WHERE '{Education}' <@ genres;
SELECT COUNT(*) FROM analytics WHERE genres @> '{"Education"}';
```
