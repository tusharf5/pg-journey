# PG Journey

## Instructions to install

### Requirements:

* docker >= 17.12.0+
* docker-compose

### Quick Start

* Clone or download this repository
* Go inside of directory,  `cd compose-postgres`
* Run this command `docker-compose up -d`


### Environments

This Compose file contains the following environment variables:

* `POSTGRES_USER` the default value is **admin**
* `POSTGRES_PASSWORD` the default value is **password**
* `PGADMIN_PORT` the default value is **5050**
* `PGADMIN_DEFAULT_EMAIL` the default value is **admin@pgadmin.org**
* `PGADMIN_DEFAULT_PASSWORD` the default value is **password**

### Access to postgres: 

* `localhost:5432`
* **Username:** postgres (as a default)
* **Password:** changeme (as a default)

### Access to PgAdmin: 

* **URL:** `http://localhost:5050`
* **Username:** pgadmin4@pgadmin.org (as a default)
* **Password:** admin (as a default)

### Add a new server in PgAdmin:

* **Host name/address** `postgres`
* **Port** `5432`
* **Username** as `POSTGRES_USER`, by default: `admin`
* **Password** as `POSTGRES_PASSWORD`, by default `password`

## Notes

## Data Exploration with SELECT

Here's a SELECT statement that fetches every row and column from `table_name`.

```sql
SELECT * FROM table_name;
```

Here's a SELECT statement that only selects a few columns.

```sql
SELECT column_one, column_two FROM table_name;
```

Here's a SELECT statement that only selects unique values in that column.

```sql
SELECT DISTINCT gender_column FROM people;
```

Here's a SELECT statement that implies for **EACH** factory, what are **ALL** the **unique** chemicals that it produces.

```sql
SELECT DISTINCT factory_name, chemical FROM factories;
```

Here's a SELECT statement that selects the rows and sort them in a particular order.

By default `ORDER BY` sorts in ascending order.

```sql
SELECT first_name, last_name, created_at FROM people ORDER BY created_at DESC;
SELECT first_name, last_name, created_at FROM people ORDER BY created_at ASC;
```

Here's a SELECT statement that filters rows based on the `WHERE` expression.

### Equal To

```sql
SELECT first_name, last_name, created_at FROM people WHERE first_name = 'Tushar';
```

### Not Equal To

```sql
SELECT first_name, last_name, created_at FROM people WHERE first_name <> 'Tushar';
```

### Greater Than

```sql
SELECT first_name, last_name, created_at FROM people WHERE age > 24;
```

### Less Than

```sql
SELECT first_name, last_name, created_at FROM people WHERE age < 24;
```

### Greater Than Equal To

```sql
SELECT first_name, last_name, created_at FROM people WHERE age >= 24;
```

### Less Than Equal To

```sql
SELECT first_name, last_name, created_at FROM people WHERE age <>= 24;
```

### Within a Range

```sql
SELECT first_name, last_name, created_at FROM people WHERE age BETWEEN 24 and 30;
```

### Match within a set of values

```sql
SELECT first_name, last_name, created_at FROM people WHERE age IN (24, 36, 48)
```

### Case Sensitive Pattern Match

**%** a wildcard matching one or more characters.

**_** a wildcard matching just one character.

```sql
SELECT first_name, last_name, created_at FROM people WHERE first_name LIKE 'Sam%';
SELECT first_name, last_name, created_at FROM people WHERE first_name LIKE 'S_m';
SELECT first_name, last_name, created_at FROM people WHERE first_name LIKE '%ohn%';
SELECT first_name, last_name, created_at FROM people WHERE first_name LIKE 'ba_er';
```

### Case InSensitive Pattern Match (Pg-only)

**%** a wildcard matching one or more characters.

**_** a wildcard matching just one character.

```sql
SELECT first_name, last_name, created_at FROM people WHERE first_name ILIKE 'Sam%';
SELECT first_name, last_name, created_at FROM people WHERE first_name ILIKE 'S_m';
SELECT first_name, last_name, created_at FROM people WHERE first_name ILIKE '%ohn%';
SELECT first_name, last_name, created_at FROM people WHERE first_name ILIKE 'ba_er';
```

### Negates a condition

```sql
SELECT first_name, last_name, created_at FROM people WHERE first_name NOT ILIKE 'Sam%';
SELECT first_name, last_name, created_at FROM people WHERE age NOT IN (24, 36, 48)
SELECT first_name, last_name, created_at FROM people WHERE age NOT BETWEEN 24 and 30;
```

### Combining Filters Using OR and AND

```sql
SELECT first_name, last_name, created_at FROM people WHERE first_name ILIKE 'Sam%' AND age IN (24, 36, 48);
SELECT first_name, last_name, created_at FROM people WHERE age IN (24, 36, 48) OR (first_name = 'Tushar' AND age < 24);
```
