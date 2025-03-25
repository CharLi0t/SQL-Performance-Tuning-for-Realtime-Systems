# SQL-Performance-Tuning-for-Realtime-Systems

This query retrieves the index name and definition for the index 'idx_pcq_subroute_status_cuedate' 
on the 'parking_customer_queue' table. This can be useful for verifying the index structure and 
ensuring it is correctly defined for performance tuning purposes.
```sql
SELECT 
    indexname, 
    indexdef 
FROM pg_indexes 
WHERE tablename = 'parking_customer_queue' 
  AND indexname = 'idx_pcq_subroute_status_cuedate';
```

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
