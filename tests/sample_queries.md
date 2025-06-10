
# Sample NRQL in Markdown

```nrql
FROM
    Log
WITH
    numeric(http.statusCode) AS `sb.statusCode`,
    numeric(timespan) * 1000 AS `sb.duration`,
    capture(pageUrl, r'https://(?P<domain>[^/]+)/.+') AS `sb.domain`
SELECT
    average(`sb.duration`) AS 'Avg. Duration (s)'
WHERE
    entity.name = 'Sample Application' AND
    `sb.duration` > 0
FACET
    CASES(
        `sb.statusCode` < 400 AS 'Success',
        `sb.statusCode` < 500 AS 'Client Error',
        `sb.statusCode` < 600 AS 'Server Error'
    ) AS 'Status',
    `sb.domain` AS 'Domain'
TIMESERIES
    3 hours
SINCE
    '2024-10-01 00:00:00'
UNTIL
    '2024-11-01 00:00:00'
WITH TIMEZONE
    'America/LIMA'
COMPARE WITH
    1 month ago
```
