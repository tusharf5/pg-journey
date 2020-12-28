# PG Journey

## Instructions to install

### Requirements

- docker >= 17.12.0+
- docker-compose

### Quick Start

- Clone or download this repository
- Go inside of directory, `cd compose-postgres`
- Run this command `docker-compose up -d`

### Environments

This Compose file contains the following environment variables:

- `POSTGRES_USER` the default value is **admin**
- `POSTGRES_PASSWORD` the default value is **password**
- `PGADMIN_PORT` the default value is **5050**
- `PGADMIN_DEFAULT_EMAIL` the default value is **admin@pgadmin.org**
- `PGADMIN_DEFAULT_PASSWORD` the default value is **password**

### Access to postgres

- `localhost:5432`
- **Username:** postgres (as a default)
- **Password:** changeme (as a default)

### Access to PgAdmin

- **URL:** `http://localhost:5050`
- **Username:** pgadmin4@pgadmin.org (as a default)
- **Password:** admin (as a default)

### Add a new server in PgAdmin

- **Host name/address** `postgres`
- **Port** `5432`
- **Username** as `POSTGRES_USER`, by default: `admin`
- **Password** as `POSTGRES_PASSWORD`, by default `password`

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

### Within A Range

```sql
SELECT first_name, last_name, created_at FROM people WHERE age BETWEEN 24 and 30;
```

### Match Within A Set Of Values

```sql
SELECT first_name, last_name, created_at FROM people WHERE age IN (24, 36, 48)
```

### Case Sensitive Pattern Match

**"%"** a wildcard matching one or more characters.

**"\_"** a wildcard matching just one character.

```sql
SELECT first_name, last_name, created_at FROM people WHERE first_name LIKE 'Sam%';
SELECT first_name, last_name, created_at FROM people WHERE first_name LIKE 'S_m';
SELECT first_name, last_name, created_at FROM people WHERE first_name LIKE '%ohn%';
SELECT first_name, last_name, created_at FROM people WHERE first_name LIKE 'ba_er';
```

### Case In Sensitive Pattern Match (Pg-Only)

**"%"** a wildcard matching one or more characters.

**"\_"** a wildcard matching just one character.

```sql
SELECT first_name, last_name, created_at FROM people WHERE first_name ILIKE 'Sam%';
SELECT first_name, last_name, created_at FROM people WHERE first_name ILIKE 'S_m';
SELECT first_name, last_name, created_at FROM people WHERE first_name ILIKE '%ohn%';
SELECT first_name, last_name, created_at FROM people WHERE first_name ILIKE 'ba_er';
```

### Negates A Condition

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

## Understanding Data Types

### Character Types

#### char(n)

A fixed-length column where the column length is specified by **n**. If you insert a column with
fewer characters, Pg will pad the value to make the length equal to **n**.

Not popuplar anymore.

#### varchar(n)

A variable-length column where the maximum length is specified by **n**.

#### text

A variable-length column of **unlimited** length. In Pg, the longest possible value to store is 1 GB of text in a single column.

There is no performance difference between the three but choose wisely depending on your usage, data integrity and how you think the data will scale.

```sql
CREATE TABLE my_people (
  gender char(1),
  name varchar(30),
  bio text
);

INSERT INTO my_people (gender, name, bio)
VALUES
('M', 'Tushar', 'Eu aliquip commodo veniam reprehenderit aute sint id deserunt eu amet.'),
('M', 'Jimmy', 'Mollit ex nisi deserunt cillum ad ut nisi ut commodo sit. Sunt ullamco eiusmod commodo velit cupidatat ad minim ea. Irure voluptate dolore adipisicing magna esse. Sit non adipisicing officia et.'),
('F', 'Alex', 'cillum'),
```

### Numbers

The SQL number types include.

**Integers** - Whole numbers, both positive and negative.

**Fixed-point and floating-point** - Two formats of the fraction of the whole numbers.

#### Integer Types

