# Chapter 3: Complex Queries

## 3.1: Special operators and clauses
### `IN` operator
- The `IN` operator is used in a `WHERE` clause to check if a value matches one of several values
- ```mysql
  CREATE TABLE CountryLanguage(
      CountryCode CHAR(3),
      Language VARCHAR(20),
      IsOfficial CHAR(1),
      Percentage DECIMAL(3, 1)
  );
  
  INSERT INTO CountryLanguage
  VALUES ('ABW', 'Dutch', 'T', 5.3),
         ('AFG', 'Balochi', 'F', 0.9),
         ('AGO', 'Kongo', 'F', 13.2),
         ('ALB', 'Albanian', 'T', 97.9),
         ('AND', 'Catalan', 'T', 32.3);
  
  SELECT *
  FROM CountryLanguage
  WHERE Language IN ('Dutch', 'Kongo', 'Albanian');
  ```
### `BETWEEN` operator
- The `BETWEEN` operator provides an alternative way to determine if a value is between two other values, written as `value BETWEEN minValue AND maxValue` and is equivalent to `value >= minValue AND value <= maxValue`
- ```mysql
  SELECT Name
  FROM Employee
  WHERE HireDate BETWEEN '2000-01-01' AND '2020-01-01';
  ```

### `LIKE` operator
- The `LIKE` operator, when used in a `WHERE` clause, matches text against a pattern using wildcard characters `%` and `_`
  - `%` matches any number of characters
  - `_` matches exactly one character
- The `LIKE` operator performs case-insensitive pattern matching by default, but can do case-sensitive if followed by `BINARY` keyword (ex. `LIKE BINARY 'L%t'`)
- To escape wildcard characters, a backslash must precede them
- Other mechanisms can be used for pattern matching, for example, MySQL uses REGEXP (regular expressions)

### `DISTINCT` keyword
- The `DISTINCT` keyword is used with `SELECT` statements to return only unique/distinct values, this can help deduplicate query results
- ```mysql
  CREATE TABLE CountryLanguage (
      CountryCode CHAR(3) NOT NULL,
      Language VARCHAR(20) NOT NULL,
      IsOfficial CHAR(1) NOT NULL,
      Percentage DECIMAL(4,1),
      PRIMARY KEY (CountryCode, Language)
  );
  INSERT INTO CountryLanguage VALUES
      ('ABW', 'Spanish', 'F', 7.4),
      ('AFG', 'Balochi', 'F', 0.9),
      ('ARG', 'Spanish', 'T', 96.8),
      ('BLZ', 'Spanish', 'F', 31.6),
      ('BRA', 'Portuguese', 'T', 97.5)
  ;
  SELECT Language
  FROM CountryLanguage
  WHERE IsOfficial = 'F';
  ```

### `ORDER BY` clause
- A `SELECT` statement selects rows from a table with no guarantee that the rows come back in a certain order. `ORDER BY` orders selected rows by one or more columns in ascending (alphabetic or numeric) order. Adding `DESC` keyword reverses the row ordering
- ```mysql
  CREATE TABLE CountryLanguage (
      CountryCode CHAR(3) NOT NULL,
      Language VARCHAR(20) NOT NULL,
      IsOfficial CHAR(1) NOT NULL,
      Percentage DECIMAL(4,1),
      PRIMARY KEY (CountryCode, Language)
  );
  INSERT INTO CountryLanguage VALUES
      ('FSM', 'Woleai', 'F', 3.7),
      ('FSM', 'Yap', 'F', 5.8),
      ('GAB', 'Fang', 'F', 35.8),
      ('GAB', 'Mbete', 'F', 13.8)
  ;
  SELECT *
  FROM CountryLanguage
  ORDER BY Language;
  
  SELECT *
  FROM CountryLanguage
  ORDER BY Language DESC;
  ```

## 3.2: Simple functions
### Numeric functions
- A **function** operates on an expression enclosed in parentheses, called an **argument**, and returns a value. Usually the argument is a simple expression like a column name or fixed value. Some functions will have multiple values and some will have no values
- Each function operates on and evaluates to specific data types, for example, numeric functions operate on and evaluate to integer and decimal data types
- | Function      | Description                                                     | Example                                                           |
  |---------------|-----------------------------------------------------------------|-------------------------------------------------------------------|
  | *ABS(n)*      | Returns the absolute value of *n*                               | <pre><code>SELECT ABS(-5);</code></pre>returns 5                  |
  | *LOG(n)*      | Returns the natural logarithm of *n*                            | <pre><code>SELECT LOG(10);</code></pre>returns 2.302585092994046  |
  | *POW(x, y)*   | Returns *x* to the power of *y*                                 | <pre><code>SELECT POW(2, 3);</code></pre>returns 8                |
  | *RAND()*      | Returns a random number between 0 (inclusive) and 1 (exclusive) | <pre><code>SELECT RAND();</code></pre>returns 0.11831825703225868 |
  | *ROUND(n, d)* | Returns *n* rounded to *d* decimal places                       | <pre><code>SELECT ROUND(16.25, 1);</code></pre>returns 16.3       |
  | *SQRT(n)*     | Returns the square root of *n*                                  | <pre><code>SELECT SQRT(25);</code></pre>returns 5                 |

