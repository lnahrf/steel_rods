---
title: "Mastering Postgres Debugging: must-know queries for database troubleshooting"
datePublished: Mon Nov 20 2023 07:42:10 GMT+0000 (Coordinated Universal Time)
cuid: clp6lmnzk000109l2b3zt8elh
slug: mastering-postgres-debugging-must-know-queries-for-database-troubleshooting
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/BxHnbYyNfTg/upload/9b97857b22daf3beca567f129dbfd407.jpeg
tags: postgresql, databases, debugging, troubleshooting

---

I have been through several different roles in my career as a software engineer. In one of them, I had the pleasure of working on a very large corporate Postgres database. Most of my job consisted of optimizing queries, indexes and database flows.

The system was a hot mess when I first started working on it, It took the team about a year to stabilize it, and even then it was not as stable as they would want it to be.

Who is at fault for the awful system we had to work on for a whole year? Not Postgres! The system had a lot of anti-patterns and horrible architectural decisions implemented, these inevitably pushed Postgres and other systems on the cluster to their limits.

Vertical scaling was not an option, we could not spend more than *10k* USD per month just to run the production database. And with a dozen systems directly querying our database we had no choice but to start digging into performance stats and system queries to find out - *what on earth is making our database SO SLOW?!*

We ruled out specific queries and users, badly configured indexes, bad trigger logic, or even replications. After iterating all possibilities over and over again, we found out that we had a problem with the automatic vacuum process (that is a whole other article). Either way, making it operational again resolved most of our Postgres performance issues.

