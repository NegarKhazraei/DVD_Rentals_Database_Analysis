# Blockbuster Data Manipulation with PostgreSQL

## Project Overview
In this project, I explored data manipulation techniques using PostgreSQL's built-in functions, focusing on string, numeric, and date/time data types. This project is designed to simulate data analysis for a fictional Blockbuster video store, showcasing the use of PostgreSQL functions to transform, query, and analyze data related to movie rentals and store operations. The project demonstrates real-world SQL data manipulation and PostgreSQL's advanced text search capabilities.(This project was completed as part of the "Functions for Manipulating Data in PostgreSQL" course on DataCamp.)

## Data Source
The dataset simulates transactional data from a Blockbuster video rental store, including details about movies, rentals, customers, and rental dates. This data is part of the "Functions for Manipulating Data in PostgreSQL" module on DataCamp and was provided in a PostgreSQL database format for the purpose of practicing SQL functions and operations.

## Key Skills and Techniques

- **Data Manipulation with PostgreSQL Functions**: Utilized built-in PostgreSQL functions to handle various data types:
  - **String Functions**: Cleaned, formatted, and extracted data from text fields, such as movie titles and customer names.
  - **Numeric Functions**: Calculated values based on rental prices, stock counts, and customer spending.
  - **Date/Time Functions**: Transformed, formatted, and calculated intervals for dates, such as rental and release dates.
- **Full-Text Search**: Employed PostgreSQL’s full-text search capabilities to search and match keywords within text data.
- **PostgreSQL Extensions**: Learned how to extend PostgreSQL’s features with extensions for enhanced functionality and analysis.

---

## Data Cleaning and Processing

### Step 1: Concatenating Strings for Customer Email Formatting

In this step, I used string concatenation to format customer names and email addresses into a "To" field format that would be useful in an email script or automation program. This format combines a customer’s full name with their email address in the following structure:
```sql
Full Name <email@example.com>
```
```sql
-- Concatenate the first_name and last_name 
SELECT first_name || ' ' || last_name  || ' <' || email || '>' AS full_email 
FROM customer
```
OR 
```sql
-- Concatenate the first_name and last_name and email
SELECT CONCAT(first_name, ' ', last_name, ' <', email, '>') AS full_email 
FROM customer
```

<img width="571" alt="Screenshot 2024-11-13 at 11 03 38" src="https://github.com/user-attachments/assets/89ebfded-b0af-471a-b579-7a87e992b81e">

---

### Step 2: Removing Whitespace in Film Titles

In this step, I focused on data cleansing by removing whitespace in film titles. To create a more uniform and searchable format, I replaced any spaces within the title column of the film table with underscores (_). This technique is often used to prepare data for systems where whitespace can cause formatting issues or when a standard naming convention is required.

```sql
SELECT 
  -- Replace whitespace in the film title with an underscore
  REPLACE(title, ' ', '_') AS title
FROM film;
```

<img width="318" alt="Screenshot 2024-11-13 at 11 17 33" src="https://github.com/user-attachments/assets/84724f63-027b-4bb3-87fb-c180aa74d518">

---

### Step 3: Extracting Street Names from Address Data


<img width="1105" alt="Screenshot 2024-11-13 at 12 42 13" src="https://github.com/user-attachments/assets/d95f8566-d332-4961-a004-8ec70b361803">

In this step, I focused on extracting just the street name from the address column in the address table. The address data contains both the street number and the street name, but for analysis purposes, we are only interested in the street name

```sql
SELECT 
  -- Select only the street name from the address table
  SUBSTRING(address FROM POSITION(' ' IN address)+1 FOR LENGTH(address))
FROM 
  address;
```

<img width="310" alt="Screenshot 2024-11-13 at 12 45 13" src="https://github.com/user-attachments/assets/0e83b95f-5381-4f34-a1ca-b45c7e083a25">

---

### Step 4: Parsing Email Addresses into Username and Domain


<img width="996" alt="Screenshot 2024-11-13 at 13 21 50" src="https://github.com/user-attachments/assets/92e66632-d2f5-4b80-8080-0eef6debb785">

In this step, I demonstrated how to break down a single email column into two new derived fields: username and domain. Parsing email addresses in this way is useful when you need to analyze or extract specific pieces of information from the email address, such as the domain name or username, for tasks like customer segmentation, identifying email providers, or gathering insights about customer behaviour based on domain usage.

```sql
SELECT
  -- Extract the characters to the left of the '@'
  LEFT(email, POSITION('@' IN email)-1) AS username,
  -- Extract the characters to the right of the '@'
  SUBSTRING(email FROM POSITION('@' IN email)+1 FOR LENGTH(email)) AS domain
FROM customer;
```

<img width="796" alt="Screenshot 2024-11-13 at 13 23 03" src="https://github.com/user-attachments/assets/93d94f40-fd68-4f12-bb21-3904124979fb">

---

## Exploratory Data Analysis (EDA)

### Step 1: Exploring the Database Structure

