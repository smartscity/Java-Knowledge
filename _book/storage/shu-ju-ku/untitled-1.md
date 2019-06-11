# Untitled

### About Time Function

* toString\(\)
* toUnixTimestamp\(\)
* toFixedString\(\)
* toStringCutToZero\(\)
* CAST\(\)

```text
SELECT
    now() AS now_local,
    toString(now(), 'Asia/Yekaterinburg') AS now_yekat
    
toUnixTimestamp('2000-01-01 00:00:00', 'Asia/Yekaterinburg')

SELECT toFixedString('foo\0bar', 8) AS s, toStringCutToZero(s) AS s_cut

SELECT
    '2016-06-15 23:00:00' AS timestamp,
    CAST(timestamp AS DateTime) AS datetime,
    CAST(timestamp AS Date) AS date,
    CAST(timestamp, 'String') AS string,
    CAST(timestamp, 'FixedString(22)') AS fixed_string        
```





[https://clickhouse-docs.readthedocs.io/en/latest/functions/type\_conversion\_functions.html](https://clickhouse-docs.readthedocs.io/en/latest/functions/type_conversion_functions.html)

