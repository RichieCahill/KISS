# datetime are bad

table with 770 million rows
the goal was to slect all the rows where a epoch value is day YYYY-MM-DD

## Original query

```sql
SELECT
    *
WHERE
    (DATE(1970-1-1)+(epoch/1000/60/60/24)) LIKE 'JAN-01-2020'
```

This query was slow took about 30 minutes to run
`date(1970-1-1)+(epoch/1000/60/60/24)` is run against every row in the table

## improved query

```sql
SELECT
    *
WHERE
    epoch < end_of_day
    AND epoch > start_of_day
```

This query was much faster took about 30 seconds to run
`epoch < end_of_day` and `epoch > start_of_day` are run against every row in the table
and `end_of_day` and `start_of_day` are calculated once and stored in a variable

If your want to really understand why this is faster go look into the execution plan and integer operations and why they are faster than almost all other operations.