To begin our analysis of the DVD Rentals database, we need to understand its structure, including the tables and views available for analysis. PostgreSQL provides a system database called `INFORMATION_SCHEMA`, which stores metadata about the database itself. By querying this schema, we can retrieve information about all tables and views in our database.

```sql
-- Get the column name and data type
SELECT
 	column_name, 
    data_type
-- From the system database information schema
FROM INFORMATION_SCHEMA.COLUMNS 
-- For the customer table
WHERE table_name ='customer';
```
<img width="610" alt="Screenshot 2024-11-11 at 17 32 22" src="https://github.com/user-attachments/assets/e992d38f-1b76-4b29-9de3-6ad69583b2c9">

---

### Step 2: Calculating Expected Return Dates

To continue my analysis, I calculated the expected return date for each DVD rental. Since the rental policy specifies a 3-day rental period, I used PostgreSQL's `INTERVAL` data type to add 3 days to the `rental_date` for each transaction. This step demonstrates date manipulation in PostgreSQL, which is useful for calculating due dates and deadlines.

```sql
SELECT
    -- Select the rental and return dates
    rental_date,
    return_date,
    -- Calculate the expected return date by adding a 3-day INTERVAL
    rental_date + INTERVAL '3 days' AS expected_return_date
FROM 
    rental;
```

<img width="905" alt="Screenshot 2024-11-11 at 17 13 45" src="https://github.com/user-attachments/assets/e3137ed1-8954-47ca-82a7-cd7a2733473a">

---

### Step 3: Filtering Films by Special Features Using Array Indexing

In this step, I demonstrated how to work with PostgreSQL's array data type. I queried the film table to select the title and special_features columns, where special_features is an array. Using array indexing, I filtered the rows to find films where the first special feature is 'Trailers'. This shows how to access and filter specific elements from an array in a SQL query.

```sql
-- Select the title and special features column 
SELECT 
  title, 
  special_features 
FROM film
-- Use the array index of the special_features column
WHERE special_features[1] = 'Trailers';
```

<img width="780" alt="Screenshot 2024-11-11 at 21 19 17" src="https://github.com/user-attachments/assets/dddcbf6b-0ffe-4683-9a6d-4d41dd60d956">

---

### Step 4: Searching for Values in Arrays Using the ANY Function

In this step, I demonstrated how to use the ANY function in PostgreSQL to search for a value within an entire array, regardless of its position. I applied this to the special_features array in the film table to filter films where 'Trailers' appeared in any index of the array.

```sql
SELECT 
  title, 
  special_features 
FROM film 
-- Modify the query to use the ANY function 
WHERE 'Trailers' = ANY (special_features);
```

<img width="792" alt="Screenshot 2024-11-11 at 21 23 47" src="https://github.com/user-attachments/assets/346f8e24-5626-4947-8ac8-3213dc787f33">

---

### Step 5: Calculating the Number of Special Features Using array_length()

In this step, I used PostgreSQL's array_length() function to calculate the number of elements in the special_features array for each film. This function helps in determining how many special features are associated with each film, which can be useful for filtering, analysis, or understanding the variety of features available for each film.

```sql
SELECT 
  title, 
  special_features,
  array_length(special_features,1) AS number_of_special_features
FROM film
```

<img width="1103" alt="Screenshot 2024-11-11 at 21 32 41" src="https://github.com/user-attachments/assets/74d935cd-fb31-4f98-8227-fc2fa5c1d616">

---

### Step 6: Calculating Actual Rental Duration with Table Joins

In this step, I calculated the actual rental duration in days for each film by subtracting the rental_date from the return_date in the rental table. To access data from both the film and rental tables in a single query, I joined three tables: film, inventory, and rental. This allowed me to calculate the number of days a DVD was actually rented based on the rental and return dates.

```sql
SELECT f.title, f.rental_duration,
       -- Calculate the number of days rented
       r.return_date - r.rental_date AS days_rented
FROM film AS f
     INNER JOIN inventory AS i ON f.film_id = i.film_id
     INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
ORDER BY f.title;
```

<img width="905" alt="Screenshot 2024-11-12 at 15 37 01" src="https://github.com/user-attachments/assets/b62a88ba-e551-4d37-a967-42a096bb8837">

---

### Step 7: Calculating Rental Duration with the AGE() Function

In this step, I used the AGE() function in PostgreSQL to calculate the rental duration for each film more precisely. 

```sql
SELECT f.title, f.rental_duration,
	-- Calculate the number of days rented
	AGE(r.return_date, r.rental_date) AS days_rented
FROM film AS f
	INNER JOIN inventory AS i ON f.film_id = i.film_id
	INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
ORDER BY f.title;
```

<img width="959" alt="Screenshot 2024-11-12 at 15 49 34" src="https://github.com/user-attachments/assets/3e4d621d-f616-4b95-971d-bf542e2b7b43">

---

### Step 8: Using INTERVAL Arithmetic to Calculate Rental Duration

In this step, I used PostgreSQL INTERVAL arithmetic to convert the rental_duration from a numeric value (days) into an interval type, which provides a clearer representation of the rental duration. Additionally, I filtered out records where the film is still out on rental (i.e., where return_date is NULL). This approach allows me to focus on rentals that have been returned and makes it easier to calculate and compare the actual rental duration against the allowed rental period.