![](https://media.tenor.com/iV__D-FgJQQAAAAC/good-fine.gif align="center")

As time went on and the system stabilized, I researched and wrote down the most useful queries I had to use (and re-use, and re-use) to troubleshoot such a large database. I have decided to write them here, share the knowledge, and make your time with Postgres a little bit easier.

### #1 Search for a column or variable name in a function's source code

This one is so simple yet amazingly useful.

In a large database, with hundreds of triggers and functions firing at every action - it is almost impossible to manually search which trigger might affect or use a specific column or variable. Using this simple query, we can quickly find out which trigger is the culprit for an unwanted update or alteration.

```sql
SELECT 
proname,
prosrc 
FROM pg_proc 
WHERE prosrc LIKE '%variable_name%';
```

### #2 Find which action triggers a specific function, on which table

Doubling down on the last query, we can use the following to tell us exactly which action triggers a function - on which table. All we have to do is replace 'name' with our function name, and Postgres will tell us which table it is and whether it's after or before the update, insert or delete.

```sql
SELECT 
trigger_schema AS schema,
event_object_table AS table,
trigger_name,
event_manipulation,
action_timing, 
action_statement 
FROM information_schema.triggers
WHERE trigger_name 
LIKE '%name%'
```

### #3 Query statistics: identify slow queries, their total execution time, and filter by users

This one needs to be considered in advance because it requires some configuration. To enable Postgres statistics you must first run the following query as superadmin.

```sql
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
```

After that, you have to edit your *postgresql.conf* file, located in:

```plaintext
Windows: C:\Program Files\PostgreSQL\version\data
Linux: /etc/postgresql/version/main
```

If you are not sure where the configuration file is, you can run the following query and ask Postgres itself where it stores the configuration file.

```sql
SHOW config_file;
```

After you have located the file, search for the following line and uncomment it.

```sql
shared_preload_libraries = ''
```

Add *pg\_stat\_statements* to *shared\_preload\_libraries* and additionally add the following lines underneath it.

```sql
-- postgresql.conf
shared_preload_libraries = 'pg_stat_statements'
pg_stat_statements.track = all
track_activity_query_size = 2048 
-- track_activity_query_size defaults to 1024 (1kB),
-- the minimum is 100 (100B), maximum is 1048576 (1MB)
```

Save the file and reload your Postgres service. Now Postgres should start collecting query statistics. *To periodically reset the statistics collected I recommend setting up a weekly or monthly job that executes the clearing query.*

```sql
SELECT pg_stat_statements_reset(); 
-- OR PERFORM from within a Postgres function
```

Finally, the query I use to view query statistics.

```sql
SELECT
pguser.usename,
stats.mean_exec_time,
stats.min_exec_time,
stats.max_exec_time,
stats.calls,
interval '1 millisecond' * stats.total_exec_time AS exec_time,
interval '1 millisecond' * (stats.blk_read_time + stats.blk_write_time) AS io_time,
query
FROM pg_stat_statements stats
LEFT JOIN pg_user pguser ON pguser.usesysid = stats.userid
ORDER BY exec_time DESC LIMIT 100;
```

You can also add a filter to find the slowest queries of a specific Postgres user.

```sql
SELECT
pguser.usename,
stats.mean_exec_time,
stats.min_exec_time,
stats.max_exec_time,
stats.calls,
interval '1 millisecond' * stats.total_exec_time AS exec_time,
interval '1 millisecond' * (stats.blk_read_time + stats.blk_write_time) AS io_time,
query
FROM pg_stat_statements stats
LEFT JOIN pg_user pguser ON pguser.usesysid = stats.userid
WHERE pguser.usename = 'username'
ORDER BY exec_time DESC LIMIT 100;
```

### #4 Display current connections and the most recent operation

This one is most useful if you don't have access to a GUI Postgres client (such as PGAdmin). Use this query to view the current connections and their status, their latest operation (or query) and whether they are blocked by or waiting for a certain event. This can also be useful if you want to kill locked processes by their process ID.

```sql
SELECT 
pid,
usename AS username,
application_name,
client_addr AS ip,
client_port AS port,
backend_start AS operation_start,
backend_type AS operation,
wait_event AS waiting_for,
state_change,
state,
query
FROM pg_stat_activity;
```

### #5 Display least used indexes (including primary keys)

This one is a classic - great for when you are trying to optimize database performance. It will display which indexes were never used for scanning.

It is important to manage and optimize your indexes, having unused indexes may create bloat and slow down your database's performance significantly.

```sql
SELECT
	schemaname AS schema,
	relname AS table,
	indexrelname AS relation,
	last_idx_scan,
	indisunique AS is_unique,
    idx_scan
FROM pg_stat_user_indexes indexes
LEFT JOIN pg_index ON pg_index.indexrelid = indexes.indexrelid
ORDER BY
    idx_scan DESC;
```

### #6 Display index size on disk (heaviest indexes)

Another great query for index optimization is the following query, allowing you to view the heaviest indexes on disk (displayed in a nice format).

```sql
SELECT 
schemaname,
tablename,
indexname,
pg_size_pretty(pg_total_relation_size(CONCAT(schemaname, '.', indexname))) 
FROM pg_indexes
ORDER BY pg_size_pretty DESC;
```

### #7 Display table size on disk (heaviest tables)

Same as the previous query, but for tables - displays which table weighs the most on disk (might be an indication for tables that need a vacuum).

```sql
SELECT
    table_schema AS schema,
	table_name AS table,
	 pg_size_pretty(pg_total_relation_size(CONCAT(table_schema, '.', table_name))) AS size
FROM information_schema.tables
ORDER BY
    pg_total_relation_size(CONCAT(table_schema, '.', table_name)) DESC;
```

### #8 Display tables requiring a vacuum (most dead tuples on a table)

Got dead tuples? use the following query to find out which table needs a vacuum, or which table hasn't been cleaned in a while. *Remember that having a lot of dead tuples on any table will slow down your database.*

```sql
SELECT 
relname,
last_vacuum,
last_autovacuum,
vacuum_count,
autovacuum_count,
n_dead_tup
FROM pg_stat_user_tables 
ORDER BY n_dead_tup DESC;
```

Keep this article in your browser bookmarks and come back to it every time you need one of these helpful queries.

Happy Postgres troubleshooting!