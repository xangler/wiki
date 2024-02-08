# Clickhouse

## clickhouse 使用
```bash
clickhouse-client -u admin --password xxx
# describe databases → tables;
```

## 聚合 demo
```
with 
op_count as (
  SELECT
      op_time,
      sum(count) AS op_total,
      sumIf(count, status_code!=200 or error_code!=0) AS op_error,
      sumIf(count, error_code=1) AS op_special
  FROM usage
  
  WHERE
      op_time >= toDateTime(1697184370 - 60*60) AND op_time <= toDateTime(1697187970)
      AND account_id in [1]
      AND (endsWith(url, 'demo') OR endsWith(url, 'xemo'))

  GROUP BY op_time
  ORDER BY op_time
),
agg_count as(
  SELECT 
    op_time,
    sum(op_total) OVER (ORDER BY op_time RANGE BETWEEN 60*60 PRECEDING AND CURRENT ROW) as agg_total,
    sum(op_error) OVER (ORDER BY op_time RANGE BETWEEN 60*60 PRECEDING AND CURRENT ROW) as agg_error,
    sum(op_special) OVER (ORDER BY op_time RANGE BETWEEN 60*60 PRECEDING AND CURRENT ROW) as agg_special
  FROM op_count
)
SELECT 
    (intDiv(toUInt32(op_time), 60) * 60) * 1000 as t,
    agg_error/agg_total * 100 as error,
    agg_special/agg_total * 100 as special
FROM agg_count
WHERE
    op_time >= toDateTime(1697184370) AND op_time <= toDateTime(1697187970)
```