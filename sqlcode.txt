/*Question 1 Question Set 1*/

SELECT
  title,
  category,
  COUNT(rental_id) AS rental_count
FROM (SELECT
  category.name AS category,
  film.title AS title,
  rental_id
FROM category
JOIN film_category
  ON category.category_id = film_category.category_id
JOIN film
  ON film.film_id = film_category.film_id
JOIN inventory
  ON film.film_id = inventory.film_id
JOIN rental
  ON inventory.inventory_id = rental.inventory_id
WHERE category.name = 'Animation'
OR category.name = 'Children'
OR category.name = 'Classics'
OR category.name = 'Comedy'
OR category.name = 'Family'
OR category.name = 'Music') sub_table
GROUP BY 1,
         2
ORDER BY 2;

/*Question 2 Question Set 1*/

SELECT
  film.title AS title,
  category.name AS name,
  film.rental_duration AS rental_duration,
  NTILE(4) OVER (ORDER BY rental_duration) AS quartile
FROM category
JOIN film_category
  ON category.category_id = film_category.category_id
JOIN film
  ON film.film_id = film_category.film_id
WHERE name = 'Animation'
OR name = 'Children'
OR name = 'Classics'
OR name = 'Comedy'
OR name = 'Music'
OR name = 'Family'
ORDER BY 4;

/*Question 3 Question Set 1*/

SELECT
  name,
  quartile,
  COUNT(film_id)
FROM (SELECT
  category.name AS name,
  NTILE(4) OVER (ORDER BY film.rental_duration) AS quartile,
  film.film_id AS film_id
FROM category
JOIN film_category
  ON category.category_id = film_category.category_id
JOIN film
  ON film.film_id = film_category.film_id
WHERE name = 'Animation'
OR name = 'Children'
OR name = 'Classics'
OR name = 'Comedy'
OR name = 'Music'
OR name = 'Family') sub_table
GROUP BY 1,
         2
ORDER BY 1, 2;

/*Question 1 Question Set 2*/

SELECT
  DATE_PART('month', rental.rental_date) AS month,
  DATE_PART('year', rental.rental_date) AS year,
  staff.store_id AS store_id,
  COUNT(rental_id) AS count
FROM rental
JOIN staff
  ON rental.staff_id = staff.staff_id
GROUP BY 2,
         1,
         3
ORDER BY 4 DESC;
