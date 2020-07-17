#!/bin/sh
INTERVAL=5
PREFIX=$INTERVAL-sec-status
RUNFILE=/home/benchmarks/running
mysql -e 'SHOW GLOBAL VARIABLES' >> mysql-variables
while test -e $RUNFILE; do  
 file=$(date +%F_%I)   
    sleep=$(date +%s.%N | awk "{print $INTERVAL - (\$1 % $INTERVAL)}")  
 sleep $sleep   
    ts="$(date +"TS %s.%N %F %T")"  
 loadavg="$(uptime)"   
    echo "$ts $loadavg" >> $PREFIX-${file}-status   
    mysql -e 'SHOW GLOBAL STATUS' >> $PREFIX-${file}-status &   
    echo "$ts $loadavg" >> $PREFIX-${file}-innodbstatus   
    mysql -e 'SHOW ENGINE INNODB STATUS\G' >> $PREFIX-${file}-innodbstatus &   
    echo "$ts $loadavg" >> $PREFIX-${file}-processlist   
    mysql -e 'SHOW FULL PROCESSLIST\G' >> $PREFIX-${file}-processlist &   
    echo $ts
done
echo Exiting because \$RUNFILE does not exist.

whether a server is doing all of its work optimally, to find out why a specific query is not executing quickly enough, and to troubleshoot mysterious intermittent incidents(stalls, pileups, freezes)

# Chapter 3: Profiling Server Performance

Our definition is that performance is measured by the time required to complete a task. In other words, performance is response time.
A database server’s performance is measured by query response time, and the unit of measurement is time per query.
If you thought that performance optimization was about improving queries per second, then you were thinking about throughput optimization.
Optimizing queries makes it possible for the server to execute more queries per second, because each one requires less time to execute when the server is optimized. (The unit of throughput is queries per time, which is the inverse of our definition of performance.)

So if the goal is to reduce response time, we need to understand why the server requires a certain amount of time to respond to a query, and reduce or eliminate whatever unnecessary work it’s doing to achieve the result. you cannot reliably optimize what you cannot measure.

In contrast, we aim to spend most of our time—perhaps upwards of 90%—measuring where the response time is spent. If we don’t find the answer, we might not have measured correctly or completely.

Execution-time profiling shows which tasks consume the most time, whereas wait analysis shows where tasks get stuck or blocked the most.

It’s especially helpful to have more information on the response times, such as histograms, percentiles, the standard deviation, and the index of dispersion.

The first tactic is watching SHOW FULL PROCESSLIST repeatedly with the --processlist option, noting when queries first appear and when they disappear. This is a sufficiently accurate method for some purposes, but it can’t capture all queries. Very short-lived queries can sneak in and finish before the tool can observe them.

Another bit of extra detail here is the variance-to-mean ratio, in the V/M column. This is also known as the index of dispersion. Queries with a higher index of dispersion have a more variable execution-time profile, and highly variable queries are generally good candidates for optimization.

## Show profile

mysql> SET profiling = 1;

mysql> SET @query*id = 1;Query OK,
0 rows affected (0.00 sec)
mysql> SELECT STATE, SUM(DURATION) AS Total_R,  
-> ROUND(  
-> 100 * SUM(DURATION) /  
-> (SELECT SUM(DURATION)  
-> FROM INFORMATION*SCHEMA.PROFILING  
-> WHERE QUERY_ID = @query_id  
-> ), 2) AS Pct_R,  
-> COUNT(*) AS Calls,  
-> SUM(DURATION) / COUNT(\*) AS "R/Call"  
-> FROM INFORMATION_SCHEMA.PROFILING  
-> WHERE QUERY_ID = @query_id  
-> GROUP BY STATE  
-> ORDER BY Total_R DESC;

## Using performance schema

mysql> SELECT event_name, count_star, sum_timer_wait  
-> FROM events_waits_summary_global_by_event_name  
-> ORDER BY sum_timer_wait DESC LIMIT 5;

Diagnosing Intermittent Problems
InnoDB scalability limitations were causing query plan optimization to take too long when concurrency was over some threshold.
Try to determine whether the problem is with a single isolated query, or if it's server-wide.
This is good news and bad news: good because you’re much less likely to hit them, and bad because they require more knowledge of MySQL internals to diagnose. It also means that a lot of problems can be solved by simply upgrading MySQL.

## Show Global Status: threads_connected, threads_running, query per second.

Two common cases. One is some kind of internal bottleneck in the server, causing new queries to begin executing but to pile up against some lock that the older queries are waiting to acquire. This type of lock usaully puts back-pressure on the application servers and causes some queueing there, too.

## Show processlist

State: sending data
State: freeing items
State: NULL
State: end
State: Updating
State: Cleaning up
State: Update
State: Sorting result
State: logging slow query
The most characteristic and reliable indicator of a problem was a high number of queries in the “freeing items” state. A spike of unusual thread states in show processlist is another good indicator.

## Using query logging

