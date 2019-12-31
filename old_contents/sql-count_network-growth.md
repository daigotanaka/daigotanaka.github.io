## How to count the network growth with SQL

In the data science and analytics project for transactional data, the first step
often involves counting the number of transactions along the age of each account.
After writing many queries to make SQL view of the aggregated statistics, I saw
recurring patterns of the query. I also noticed that such query tends to get
complex. So it is worth making a template query so we don't make mistakes.

One complex pattern is to count the growth of the unique item ID per account
over the time. Think of a library system where the borrower loans books and
you want to see the growth of unique books each borrower borrowed. (If Jane
loaned the same book for the second time it does not count.)

![book borrower digram](https://raw.githubusercontent.com/daigotanaka/essays/master/images/book-borrower-diagram.png)

The raw record may look like this:

| borrowed_at | borrower_id | book_id |
| :----       | :----       | :----   |
| 2018-04-28 10:05:00 | 1 | 105 |
| 2018-04-28 11:15:00 | 2 | 106 |
| 2018-05-10:05:00:00 | 1 | 110 |
| 2018-05-11 10:05:00 | 3 | 105 |
| 2018-06-01 10:05:00 | 1 | 105 |
| ... | ... | ... |

And you want to produce a daily statistics like this:

| date  | borrower_id | unique_books_borrowed |
| :---- | :----       | :----                 |
| 2018-04-28 | 1 | 1 |
| 2018-04-28 | 2 | 1 |
| 2018-05-10 | 1 | 2 |
| 2018-05-11 | 3 | 1 |
| 2018-06-01 | 1 | 2 |

**Note:** Here, I am not producing the rows of the day when a borrower does
not have a transaction.

We can use window functions to produce the statistics. One difficulty doing this
in Redshift is that it does not support a window function for count(distinct).
So we need a trick.

The trick is to first create a list of the time stamps when the borrower loaned
the book for the first time. borrower_id=1 borrowed the book_id=105 at
2018-04-28 10:05:00 and 2018-06-01 10:05:00 and we want to keep 2018-04-28 10:05:00
only in such list. Then use the windows function to count the growth of the
new titles borrowed. Lastly, sort with the time stamp, truncate it as day,
then pick the max count per day just in case there are multiple transactions
by the same borrower in one day.

The query gets a bit long and it is complex enough for anybody to make small
mistakes each time he or she writes. So I wrote a template query that I can
tweak the first part to change the inputs and the last part to rename the
outcome columns.

Here is the query:

```
/*
General template for per-day rolling sum of unique item per account
To use, edit Sections 1 and 3 only to give the input and rename the
outcome column names
 */
-- 1. Convert the fields into the standardized inputs
WITH standard_input AS (
    SELECT
      borrowed_at
        -- keep the next line as is
        AS "date_time",
      borrower_id
        -- keep the next line as is
        AS account_id,
      book_id
        -- keep the next line as is
        AS item_id
    FROM library_transactions
    WHERE 1 = 1
          -- add filter conditions if needed
          AND item_id not in (1, 3, 10)
),
  -- 2. Compile the rolling sum (this part does not have to change)
  -- Redshift does not support a window function for count(distinct item)
  -- 2.1. Calculate the first date_time per account_id per item_id
    new_items_timeline AS (
      SELECT
        MIN(date_time) AS "date_time",
        account_id,
        item_id        AS new_item_id
      FROM standard_input
      GROUP BY 2, 3
  ),
  -- 2.2 Count the record to create rolling sum per account
    rolling_sum AS (
      SELECT
        date_time,
        account_id,
        COUNT(new_item_id)
        OVER (
          PARTITION BY account_id
          ORDER BY "date_time"
          ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW )
          AS unique_item_count
      FROM new_items_timeline
  ),
  -- 2.2 Make unique item count per account aggregated daily
    daily_aggregate AS (
      SELECT
        date_trunc('day', date_time) AS "date",
        account_id,
        max(unique_item_count)       AS unique_item_count
      FROM rolling_sum
      GROUP BY 1, 2
  )
-- 3. Rename the generic metrics according the use-case
SELECT
  "date",
  account_id        AS borrower_id,
  unique_item_count AS unique_books_borrowed
FROM daily_aggregate
ORDER BY 1, 2
```

As we hire more analysts at Anelen, I am going to make a collection of the
template like this so we can make less mistakes, share the practice with
co-workers and clients, and work very fast.
