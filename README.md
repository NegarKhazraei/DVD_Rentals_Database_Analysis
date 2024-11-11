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

---

## Step 2: Calculating Expected Return Dates

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