To find problems in the query log, turn on the slow query log and set logn_query_time to 0 globally, and make sure that all of the connections see the new setting.
mysql -e 'SHOW PROCESSLIST\G' | grep -c "State: freeing items"

Watch the status variables once per second, and if Threads_running exceeds 20 for more than 5 seconds, start gathering diagnostic data.

Execution time is spent doing work or waiting, as you’ll recall. When an unknown problem happens, there are two types of causes, broadly speaking. The server could be doing a lot of work—consuming a lot of CPU cycles—or it could be stuck waiting for resources to become free.

It’s a good idea not only to try to understand how the server behaves, but also to take an inventory of the server’s status, configuration, software, and hardware.

The queries weren’t perfect, but they were still running in less than 10 ms most of the time. So we confirmed that the server was fine under normal circumstances. (This is important to do; many problems that are noticed only sporadically are actually symptoms of chronic problems, such as failed hard drives in RAID arrays.)

What Causes Poor Performance? When a resource is behaving badly, it’s good to try to understand why. There are a few possibilities:The resource is being overworked and doesn’t have the capacity to behave well.The resource is not configured properly.The resource is broken or malfunctioning.

mysql> SHOW TABLES FROM INFORMATION_SCHEMA LIKE '%\_STATISTICS';
You can find the most-used and least-used tables and indexes, by reads, updates, or both.You can find unused indexes, which are candidates for removal.You can look at the CONNECTED_TIME versus the BUSY_TIME of the replication user to see whether replication will likely have a hard time keeping up soon.

- We think that the most useful way to define performance is in terms of response time.
- You cannot reliably improve what you cannot measure, so performance improvement works best with high-quality, well-scoped, complete measurements of response time.
- The best place to start measuring is the application, not the database. If there is a problem in the lower layers, such as the database, good measurements will make it obvious.
- Most systems are impossible to measure completely, and measurements are always wrong. But you can usually work around the limitations and get a good outcome anyway, if you acknowledge the imperfection and uncertainty of the process you use.
- Thorough measurements produce way too much data to analyze, so you need a profile. This is the best tool to help you bubble important things to the top so you can decide where to start.
- A profile is a summary, which obscures and discards details. It also usually doesn’t show you what’s missing. It’s unwise to take a profile at face value.
- There are two kinds of time consumption: working and waiting. Many profiling tools can only measure work done, so wait analysis is sometimes an important supplement, especially when CPU usage is low but things still aren’t getting done.
- Optimization is not the same thing as improvement. Stop working when further improvement is not worth the cost.
- Pay attention to your intuition, but try to use it to direct your analysis, not to decide on changes to the system. Base your decisions on data, not gut feeling, as much as possible.

# Chapter 4. Optimizing Schema and Data Types

For example, a denormalized schema can speed up some types of queries but slow down others. Adding counter and summary tables is a great way to optimize queries, but they can be expensive to maintain.

It’s a chapter on MySQL database design—it’s about what is different when designing databases with MySQL rather than other relational database management systems.

MySQL likes simplicity, and so will the people who have to work with your database:

- Try to avoid extremes in your design, such as a schema that will force enormously complex queries, or tables with oodles and oodles of columns.
- Use small, simple, appropriate data types, and avoid NULL unless it’s actually the right way to model your data’s reality.
- Try to use the same data types to store similar or related values, especially if they’ll be used in a join condition.
- Watch out for variable-length strings, which might cause pessimistic full-length memory allocation for temporary tables and orting.
- Try to use integers for identifiers if you can.
- Avoid the legacy MySQL-isms such as specifying precisions for floating-point numbers or display widths for integers.
- Be careful with ENUM and SET. They’re handy, but they can be abused, and they’re tricky sometimes. BIT is best avoided.

Normalization is good, but denormalization (duplication of data, in most cases) is sometimes actually necessary and beneficial. We’ll see more examples of that in the next chapter. And precomputing, caching, or generating summary tables can also be a big win.

Finally, ALTER TABLE can be painful because in most cases, it locks and rebuilds the whole table. We showed a number of workarounds for specific cases; for the general case, you’ll have to use other techniques, such as performing the ALTER on a replica and then promoting it to master.

# Chapter 5. Indexing for High Performance

Index optimization is perhaps the most powerful way to improve query performance. Indexes can improce performance by many orsers of magnitude, and optimal indexes can sometimes boost performance about two orders of magnitude more than indexes that are merely "good".

An index contains values from one or more columns in a table. If you index more than one column, the column order is very important, because MySQL can only search efficiently on a leftmost prefix of the index.

## B-tree indexes

## Hash indexes

## Building your own indexes

The idea is simple: create a pseudohash index on top of a standard B-Tree index. It will not be exactly the same thing as a real hash index, because it will still use the B-Tree index for lookups. However, it will use the keys’ hash values for lookups, instead of the keys themselves. All you need to do is specify the hash function manually in the query’s WHERE clause.

