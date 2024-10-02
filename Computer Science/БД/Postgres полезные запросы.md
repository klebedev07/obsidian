Топ 10 медленных запросов

```sql
SELECT
query, 
calls, 
total_time,
mean_time,
rows,
100.0 * shared_blks_hit / nullif(shared_blks_hit + shared_blks_read, 0) AS hit_percent, 
shared_blks_read, 
shared_blks_hit,
shared_blks_dirtied,
shared_blks_written,
local_blks_read, 
local_blks_hit,
local_blks_dirtied,
local_blks_written, 
temp_blks_read, 
temp_blks_written,
blk_read_time, 
blk_write_time 
FROM pg_stat_statements 
ORDER BY total_time DESC, blk_read_time DESC, blk_write_time DESC, calls DESC LIMIT 10;
```
