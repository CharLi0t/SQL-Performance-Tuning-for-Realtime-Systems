# SQL-Performance-Tuning-for-Realtime-Systems

This SQL query is used to retrieve the name and definition of a specific index on a given table in a PostgreSQL database. It is useful for verifying if the index is correctly defined to support query performance tuning.
```sql
SELECT 
    indexname, 
    indexdef 
FROM pg_indexes 
WHERE tablename = '<your_table_name>' 
  AND indexname = '<your_index_name>';
```

🔍 Usage Tips:

- Replace <your_table_name> with the name of the table you want to inspect.
- Replace <your_index_name> with the name of the index you are checking.
- Useful to check if the index:
    - Is a covering index (includes columns needed by queries)
    - Is a partial index (applies to a subset of data)
    - Uses functional expressions (e.g., indexing on LOWER(column))
    - Matches query WHERE clauses for effective index usage

---

This query retrieves a list of active queries from the PostgreSQL statistics collector,
showing details such as process ID (pid), username (usename), database name (datname),
client address (client_addr), query state (state), query start time (query_start), 
execution time in milliseconds (exec_time_ms), and the query text (query).
The results are ordered by the query start time in descending order (newest queries first).
This can be useful for monitoring active queries and identifying long-running queries.
```sql
SELECT pid, 
       usename, 
       datname, 
       client_addr, 
       state, 
       query_start, 
       ROUND(EXTRACT(EPOCH FROM now() - query_start) * 1000) AS exec_time_ms, 
       query
FROM pg_stat_activity
WHERE state != 'idle'
ORDER BY query_start DESC;
```