If you use this approach, you should not use SHA1() or MD5() hash functions. These return very long strings, which waste a lot of space and result in slower comparisons. They are cryptographically strong functions designed to virtually eliminate collisions, which is not your goal here. Simple hash functions can offer acceptable collision rates with better performance.

## Benefits of indexes

B-Tree indexes, which are the most common type you’ll use, function by storing the data in sorted order, and MySQL can exploit that for queries with clauses such as ORDER BY and GROUP BY. Because the data is presorted, a B-Tree index also stores related values close together. Finally, the index actually stores a copy of the values, so some queries can be satisfied from the index alone. Three main benefits proceed from these properties:Indexes reduce the amount of data the server has to examine.Indexes help the server avoid sorting and temporary tables.Indexes turn random I/O into sequential I/O.

For enormous tables, the overhead of indexing, as well as the work required to actually use the indexes, can start to add up. In such cases you might need to choose a technique that identifies groups of rows that are interesting to the query, instead of individual rows. You can use partitioning for this purpose.At the scale of terabytes, locating individual rows doesn’t make sense; indexes are replaced by per-block metadata.

## Indexing strategies for high performance

### Isolating the column

### Prefix indexes and index selectivity

The trick is to choose a prefix that’s long enough to give good selectivity, but short enough to save space. The prefix should be long enough to make the index nearly as useful as it would be if you’d indexed the whole column. In other words, you’d like the prefix’s cardinality to be close to the full column’s cardinality.

Another way to calculate a good prefix length is by computing the full column’s selectivity and trying to make the prefix’s selectivity close to that value. Here’s how to find the full column’s selectivity:
SELECT COUNT(DISTINCT city)/COUNT(\*) FROM sakila.city_demo;

Prefix indexes can be a great way to make indexes smaller and faster, but they have downsides too: MySQL cannot use prefix indexes for ORDER BY or GROUP BY queries, nor can it use them as covering indexes.

### Multicolumn indexes

There is an old rule of thumb for choosing column order: place the most selective columns first in the index. How useful is this suggestion? It can be helpful in some cases, but it’s usually much less important than avoiding random I/O and sorting, all things considered.

### Clustered indexes

Insert speeds depend heavily on insertion order. Inserting rows in primary key order is the fastest way to load data into an InnoDB table. It might be a good idea to reorganize the table with OPTIMIZE TABLE after loading a lot of data if you didn’t load the rows in primary key order.

If your server version doesn’t support innodb_autoinc_lock_mode, you can upgrade to a newer version of InnoDB that will perform better for this specific workload.

### Indexes and Locking

InnoDB can lock rows it doesn’t really need even when it uses an index. The problem is even worse when it can’t use an index to find and lock the rows: if there’s no index for the query, MySQL will do a full table scan and lock every row, whether it “needs” it or not.Here’s a little-known detail about InnoDB, indexes, and locking: InnoDB can place shared (read) locks on secondary indexes, but exclusive (write) locks require access to the primary key. That eliminates the possibility of using a covering index and can make SELECT FOR UPDATE much slower than LOCK IN SHARE MODE or a nonlocking query.

## Index and Table maintenance

Corrupted indexes can cause queries to return incorrect results, raise duplicate-key errors when there is no duplicated value, or even cause lockups and crashes. If you experience odd behavior—such as an error that you think shouldn’t be happening—run CHECK TABLE to see if the table is corrupt.

If the statistics were never generated, or if they are out of date, the optimizer can make bad decisions. The solution is to run ANALYZE TABLE, which regenerates the statistics.

You can examine the cardinality of your indexes with the SHOW INDEX FROM command. You can also get this data from the INFORMATION_SCHEMA.STATISTICS table in MySQL 5.0 and newer
InnoDB also calculates statistics for queries against some INFORMATION_SCHEMA tables, SHOW TABLE STATUS and SHOW INDEX queries.
There will also be index statistics persistence in MySQL 5.6, controlled by the innodb_analyze_is_persistent option.

If you configure your server not to update index statistics automatically, you need to do it manually with periodic ANALYZE TABLE commands, unless you know that the statistics won’t change in ways that will create bad query plans.

## Reducing index and data fragmentation

For storage engines that don’t support OPTIMIZE TABLE, you can rebuild the table with a no-op ALTER TABLE. Just alter the table to have the same engine it currently uses:
mysql> ALTER TABLE <table> ENGINE=<engine>;

How do you know whether your schema is indexed well enough? As always, we suggest that you frame the question in terms of response time. Find queries that are either taking too long or contributing too much load to the server. Examine the schema, SQL, and index structures for the queries that need attention. Determine whether the query has to examine too many rows, perform post-retrieval sorting or use temporary tables, access data with random I/O, or look up full rows from the table to retrieve columns not included in the index.

# Chapter 6. Query Performance Optimization

In the previous chapters we explained schema optimization and indexing, which are necessary for high performance. But they aren’t enough—you also need to design your queries well. If your queries are bad, even the best-designed schema and indexes will not perform well.