### String functions
- String functions manipulate string values and are similar to string functions in other programming languages like Java/Python
- | Function                 | Description                                                                       | Example                                                                                     |
  |--------------------------|-----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
  | *CONCAT(s1, s2, ...)*    | Returns the string that results from concatenating the string arguments           | <pre><code>SELECT CONCAT('Dis', 'en', 'gage');</code></pre>returns 'Disengage'              |
  | *LOWER(s)*               | Returns the lowercase *s*                                                         | <pre><code>SELECT LOWER('MySQL');</code></pre>returns 'mysql'                               |
  | *REPLACE(s, from, to)*   | Returns the string *s* with all occurrences of *from* replaced with *to*          | <pre><code>SELECT REPLACE('This and that', 'and', 'or');</code></pre>returns 'This or that' |
  | *SUBSTRING(s, pos, len)* | Returns the substring from *s* that starts at position *pos* and has length *len* | <pre><code>SELECT SUBSTRING('Boomerang', 1, 4);</code></pre>returns 'Boom'                  |
  | *TRIM(s)*                | Returns the string *s* without leading and trailing spaces                        | <pre><code>SELECT TRIM('  test  ');</code></pre>returns 'test'                              |
  | *UPPER(s)*               | Returns the uppercase *s*                                                         | <pre><code>SELECT UPPER('mysql');</code></pre>returns 'MYSQL'                               |

### Date and time functions
- Date and time functions operate on `DATE`, `TIME` and `DATETIME` data types
- | Function                                             | Description                                                                                                              | Example                                                                                                                                                                                    |
  |------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  | *CURDATE()*<br>*CURTIME()*<br>*NOW()*                | Returns the current date, time, or date and time in<br>'YYYY-MM-DD', 'HH:MM:SS', or<br>'YYYY-MM-DD HH:MM:SS' format      | <pre><code>SELECT CURDATE();</code></pre>returns '2019-01-25'<pre><code>SELECT CURTIME();</code></pre>returns '21:05:44'<pre><code>SELECT NOW();</code></pre>returns '2019-01-25 21:05:44' |
  | *DATE(expr)*<br>*TIME(expr)*                         | Extracts the date or time from a date or datetime expression *expr*                                                      | <pre><code>SELECT DATE('2013-03-25 22:11:45');</code></pre>returns '2013-03-25'<pre><code>SELECT TIME('2013-03-25 22:11:45');</code></pre>returns '22:11:45'                               |
  | *DAY(d)*<br>*MONTH(d)*<br>*YEAR(d)*                  | Returns the day, month, or year from date *d*                                                                            | <pre><code>SELECT DAY('2016-10-25');</code></pre>returns 25<pre><code>SELECT MONTH('2016-10-25');</code></pre>returns 10<pre><code>SELECT YEAR('2016-10-25');</code></pre>returns 2016     |
  | *HOUR(t)*<br>*MINUTE(t)*<br>*SECOND(t)*              | Returns the hour, minute, or second from time *t*                                                                        | <pre><code>SELECT HOUR('22:11:45');</code></pre>returns 22<pre><code>SELECT MINUTE('22:11:45');</code></pre>returns 11<pre><code>SELECT SECOND('22:11:45');</code></pre>returns 45         |
  | *DATEDIFF(expr1, expr2)*<br>*TIMEDIFF(expr1, expr2)* | Returns *expr1* - *expr2* in number of days or time values, given *expr1* and *expr2* are date, time, or datetime values | <pre><code>SELECT DATEDIFF('2013-03-10', '2013-03-04');</code></pre>returns 6<pre><code>SELECT TIMEDIFF('10:00:00', '09:45:30');</code></pre>returns 00:14:30                              |

## 3.3: Aggregate functions
### Aggregate functions
- An aggregate function processes values from a set of rows and returns a summary value. Common aggregate functions are shown below
  - `COUNT()` counts the number of rows in the set
  - `MIN()` finds the minimum value in the set
  - `MAX()` finds the maximum value in the set
  - `SUM()` sums all values in the set
  - `AVG()` computes the arithmetic mean of all the values in the set
- Aggregate functions appear in a `SELECT` clause and process all rows that satisfy the `WHERE` clause. If there is no `WHERE` clause, the aggregate function processes all rows

### `GROUP BY` clause
- Aggregate functions are commonly used with a `GROUP BY` clause
- The **`GROUP BY`** clause consists of the `GROUP BY` keyword and one or more columns, and each simple/composite value of the column(s) becomes a group. The query then computes the aggregate function separately and returns one row for each group
- The `GROUP BY` clause appears between the `WHERE` clause (if it exists) and the `ORDER BY` clause
- If using `GROUP BY`, may not use columns in the `SELECT` with more than one value per group. See [MySQL Handling of GROUP BY](https://dev.mysql.com/doc/refman/8.0/en/group-by-handling.html)

### `HAVING` clause
- The `HAVING` clause is used with `GROUP BY` clause to filter group results. This optional clause follows `GROUP BY` and precedes the optional `ORDER BY` clause

### Aggregate functions and NULL values
- Aggregate functions will ignore NULL values, but keep in mind that this may be different than how certain arithmetic operators handle NULLs