<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

* [Bookstore TX Sandbox](#Bookstore-tx-sandbox)
	* [Overview](#overview)
	* [Online Bookstore Data](#online-Bookstore-data)
	* [System Info](#system-info)
* [Labs](#labs)
	* [General](#general)
	* [Books](#Books)
		* [**Q:**  How many Books do we sell?](#q-how-many-Books-do-we-sell)
		* [**Q:** Lets try to find out who is the youngest customer.](#q-lets-try-to-find-out-who-is-the-youngest-customer)
		* [**Q:** Is this the youngest customer ?](#q-is-this-the-youngest-customer)
		* [**Q:** We are trying to position our online Bookstore towards Fantasy and Sci-Fi theme, but we are also trying to provide good variety of Books as well.](#q-we-are-trying-to-position-our-online-Bookstore-towards-fantasy-and-sci-fi-theme-but-we-are-also-trying-to-provide-good-variety-of-Books-as-well)
		* [**Q:** Are we making good profit from our focus categories ?](#q-are-we-making-good-profit-from-our-focus-categories)
		* [**Q:** Do we provide enough variety ?](#q-do-we-provide-enough-variety)
	* [Customers](#Customers)
		* [**Q:** Who is our customer ?](#q-who-is-our-customer)
		* [**Q:** How many Customers are male and how many are female?](#q-how-many-Customers-are-male-and-how-many-are-female)
		* [**Q:** Query Top Buyers](#q-query-top-buyers)
		* [**Q:**  But do the younger people read more than seniores ?](#q-but-do-the-younger-people-read-more-than-seniores)
		* [**Q:** What are the reading preferencess of our top customer?](#q-what-are-the-reading-preferencess-of-our-top-customer)

<!-- /code_chunk_output -->

## Bookstore TX Sandbox

### Overview

### Online Bookstore Data

This sandbox is based on procedural generated data representing data for a typical online bookstore.
In the examples below we will try to answer the typical questions every business user has at some point when his business catch some speed.
Like “How am I doing?”, “What can be improved?” etc.

### System Info

The data is loaded in a MariaDB Server  one of the most popular open source databases in the world. It is the default database in leading Linux distributions – Arch Linux, CentOS, Debian, Fedora, Manjaro, openSUSE, Red Hat Enterprise Linux and SUSE Linux Enterprise, to name a few.

## Labs
### General 
 Your database is prepared and the sandbox data is loaded. Lets start by choosing the database we want to work on. In this case **Bookstore** database.
 
```sql
USE bookstore;
```

We can check what tables we have.
```sql
SHOW TABLES;
```

Lets see the size of the data
```sql
SELECT 'Addresses' as TableName, count(*) FROM `Addresses` UNION ALL
SELECT 'Books' as TableName, count(*) FROM `Books` UNION ALL
SELECT 'Cards' as TableName, count(*) FROM `Cards` UNION ALL
SELECT 'Covers' as TableName, count(*) FROM `Covers` UNION ALL
SELECT 'Customers' as TableName, count(*) FROM `Customers` UNION ALL
SELECT 'Emails' as TableName, count(*) FROM `Emails` UNION ALL
SELECT 'LoyaltyPoints' as TableName, count(*) FROM `LoyaltyPoints`  UNION ALL
SELECT 'MaritalStatuses' as TableName, count(*) FROM `MaritalStatuses` UNION ALL
SELECT 'Phones' as TableName, count(*) FROM `Phones` UNION ALL
SELECT 'TransactionTypes' as TableName, count(*) FROM `TransactionTypes` UNION ALL
SELECT 'Transactions' as TableName, count(*) FROM `Transactions`;
```

```
+------------------+----------+
| TableName        | count(*) |
+------------------+----------+
| Addresses        |   326952 |
| Books            |     5000 |
| Cards            |   196810 |
| Covers           |       20 |
| Customers        |   245904 |
| Emails           |   314794 |
| LoyaltyPoints    |   126178 |
| MaritalStatuses  |        5 |
| Phones           |   297526 |
| TransactionTypes |        3 |
| Transactions     |  1541044 |
+------------------+----------+
```

### Books

In this first part we will focus mainly on our main commodity - the Books. We want to know what we offer to our Customers and how can be improved.
Lets start by answering our first question.
#### **Q:**  How many Books do we sell?
```sql
SELECT COUNT(*) FROM bookstore.Books;
```
Total Books:
```
+----------+
| COUNT(*) |
+----------+
|     5000 |
+----------+
```

This was easy question 5000.
**!** You can experiment replacing the table name with **Customers**, **transactions** etc.

#### **Q:** Lets try to find out who is the youngest customer. 
```sql
SELECT customer_nm,age FROM bookstore.Customers ORDER BY age LIMIT 1;
```

The result should be somthing like this:

```
+---------------+-----+
| customer_nm   | age |
+---------------+-----+
| Tiffany Green |   8 |
+---------------+-----+
```

#### **Q:** Is this the youngest customer ?

```sql
SELECT count(*) FROM bookstore.Customers WHERE age=8;
```

The result should be somthing like this:

```
+----------+
| count(*) |
+----------+
|     591  |
+----------+
```
Apparantly we have many Customers at that age.

Lets try something harder.

We are trying to position our online Bookstore towards Fantasy and Sci-Fi theme, but we are also trying to provide good variety of Books as well.Did we provide achieve those goals?

#### **Q:** Do we provide enough variety ?
```sql
SELECT category, COUNT(*) as Books FROM bookstore.Books GROUP BY category;
```
The **GROUP BY** statement instructs the database to grup the results by the first column
The Result:
```
+-------------------+-------+
| category          | Books |
+-------------------+-------+
| Children Classics |   650 |
| Classics          |   168 |
| Drama             |   551 |
| Fantasy           |   928 |
| Horror            |   387 |
| Humor             |   306 |
| Mystery           |   453 |
| Non-fiction       |    34 |
| Romance           |   731 |
| Sci-fi            |   792 |
+-------------------+-------+
```
We have the most tiples in our focus categories Fantasy: 928 and Sci-Fi: 792 we provide good variety of books in our focus categories. 

**!** You can try to use the **GROUP BY** with index instead of the column name for the same result.

```sql
SELECT category, COUNT(*) as Books FROM bookstore.Books GROUP BY 1;
```

Will yeald the same result.
+-------------------+-------+
| category          | Books |
+-------------------+-------+
| Children Classics |   650 |
| Classics          |   168 |
| Drama             |   551 |
| Fantasy           |   928 |
| Horror            |   387 |
| Humor             |   306 |
| Mystery           |   453 |
| Non-fiction       |    34 |
| Romance           |   731 |
| Sci-fi            |   792 |
+-------------------+-------+


Now lets try something more tangable. Money.

#### **Q:** Are we making good profit from our focus categories ?
Lets asume for a seciond that the higher the price the more profit we make.

```sql
SELECT category, AVG(cover_price) as projected_profitabiliti FROM bookstore.Books GROUP BY 1;
```
As you can see the the **COUNT(*)** is replaced by **AVG(cover_price)** this will return the average cover price grouped by first column i.e. the book category.

```
+-------------------+------------------------+
| category          | projected_profitabilit |
+-------------------+------------------------+
| Horror            | 11.111320              |
| Fantasy           | 23.299910              |
| Drama             | 13.310811              |
| Classics          | 22.914340              |
| Sci-fi            | 18.757123              |
| Children Classics | 9.463908               |
| Mystery           | 14.242681              |
| Romance           | 9.781252               |
| Non-fiction       | 9.983043               |
| Humor             | 5.393195               |
+-------------------+------------------------+
```

This show the average projected_profitabiliti of the books in each categories. The precision is in thousends of cents which is too much for practical use.

Lets format the **projected_profitability** in currency format. Two digits after decimal separator. 
```sql
SELECT category, FORMAT(AVG(cover_price),2) as projected_profitabilit FROM bookstore.Books GROUP BY 1;
```
+-------------------+------------------------+
| category          | projected_profitabilit |
+-------------------+------------------------+
| Children Classics | 9.25                   |
| Classics          | 22.77                  |
| Drama             | 13.04                  |
| Fantasy           | 23.32                  |
| Horror            | 11.38                  |
| Humor             | 5.25                   |
| Mystery           | 14.50                  |
| Non-fiction       | 9.02                   |
| Romance           | 9.82                   |
| Sci-fi            | 18.78                  |
+-------------------+------------------------+
the result is more readable than before. 

It looks like Fantasy and Sci-Fi Books have also good potential for profit $23.32 and $18.78, if they sell.
There is something intersting the classics have a high potential maybe we should target selling more of those with a propper promotion.

As a result from this quick anlysis we already can make a decision for improvement in the future. 

Let try more complex queries in the next section.

### Customers

In this section we will try to identify who our Customers are ? what are their preferences? how likely is for them to buy somethin out of their main focus.

Lets try to make a demographical profile of our Customers.

The Customers are stored in **bookstore.Customers** we should find more about our most important asset. 
#### **Q:** How many customers do we have?
```sql
select  count(*) from bookstore.Customers LIMIT 10;
```
```
+----------+
| count(*) |
+----------+
|   245904 |
+----------+
```
We are lucky our busines trives.

#### **Q:** Who is our customer ?

```sql
select  * from bookstore.Customers LIMIT 10;
```
The query will give use what we have in this table The *LIMIT 10* statement will limit the results to 10, we only want to see a sample of the sata not the all 1.4 milion Customers.
```
+--------------------+-------------+----------------------------+-----+-----+-------+
| customer_nm        | customer_id | customer_username_nm       | sex | age | ms_id |
+--------------------+-------------+----------------------------+-----+-----+-------+
| Donald Anthony PhD |           1 | natasha77@hutmail.con      | M   |  23 |     1 |
| Wayne Rasmussen    |           2 | mistygilbert@hutmail.con   | M   |  21 |     1 |
| Kathleen Webb      |           3 | nthompson@yupee.con        | F   |  32 |     2 |
| Meghan Webb        |           4 | sanchezkristen@hutmail.con | F   |  25 |     1 |
| Courtney Alexander |           5 | kathrynfrazier@shelf.con   | F   |  57 |     1 |
| Nicole Montoya     |           6 | rothjared@hutmail.con      | F   |  39 |     2 |
| Rebecca Wilkinson  |           7 | thomas80@hutmail.con       | F   |  76 |     2 |
| Manuel Flowers     |           8 | xprice@hutmail.con         | M   |  32 |     2 |
| Lisa Bennett       |           9 | kylesmith@gamail.con       | F   |  71 |     3 |
| Erica Miller       |          10 | jessicageorge@yupee.con    | F   |  52 |     5 |
+--------------------+-------------+----------------------------+-----+-----+-------+
```
This is random sample of 10 of our customers.

It looks like we have the name, the sex, the age in this table. Those demographics we understand right away. There is also a column ms_id which is a bit criptic. This column is a key to another table. This key is sometimes called  *FREIGN ID* or*FREIGN KEY*. 
To get the actual text that stands behind this key we need to conect those tables. This is done by the **JOIN** statement.
```sql
SELECT  * from bookstore.Customers JOIN bookstore.MaritalStatuses ON bookstore.Customers.ms_id = bookstore.MaritalStatuses.ms_id LIMIT 10;
```

```
+--------------------+-------------+----------------------------+-----+-----+-------+-------+---------------+
| customer_nm        | customer_id | customer_username_nm       | sex | age | ms_id | ms_id | ms_type       |
+--------------------+-------------+----------------------------+-----+-----+-------+-------+---------------+
| Donald Anthony PhD |           1 | natasha77@hutmail.con      | M   |  23 |     1 |     1 | Never married |
| Wayne Rasmussen    |           2 | mistygilbert@hutmail.con   | M   |  21 |     1 |     1 | Never married |
| Kathleen Webb      |           3 | nthompson@yupee.con        | F   |  32 |     2 |     2 | Married       |
| Meghan Webb        |           4 | sanchezkristen@hutmail.con | F   |  25 |     1 |     1 | Never married |
| Courtney Alexander |           5 | kathrynfrazier@shelf.con   | F   |  57 |     1 |     1 | Never married |
| Nicole Montoya     |           6 | rothjared@hutmail.con      | F   |  39 |     2 |     2 | Married       |
| Rebecca Wilkinson  |           7 | thomas80@hutmail.con       | F   |  76 |     2 |     2 | Married       |
| Manuel Flowers     |           8 | xprice@hutmail.con         | M   |  32 |     2 |     2 | Married       |
| Lisa Bennett       |           9 | kylesmith@gamail.con       | F   |  71 |     3 |     3 | Widow         |
| Erica Miller       |          10 | jessicageorge@yupee.con    | F   |  52 |     5 |     5 | Divorced      |
+--------------------+-------------+----------------------------+-----+-----+-------+-------+---------------+
```
It is clearly visible that we cannected those two tables by the column **ms_id**, now we can exclude those columns because they have no meaning for us. 
In addition we had to write the full name of those columns **bookstore.Customers.ms_id** which becomes quite long. Especialy when we have to specify all the names of all the columns we want.

In the next query we will remove those key columns.
```sql
SELECT
	bookstore.Customers.customer_nm, 
	bookstore.Customers.customer_id, 
	bookstore.Customers.customer_username_nm, 
	bookstore.Customers.sex, 
	bookstore.Customers.age, 
	bookstore.MaritalStatuses.ms_type
from bookstore.Customers
JOIN bookstore.MaritalStatuses 
ON bookstore.Customers.ms_id = bookstore.MaritalStatuses.ms_id 
LIMIT 10;
```
```
+--------------------+-------------+----------------------------+-----+-----+---------------+
| customer_nm        | customer_id | customer_username_nm       | sex | age | ms_type       |
+--------------------+-------------+----------------------------+-----+-----+---------------+
| Donald Anthony PhD |           1 | natasha77@hutmail.con      | M   |  23 | Never married |
| Wayne Rasmussen    |           2 | mistygilbert@hutmail.con   | M   |  21 | Never married |
| Kathleen Webb      |           3 | nthompson@yupee.con        | F   |  32 | Married       |
| Meghan Webb        |           4 | sanchezkristen@hutmail.con | F   |  25 | Never married |
| Courtney Alexander |           5 | kathrynfrazier@shelf.con   | F   |  57 | Never married |
| Nicole Montoya     |           6 | rothjared@hutmail.con      | F   |  39 | Married       |
| Rebecca Wilkinson  |           7 | thomas80@hutmail.con       | F   |  76 | Married       |
| Manuel Flowers     |           8 | xprice@hutmail.con         | M   |  32 | Married       |
| Lisa Bennett       |           9 | kylesmith@gamail.con       | F   |  71 | Widow         |
| Erica Miller       |          10 | jessicageorge@yupee.con    | F   |  52 | Divorced      |
+--------------------+-------------+----------------------------+-----+-----+---------------+
```


To simplify the query we can type only: 
```sql
SELECT
	cust.customer_nm, 
	cust.customer_id, 
	cust.customer_username_nm, 
	cust.sex, 
	cust.age, 
	ms.ms_type
from bookstore.Customers cust
JOIN bookstore.MaritalStatuses ms
ON cust.ms_id = ms.ms_id 
LIMIT 10;
```
This simplified query will return identical result. The short names **cust** and **ms** in the above query are called aliases and replace reference to  **bookstore.Customers** and **bookstore.MaritalStatuses**

```
+--------------------+-------------+--------------------------+-----+-----+---------------+
| customer_nm        | customer_id | customer_username_nm     | sex | age | ms_type       |
+--------------------+-------------+--------------------------+-----+-----+---------------+
| Patty Gonzales     |     1297169 | bodonnell@gamail.con     | F   |  33 | Never married |
| Rebekah Myers      |     1297170 | trevinomatthew@yupee.con | F   |  35 | Married       |
| Katherine Castillo |     1297171 | mccannlisa@gamail.con    | F   |  99 | Widow         |
| Michael Hamilton   |     1297172 | rpowell@gamail.con       | M   |  20 | Never married |
| Aimee Martin       |     1297173 | michaeljoseph@yupee.con  | F   |  57 | Married       |
| Johnathan Hall     |     1297174 | jamie97@hutmail.con      | M   |  33 | Never married |
| Erin Lewis         |     1297175 | nbailey@yupee.con        | F   |  27 | Never married |
| Ashley Nelson      |     1297176 | jamiegreen@shelf.con     | F   |  39 | Married       |
| Robert Wells       |     1297177 | aferrell@gamail.con      | M   |  32 | Never married |
| Samantha Herrera   |     1297178 | jasonmoran@gamail.con    | F   |  71 | Married       |
+--------------------+-------------+--------------------------+-----+-----+---------------+
```

This is sampple of individual users. We want to focus on the bigger picture.
#### **Q:** How many Customers are male and how many are female?

```sql
SELECT
	cust.sex, 
	count(*)
from bookstore.Customers cust
GROUP BY 1;
```
```
+-----+----------+
| sex | count(*) |
+-----+----------+
| F   |   127771 |
| M   |   118133 |
+-----+----------+
```
We can tell that the we have slightly more female customers, but not by much.

```sql
SELECT
	cust.age, 
	count(*)
from bookstore.Customers cust
GROUP BY 1
ORDER BY cust.age;
```
```
+-----+----------+
| age | count(*) |
+-----+----------+
|   8 |      591 |
|   9 |      691 |
|  10 |      584 |
|  11 |      813 |
|  12 |      873 |
|  13 |     1250 |
|  14 |     1399 |
|  15 |     1550 |
|  16 |     1847 |
|  17 |     2287 |
|  18 |     2288 |
|  19 |     2417 |
|  20 |     2807 |
|  21 |     2667 |
|  22 |     2911 |
|  23 |     3143 |
|  24 |     2928 |
|  25 |     3475 |
|  26 |     3745 |
|  27 |     3953 |
|  28 |     3607 |
|  29 |     4109 |
|  30 |     3564 |
|  31 |     4075 |
|  32 |     3744 |
|  33 |     4119 |
|  34 |     4162 |
|  35 |     4317 |
|  36 |     4197 |
|  37 |     4075 |
|  38 |     3646 |
|  39 |     3517 |
|  40 |     3527 |
|  41 |     3805 |
|  42 |     3827 |
|  43 |     3548 |
|  44 |     4083 |
|  45 |     4427 |
|  46 |     4489 |
|  47 |     4649 |
|  48 |     4659 |
|  49 |     4911 |
|  50 |     4829 |
|  51 |     4985 |
|  52 |     4829 |
|  53 |     5017 |
|  54 |     5164 |
|  55 |     5271 |
|  56 |     4945 |
|  57 |     5167 |
|  58 |     5321 |
|  59 |     4478 |
|  60 |     3788 |
|  61 |     4278 |
|  62 |     3955 |
|  63 |     3683 |
|  64 |     3770 |
|  65 |     3488 |
|  66 |     2766 |
|  67 |     2463 |
|  68 |     2220 |
|  69 |     2311 |
|  70 |     2305 |
|  71 |     2597 |
|  72 |     2792 |
|  73 |     2493 |
|  74 |     2886 |
|  75 |     2679 |
|  76 |     2356 |
|  77 |     1696 |
|  78 |     1361 |
|  79 |     1579 |
|  80 |     1397 |
|  81 |     1220 |
|  82 |     1153 |
|  83 |     1154 |
|  84 |      644 |
|  85 |      450 |
|  86 |      563 |
|  87 |      212 |
|  88 |      433 |
|  89 |      433 |
|  90 |      215 |
|  91 |      235 |
|  92 |      149 |
|  93 |      151 |
|  94 |      126 |
|  95 |      116 |
|  96 |      122 |
|  97 |      167 |
|  98 |      102 |
|  99 |       37 |
| 100 |       49 |
| 101 |       14 |
| 102 |       14 |
+-----+----------+
```
Quite long list isn't it. 

The most orders we have from the group of 49-58 years old.It is clear that our Customers are mostly between 25 and 65 years old with clear peak around age of 55. 
Altho the information is in this result it is not imediately visible.
We should focus on the age of the top customers with following query.

```sql 
SELECT
	cust.age, 
	count(*)
from bookstore.Customers cust
GROUP BY 1
ORDER BY 2 DESC LIMIT 10;
```

```
+-----+----------+
| age | count(*) |
+-----+----------+
|  58 |     5321 |
|  55 |     5271 |
|  57 |     5167 |
|  54 |     5164 |
|  53 |     5017 |
|  51 |     4985 |
|  56 |     4945 |
|  49 |     4911 |
|  50 |     4829 |
|  52 |     4829 |
+-----+----------+
```
The result looks more elegant.

The new construction 
```sql
ORDER BY 2 DESC
```
instructs the database to order the results descending as oppose to default ascending `ASC` order

#### **Q:** How are our customers disctriburted by maritial statsus ?
```sql 
SELECT
	ms.ms_type,
	count(*)
from bookstore.Customers cust
JOIN bookstore.MaritalStatuses ms
ON cust.ms_id = ms.ms_id 
GROUP BY ms.ms_type
LIMIT 10;
```
As you might noticed we have not **JOIN** the MaritalStatus untill now. This is because we only need this relations when we use a column from the joined (the foreign) table. We didn't need it untill now.
```
+---------------+----------+
| ms_type       | count(*) |
+---------------+----------+
| Divorced      |    26454 |
| Married       |   112979 |
| Never married |    89564 |
| Separated     |     3886 |
| Widow         |    13021 |
+---------------+----------+
```
As you can see the Customers are almost evenly distributed between single and married.

This conclude our demographics analisis.

Lets add customer behaveour to the picture we want to know more about their buying habits.
How much they buy?
How much they spend?
What do they buy?

#### Who are our best customers?

#### **Q:** Query Top Buyers

```sql
SELECT
    count(t.order_id) 'Total Orders',
    SUM(t.discounted_price) 'Spent in USD',
    cust.customer_nm 'Name',
    cust.sex,
    cust.age,
    ms.ms_type
FROM bookstore.Transactions as t 
INNER JOIN bookstore.Customers cust ON cust.customer_id = t.customer_id
INNER JOIN bookstore.MaritalStatuses ms ON ms.ms_id = cust.ms_id
GROUP BY  
    t.customer_id,
    cust.customer_nm,
    cust.sex,
    cust.age,
    ms.ms_type
ORDER BY 'Total Orders' desc
LIMIT 10;
```
The Result 
```
+--------------+--------------+--------------------+-----+-----+---------------+
| Total Orders | Spent in USD | Name               | sex | age | ms_type       |
+--------------+--------------+--------------------+-----+-----+---------------+
|           34 | 313.92       | Donald Anthony PhD | M   |  23 | Never married |
|           10 | 142.16       | Wayne Rasmussen    | M   |  21 | Never married |
|           21 | 155.02       | Kathleen Webb      | F   |  32 | Married       |
|           23 | 198.58       | Meghan Webb        | F   |  25 | Never married |
|           48 | 371.13       | Courtney Alexander | F   |  57 | Never married |
|            7 | 60.96        | Nicole Montoya     | F   |  39 | Married       |
|            9 | 49.66        | Rebecca Wilkinson  | F   |  76 | Married       |
|           23 | 238.16       | Manuel Flowers     | M   |  32 | Married       |
|           16 | 193.00       | Lisa Bennett       | F   |  71 | Widow         |
|           16 | 140.93       | Erica Miller       | F   |  52 | Divorced      |
+--------------+--------------+--------------------+-----+-----+---------------+
```
It looks like women buy more but the top spender are man after all.

#### **Q:**  But do the younger people read more than seniores ?
Lets find out.

```sql
SELECT
    count(t.order_id) 'Total Orders',
    cust.age
FROM bookstore.Transactions as t 
INNER JOIN bookstore.Customers cust ON cust.customer_id = t.customer_id
INNER JOIN bookstore.MaritalStatuses ms ON ms.ms_id = cust.ms_id
GROUP BY  
    cust.age
ORDER BY count(t.order_id) desc
LIMIT 10;
```
Result:
```
+--------------+-----+
| Total Orders | age |
+--------------+-----+
|        33374 |  55 |
|        33345 |  58 |
|        32656 |  57 |
|        32212 |  54 |
|        31249 |  51 |
|        31175 |  53 |
|        30923 |  49 |
|        30664 |  56 |
|        30307 |  52 |
|        30230 |  50 |
+--------------+-----+
```

The most orders we have from the group of 49-58 years old.

Now lets focus on our top Customers and try to profile them in order to answer the following question:

#### **Q:** What are the reading preferencess of our top customer?
```sql
SELECT 
    cust.customer_nm,
    b.category, 
    SUM(discounted_price) disc_price 
FROM bookstore.Transactions AS t  
INNER JOIN Customers cust ON cust.customer_id = t.customer_id 
INNER JOIN Books b ON b.book_id = t.book_id 
WHERE cust.customer_id=13
GROUP BY  cust.customer_id,cust.customer_nm,b.category ORDER BY cust.customer_id;
```

```
+----------------+-------------------+------------+
| customer_nm    | category          | disc_price |
+----------------+-------------------+------------+
| Joseph Griffin | Humor             | 14.80      |
| Joseph Griffin | Romance           | 33.37      |
| Joseph Griffin | Sci-fi            | 40.48      |
| Joseph Griffin | Horror            | 28.39      |
| Joseph Griffin | Drama             | 34.98      |
| Joseph Griffin | Children Classics | 45.76      |
| Joseph Griffin | Fantasy           | 60.27      |
| Joseph Griffin | Classics          | 38.49      |
+----------------+-------------------+------------+
```
Here are the reading preferencess of Joseph Griffin

Congratulations you completed our short tutorial.
