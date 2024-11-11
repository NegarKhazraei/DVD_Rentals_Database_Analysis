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

## Data Analysis

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