```sql
SELECT
    f.title,
 	-- Convert the rental_duration to an interval
    INTERVAL '1' day * f.rental_duration AS rental_days,
 	-- Calculate the days rented as we did previously
    r.return_date - r.rental_date AS days_rented
FROM film AS f
    INNER JOIN inventory AS i ON f.film_id = i.film_id
    INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
-- Filter the query to exclude outstanding rentals
WHERE r.return_date IS NOT NULL
ORDER BY f.title;
```


<img width="918" alt="Screenshot 2024-11-12 at 16 09 00" src="https://github.com/user-attachments/assets/d9b69009-7757-4e9a-beeb-acb080ed68ab">

---

### Step 9: Calculating Expected Return Dates for Rentals

In this step, I calculated the expected_return_date for each film rental by adding the rental_duration (the number of days a rental is allowed) to the rental_date. This provides a due date for each rental, which can be compared with the return_date to determine if a rental was returned late. Calculating the expected_return_date is useful for tracking on-time returns and managing overdue items.

```sql
SELECT
    f.title,
	r.rental_date,
    f.rental_duration,
    -- Add the rental duration to the rental date
    INTERVAL '1' day * f.rental_duration + r.rental_date AS expected_return_date,
    r.return_date
FROM film AS f
    INNER JOIN inventory AS i ON f.film_id = i.film_id
    INNER JOIN rental AS r ON i.inventory_id = r.inventory_id
ORDER BY f.title;
```


<img width="1066" alt="Screenshot 2024-11-12 at 16 22 50" src="https://github.com/user-attachments/assets/91cdd55c-4098-4740-aca1-0964b7ad9bb1">

---

### Step 10: Aggregating Rentals with DATE_TRUNC()

In this step, I used the DATE_TRUNC() function to group rental records by different time intervals such as year, month, and day. By truncating the rental_date field, I was able to group the rentals by year, month, and day to see trends in rental activity.

```sql
SELECT 
  DATE_TRUNC('day', rental_date) AS rental_day,
  COUNT(rental_id) as rentals 
FROM rental
GROUP BY rental_day;

```

<img width="891" alt="Screenshot 2024-11-12 at 18 09 52" src="https://github.com/user-attachments/assets/96807bc7-d682-4f6e-86ad-2afecd80b4a1">

---

### Step 11: Identifying Late Rentals with CASE and Date/Time Functions

In this part of the project, I used SQL date/time functions (EXTRACT(), DATE_TRUNC(), and AGE()) and a CASE statement to analyze rental patterns and identify late DVD returns. The goal was to pull customer rental data over a 90-day period and determine whether DVDs were returned late based on the rental duration allowed.

```sql
SELECT 
  c.first_name || ' ' || c.last_name AS customer_name,
  f.title,
  r.rental_date,
  -- Extract the day of week date part from the rental_date
  EXTRACT(dow FROM r.rental_date) AS dayofweek,
  AGE(r.return_date, r.rental_date) AS rental_days,
  -- Use DATE_TRUNC to get days from the AGE function
  CASE WHEN DATE_TRUNC('day', AGE(r.return_date, r.rental_date)) > 
    f.rental_duration * INTERVAL '1' day 
  THEN TRUE 
  ELSE FALSE END AS past_due 
FROM 
  film AS f 
  INNER JOIN inventory AS i 
  	ON f.film_id = i.film_id 
  INNER JOIN rental AS r 
  	ON i.inventory_id = r.inventory_id 
  INNER JOIN customer AS c 
  	ON c.customer_id = r.customer_id 
WHERE 
  -- Use an INTERVAL for the upper bound of the rental_date 
  r.rental_date BETWEEN CAST('2005-05-01' AS DATE) 
  AND CAST('2005-05-01' AS DATE) + INTERVAL '90 day';
```


<img width="1103" alt="Screenshot 2024-11-12 at 20 48 01" src="https://github.com/user-attachments/assets/1149bd24-ddf1-46d6-853b-8b03e757779d">

---

### Step 12: Concatenating and Formatting Film Categories and Titles

In this step, I enhanced the readability and presentation of film data by applying case transformations. I created a new field, film_category, by concatenating the uppercase category name with the title-cased film title. Additionally, I formatted the description field in lowercase to ensure a standardized look across all records.

```sql
SELECT 
  -- Concatenate the category name to coverted to uppercase
  -- to the film title converted to title case
  UPPER(c.name) || ': ' || INITCAP(f.title) AS film_category,
  -- Convert the description column to lowercase
  LOWER(f.description) AS description
FROM 
  film AS f 
  INNER JOIN film_category AS fc 
  	ON f.film_id = fc.film_id 
  INNER JOIN category AS c 
  	ON fc.category_id = c.category_id;
```

<img width="1042" alt="Screenshot 2024-11-13 at 11 14 40" src="https://github.com/user-attachments/assets/1bb190c6-702e-424f-b8d2-7af29e0a58ff">

---