The difference between three integer types that Pg supports is in the maximum size of the numbers that they can hold.

##### smallint

- 2 bytes
- -32k to +32k

##### integer

- 4 bytes
- -2.1 billion to +2.1 billion

##### bigint

- 8 bytes
- meh

Again, choose the type wisely based on the usage and how you think it would scale.

> Use bigint whenever possible. Only use smallint or integer when you absolutely know the values won't cross the limit for example days in a month, year.

#### Auto Incrementing Integer Types

These are special type based on the **Integer** types. The special property is that it auto-increments for every new row starting
from 1. There might be gaps or inconsistencies in the auto-incremented values for example when you delete rows or when a row insertion is aborted.

##### smallserial

- 2 bytes
- -32k to +32k

##### serial

- 4 bytes
- -2.1 billion to +2.1 billion

##### bigserial

- 8 bytes
- meh

#### Decimal Types

> Precision is the number of digits in a number. Scale is the number of digits to the right of the decimal point in a number. For example, the number 123.45 has a precision of 5 and a scale of 2.

Decimals is pg can be represented by **fixed-point** and **floating-point** data types.

##### Fixed-Point Data Types

###### numeric(precision, scale) OR decimal(precision, scale)

Where _precision_ is the maximum number of digits in the number and sclae is the allowable number to the right side of the decimal.

If you omit specifying a scale, the scale is set to 0 and this behaves like an **integer** type with the same limitations.

- variable size storage
- upto 130k before decimal and upto 16k after decimal.

If you specify a scale as numeric(5,2) the database will always return two digits to the right of the decimal even if the number you saved didn't have none.

If you have specified a type for a column as numeric(5,2), then

- It will always have 2 digits to the right of the decimal.
- Maximum total of 5 digits (both including left and right side of decimal allowed)
- This implies that the maximum allowed number of digits to the left of decimal is (5-2) which is **3**.

##### Floating-Point Data Types

Also called variable-precision types. Unlike **numeric**, where we specify a fixed precision for every number in a column,
in floating types, the decimal point can "float" depending upon the number. You can have either one digit after decimal, or 2, or more upto the max allowed and values can have different scales.

Floating points are not really great with maths operations that's why they are also called "inexact" types. The reason being computers try to squeeze in a lot of information in a few bytes when storing these types.

###### real

This floating type allows precision upto 6 decimal digits.

- 6 digits precision.

###### double

This floating type allows precision upto 15 decimal digits.

- 15 digits precision.

> If you are doing calculations, use numeric type to avoid errors.

### Dates and Times Types

#### timestamptz

This is **timestamp** data type with time zone also saved in the database along with the value. There is another variant **timestamp** which by default does not store the timezone which you should not do.

This is good for storing ISO strings. Takes 8 bytes.

#### date

Recommended format is `YYYY-MM-DD`. Takes 4 bytes.

#### time

Takes 8 bytes.

Variants types include

- time without time zone
- time with time zone

> time alone is equivalent to **time without time zone**.

Some valid values.

- 04:05
- 04:05:06
- 04:05:06 PST

#### interval

Takes 16 bytes.

Valid values include

| Example                                            | Description                                             |
| -------------------------------------------------- | ------------------------------------------------------- |
| 1-2                                                | SQL standard format: 1 year 2 months                    |
| 3 4:05:06                                          | SQL standard format: 3 days 4 hours 5 minutes 6 seconds |
| 1 year 2 months 3 days 4 hours 5 minutes 6 seconds | Traditional Postgres format                             |

> pg gives you util functions to use in your queries for some time related features like now(), today(), tomorrow(), and yesterday().

### Other Types

- A boolean type that stores true and false
- Network address types, such as IP or MAC addresses.
- UUID type
- XML or JSON type

### Transforming Values From One Type To Another

CAST function can be used to perform conversion from one data type to another if the target data type can accomodate the original
value.

```sql
SELECT name, CAST(user_age AS integer) FROM users;
SELECT name, user_age::integer FROM users;
```
