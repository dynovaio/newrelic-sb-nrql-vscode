```nrql
// Single line comments

-- single line comment

/*
 * Multi-line comments
 */
```

```nrql
show event types
```

```nrql
SHOW EVENT TYPES
```

SHOW EVENT TYPES /* sample intermediate
multiline comment */ SINCE 1 day ago UNTIL NOW

SHOW EVENT TYPES SINCE 1 week ago UNTIL NOW

SHOW EVENT TYPES SINCE yesterday UNTIL NOW

SHOW EVENT TYPES SINCE yesterday AT '12:00' UNTIL NOW

SHOW EVENT TYPES SINCE yesterday AT '12:00 AM' UNTIL NOW

SHOW EVENT TYPES SINCE yesterday AT '12:00 PM' UNTIL NOW

SHOW EVENT TYPES SINCE '2025-01-01 00:00:00.000' UNTIL NOW

SHOW EVENT TYPES SINCE '2025-01-01 00:00:00.000' UNTIL 1 hour ago

SHOW EVENT TYPES SINCE '2025-01-01 00:00:00.000' UNTIL 1 week ago

SHOW EVENT TYPES SINCE '2025-01-01 00:00:00.000' UNTIL today AT '12:00'

SHOW EVENT TYPES SINCE '2025-01-01 00:00:00.000' UNTIL today AT '12:00 AM'

SHOW EVENT TYPES SINCE '2025-01-01 00:00:00.000' UNTIL today AT '12:00 PM'

SHOW EVENT TYPES SINCE '2025-01-01 00:00:00.000' UNTIL '2025-02-01 00:00:00.000'

SHOW
    EVENT TYPES
SINCE
    '2025-01-01 00:00:00.000'
UNTIL
    '2025-02-01 00:00:00.000'

SHOW EVENT TYPES SINCE 123123123123123123 UNTIL 123456789123456789

SHOW EVENT TYPES SINCE NOW UNTIL tomorrow

DELETE FROM AgentUpdate WHERE a = 'b'

DELETE FROM Metric  WHERE metricName = 'newrelic.goldenmetrics.infra.kubernetes_pod.podScheduled'

DELETE FROM Span WHERE appName = 'external-usage-consumer (test-odd-wire)'

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

FROM EventType WITH function(attribute) AS var SELECT var

FROM Event [INNER|LEFT] JOIN (SELECT... FROM...) ON [key =] key SELECT ...

FROM Event SELECT attribute, name WHERE attribute IS NOT NULL

FROM Event SELECT average(attribute) FACET name

FROM foo SELECT * WHERE bar LIKE 'B'

FROM Log SELECT count(*) FACET aparse(string, '%"itemId":"*"%')

FROM Log SELECT count(*) FACET if(level_name = 'ERROR', 'ERROR', 'NOT_ERROR')

FROM Log SELECT count(*) FACET if(level_name = 'INFO' OR level_name = 'WARNING', 'NOT_ERROR', 'ERROR')

FROM
    Log
SELECT
    count(*)
WHERE
    hostname IN (
        FROM
            lookup(myHosts)
        SELECT
            uniques(myHost)
    )

FROM Log WITH aparse(message, '%itemId":"*","unitPrice":*}%') AS (itemId, unitPrice) SELECT itemId, unitPrice

FROM Log WITH aparse(message, '%itemId":"*","unitPrice":*}%') AS (itemId, unitPrice),   numeric(unitPrice) AS unitPriceNum SELECT sum(unitPriceNum) FACET itemId WHERE unitPriceNum < 100

FROM Log WITH aparse(string, 'POST: * body: {"itemId":"*","unitPrice":*}\n') AS (url, itemId, unitPrice) SELECT url, itemId, unitPrice

FROM Log WITH blob(`newrelic.ext.message`) AS encodedBlob, decode(encodedBlob, 'base64') AS decodedBlob SELECT encodedBlob, decodedBlob WHERE newrelic.ext.message IS NOT NULL LIMIT 10

FROM Log WITH capture(message, r'POST to carts: (?P<URL>.*) body: {"itemId":"(?P<UUID>.*)","unitPrice":(?P<unitPrice>.*)}.*')   AS (URL, UUID, unitPrice) SELECT URL, UUID, unitPrice WHERE URL IS NOT NULL

FROM Metric  SELECT average(convert(apm.mobile.external.duration, unit, 's'))  WHERE appName = 'my-application'

FROM Metric SELECT count(node_filesystem_size)  TIMESERIES FACET dimensions()

FROM NodeStatus SELECT uniques(tuple(index, cellName), 5)

FROM NrConsumption SELECT uniques(hostId, 10000)  SINCE 1 day AGO FACET mod(hostId, 10)

FROM PageAction LEFT JOIN  (   FROM PageView    SELECT session, browserTransactionName    LIMIT MAX )  ON session SELECT count(*) FACET session, currentUrl, browserTransactionName LIMIT MAX

FROM PageAction SELECT latest(lower(actionName)) WHERE lower(actionName) = lower('acmePageRenderedEvent') OR lower(actionName) = lower('SubmitLogin') FACET concat(actionName, ':', lower(actionName))

FROM PageAction SELECT latest(upper(actionName)) WHERE upper(actionName) = upper('acmePageRenderedEvent') OR upper(actionName) = upper('SubmitLogin') FACET concat(actionName, ':', upper(actionName))

FROM PageView  SELECT concat('Backend Duration: ', backendDuration, ', Network Duration: ', networkDuration, precision: 2)

FROM PageView JOIN  (   FROM PageAction    SELECT count(*)    FACET session, currentUrl )  ON session SELECT count(*) FACET browserTransactionName, currentUrl

FROM PageView JOIN  (   FROM PageAction    SELECT percentile(timeSinceLoad, 95, 99.5) AS pctl   FACET session, currentUrl )  ON session SELECT latest(getField(pctl, '95.0')) AS `95th`,    latest(getField(pctl, '99.5')) AS `99.5th` FACET browserTransactionName, currentUrl

FROM PageView JOIN (FROM PageAction SELECT median(timeSinceLoad) FACET session, currentUrl) ON session SELECT latest(getField(median, '50.0')) AS median FACET browserTransactionName, currentUrl

FROM PageView LEFT JOIN  (   FROM PageAction    SELECT count(*)    FACET session, currentUrl )  ON session SELECT count(*) FACET browserTransactionName, currentUrl

FROM PageView SELECT aparse(browserTransactionName, 'website.com/*')

FROM PageView SELECT average(connectionSetupDuration)  FACET concat(city, ', ', regionCode, ' ', countryCode)  WHERE countryCode IN ('US', 'CA')

FROM PageView SELECT cdfPercentage(firstPaint, 0.5, 1.0)

FROM PageView SELECT count(*)  FACET string(tuple(asnLatitude, asnLongitude), precision: 2)

FROM PageView SELECT session, encode(session, 'base64')

FROM PageView SELECT session, substring(session, 0, 3) AS First3,   substring(session, 3) AS After3rd,   substring(session, -3) AS Last3

FROM PageView SELECT string(average(duration), precision: 2)

FROM PageView SELECT string(duration, precision: 2)

FROM PageView SELECT sum(getCdfCount(firstPaint, 1.0))

FROM PageView WITH position(pageUrl, ':') AS FirstColon,   position(pageUrl, '/', 1) + 1 AS DomainBegin,    position(pageUrl, '/', 2) AS DomainEnd,    DomainEnd - DomainBegin AS DomainLength SELECT pageUrl, FirstColon, substring(pageUrl, 0, FirstColon) AS Protocol,   DomainBegin, DomainEnd, DomainLength, substring(pageUrl, DomainBegin, DomainLength) AS Domain,   position(pageUrl, '/', -1) AS LastSlash,    substring(pageUrl, position(pageUrl, '/', -1)) AS PathEnd

FROM Product SELECT convert(sum(itemWeight), 'grams', 'lbs')

FROM Span SELECT count(*)  WHERE entity.guid IS NOT NULL  AND decode(entity.guid, 'base64') NOT LIKE '%APM%'

FROM Span SELECT count(*)  WHERE entity.guid IS NOT NULL  FACET entity.guid, decode(entity.guid, 'base64')

FROM Span SELECT entity.guid, decode(entity.guid, 'base64')  WHERE entity.guid IS NOT NULL

FROM SyntheticRequest SELECT count(*)  WHERE cidrAddress(serverIPAddress, 24) NOT IN ('10.0.0.0/24', '10.10.1.0/24')

FROM SyntheticRequest SELECT count(*) FACET cidrAddress(serverIPAddress, 24)

FROM SyntheticRequest SELECT uniques(serverIPAddress)  WHERE cidrAddress(serverIPAddress, 24) = '10.0.0.0/24'

FROM Transaction  SELECT percentage(count(*), WHERE error is true ) AS 'Error Percent'  WHERE host LIKE '%west%' EXTRAPOLATE

FROM Transaction SELECT * ORDER BY duration DESC

FROM Transaction SELECT * WHERE mod(port, 2) = 1

FROM Transaction SELECT appName, duration ORDER BY duration

FROM Transaction SELECT average(duration) TIMESERIES  FACET appName ORDER BY max(responseSize)

FROM Transaction SELECT convert(duration, 'ms', 'min') AS durationMin

FROM Transaction SELECT count(*) FACET if(appName LIKE '%java%', 'Java',     if(appName LIKE '%kafka%', 'Kafka', 'Other'))

FROM Transaction SELECT count(*) FACET response.headers.contentType AS 'content type'

FROM Transaction SELECT count(*) WHERE error IS TRUE TIMESERIES PREDICT ...

FROM Transaction SELECT count(*) WHERE error IS TRUE TIMESERIES PREDICT BY 30 minutes

FROM Transaction SELECT count(*) WHERE error IS TRUE TIMESERIES PREDICT holtwinters(seasonality: 1 hour, alpha: 0.2, beta: 0.5, gamma: 1.5, phi: 0.99)

FROM Transaction SELECT count(*) WHERE error IS TRUE TIMESERIES PREDICT holtwinters(seasonality: AUTO, alpha: 0.2) BY 1 hour USING 2 hours

FROM Transaction SELECT count(*) WHERE error IS TRUE TIMESERIES PREDICT USING 2 days

FROM Transaction SELECT uniqueCount(appId) -- This will return the number of unique App IDs

From Transaction SELECT uniques(host,5000) FACET appName LIMIT 1000

FROM Transaction WITH duration * 1000 AS millisec SELECT millisec

FROM TransactionError SELECT count(*) SINCE 1 day

FROM TransactionTrace SELECT count(*)

SELECT * FROM PageView WHERE countryCode NOT IN ('CA', 'WA')

SELECT *, attribute, function(attribute), attribute1 + attribute2 FROM ...

SELECT 100 * filter(count(*), WHERE eventType() = 'TransactionError') / filter(count(*), WHERE eventType() = 'Transaction')  FROM Transaction, TransactionError  WHERE appName = 'App.Prod'  TIMESERIES 2 Minutes SINCE 6 hours ago

SELECT accountId() FROM Transaction SINCE 1 day ago

SELECT apdex(duration, t: 0.08) FROM Transaction SINCE 3 week ago

SELECT apdex(duration, t: 0.4) FROM Transaction WHERE customerName = 'ReallyImportantCustomer' SINCE 1 day ago

SELECT apdex(duration, t: 0.5) from Transaction WHERE name = 'Controller/notes/index' SINCE 1 hour ago

SELECT average(duration) FROM PageView SINCE 1 week ago

SELECT average(duration) FROM Transaction SINCE 1 day ago TIMESERIES MAX

SELECT average(duration) FROM Transaction TIMESERIES 5 minutes SLIDE BY 1 minute

SELECT average(duration) FROM Transaction TIMESERIES 5 minutes SLIDE BY AUTO

SELECT average(duration) FROM Transaction TIMESERIES 5 minutes SLIDE BY MAX

SELECT average(duration), percentile(duration, 50, 90) FROM PageView SINCE 1 week AGO TIMESERIES AUTO

SELECT bucketPercentile(duration_bucket, 50, 75, 90) FROM Metric SINCE 1 day ago

SELECT bucketPercentile(duration_bucket) FROM Metric SINCE 1 day ago

SELECT capture(errorMessage, r'(?P<firstWord>\S+)\s.+')  FROM Transaction  WHERE errorMessage IS NOT NULL SINCE 1 hour ago

SELECT capture(pageUrl, r'https://(?P<baseUrl>.*.com)/.+')  FROM PageView SINCE 1 day ago

SELECT contains(guids, '5555-3456-555') FROM Foo

SELECT count(*) FROM Log  WHERE message LIKE '%HTTP%'  FACET capture(message, r'.* "(?P<httpMethod>[A-Z]+) .*')

SELECT count(*) FROM PageView  FACET CASES  (   WHERE duration < 1,    WHERE duration > 1 AND duration < 10,    WHERE duration > 10 )

SELECT count(*) FROM PageView FACET city

SELECT count(*) FROM Transaction  FACET CASES  (   WHERE name LIKE '%login%' AS 'Total Logins',    WHERE name LIKE '%feature%' AND customer_type = 'Paid' AS 'Feature Visits from Paid Users' )

SELECT count(*) FROM Transaction  FACET CASES  (   WHERE name LIKE '%login%',    WHERE name LIKE '%feature%' AND customer_type = 'Paid' )

SELECT count(*) FROM Transaction  FACET CASES  (   WHERE name LIKE '%login%',    WHERE name LIKE '%feature%' AND customer_type = 'Paid' )  OR name

SELECT count(*) FROM Transaction FACET accountId() SINCE 1 day ago

SELECT count(*) FROM Transaction SINCE 7 days ago

SELECT count(*) FROM Transaction WHERE accountId() IN (1,2,3) SINCE 1 day ago

SELECT count(*) FROM Transaction WHERE appName = 'interestingApplication'  SINCE 60 minutes ago EXTRAPOLATE

SELECT count(*) FROM Transaction WHERE appName = 'interestingApplication' SINCE 60 minutes ago FACET name TIMESERIES 1 minute EXTRAPOLATE

SELECT count(*) FROM Transaction WHERE contains(guids, '9999-1234-9999')

SELECT count(*) FROM Transaction, PageView SINCE 3 days ago

SELECT count(*) FROM Transaction, TransactionError FACET eventType() TIMESERIES

SELECT count(*)/uniqueCount(session) AS 'Pageviews per Session' FROM PageView

SELECT durations[2] FROM Foo

SELECT earliest(countryCode) FROM PageView FACET userAgentName

SELECT funnel(SESSION,   WHERE name = 'Controller/about/main' AS 'Step 1',   WHERE name = 'Controller/about/careers' AS 'Step 2') FROM PageView SINCE 1 week ago

SELECT getField(durations, 2) FROM Foo

SELECT histogram(duration_bucket, 10, 20)  FROM Metric SINCE 1 week ago

SELECT histogram(duration, 10, 20) FROM PageView SINCE 1 week ago

SELECT histogram(duration, 3, 3, 1)  FROM PageView SINCE 1 week ago

SELECT histogram(duration, 5, 10) FROM PageView SINCE 1 week ago

SELECT histogram(duration, 50, 20) FROM PageView WHERE countryCode IN ('CA', 'US') AND userAgentName = 'Safari' AND pageUrl LIKE '%checkout%' SINCE 1 day ago

SELECT histogram(duration, width: 3, buckets: 3, start: 1)  FROM PageView SINCE 1 week ago

SELECT histogram(duration, width: 5, buckets: 10) FROM PageView SINCE 1 week ago

SELECT histogram(duration)  FROM PageView FACET appName SINCE 1 week ago

SELECT histogram(duration) FROM PageView SINCE 1 week ago

SELECT histogram(myDistributionMetric, 10, 20)  FROM Metric SINCE 1 week ago

SELECT keyset() FROM PageView SINCE 1 day ago

SELECT latest(countryCode) FROM PageView FACET userAgentName

SELECT latestrate(duration, 1 SECOND) FROM PageView

SELECT length(countries) FROM Foo

SELECT length(pageUrl) FROM PageView

SELECT max(getField(mySummary, count)) FROM Metric

SELECT median(duration) FROM PageView TIMESERIES AUTO

SELECT message FROM Log  WHERE capture(message, r'.*Job Failed: (?P<jobName>[A-Za-z]+),.*') = 'ExampleJob'  SINCE 10 minutes ago

SELECT message, blob(`newrelic.ext.message`)  FROM Log WHERE newrelic.ext.message IS NOT NULL

SELECT percentile(duration, 5, 50, 95) FROM PageView TIMESERIES AUTO

SELECT percentile(duration, 95) FROM PageView SINCE 1 week ago COMPARE WITH 1 week AGO   SELECT percentile(duration, 95) FROM PageView SINCE 1 week ago COMPARE WITH 1 week AGO TIMESERIES AUTO

SELECT rate(count(*), 10 minute) FROM Transaction  SINCE 6 hours ago TIMESERIES

SELECT
    sum(mySummary)
FROM
    Metric
WHERE
    getField(mySummary, count) > 10

SELECT sum(numeric(capture(message, r'.*CpuTime:\s(?P<cpuTime>\d+)')))  FROM Log  WHERE message LIKE '%CpuTime:%' SINCE 1 hour ago

SELECT toTimestamp('2023-10-18T15:27:03.123Z')  FROM Event

SELECT toTimestamp('2023-11-03 11:00:32', 'yyyy-MM-dd HH:mm:ss', timezone:'America/Los_Angeles')  FROM Event

SELECT toTimestamp('2023-11-03 11:00:32', 'yyyy-MM-dd HH:mm:ss')  FROM Event WITH TIMEZONE 'America/Los_Angeles'

SELECT uniqueCount(pageUrl) FROM PageView FACET city

SELECT uniqueCount(session), percentile(duration, 95) FROM PageView WHERE userAgentOS = 'Windows' FACET countryCode LIMIT 20 SINCE YESTERDAY

SINCE today UNTIL '2022-05-19T12:00-0500'

SINCE today UNTIL '2022-05-19T12:00-0500' WITH TIMEZONE 'America/Los_Angeles'

SINCE today UNTIL '2022-05-19T12:00' WITH TIMEZONE 'America/Los_Angeles'

WITH '["abc", "xyz"]' AS jsonString SELECT jparse(jsonString)[0]

WITH '{"user": {"name": "John", "id": 5}}' AS jsonString SELECT jparse(jsonString)

WITH '{"userName": "test", "unused": null, "id": 100}' AS jsonString, jparse(jsonString) AS (userName, id) SELECT userName, id

WITH '{"userNames": ["abc", "xyz"]}' AS jsonString SELECT jparse(jsonString)[userNames]

WITH '{"userResult1": 100, "userResult2": 200, "userResult3": 4}' AS jsonString SELECT mapKeys(jparse(jsonString)) AS keys

WITH '{"userResult1": 100, "userResult2": 200, "userResult3": 4}' AS jsonString SELECT mapValues(jparse(jsonString)) AS values

WITH '{"users": [{"name": "A", "id": 5}, {"name": "B", "id": 10}]}' AS jsonString, jparse(jsonString, 'users[*].id') AS ids SELECT ids

WITH '{"value1": "test", "value2": {"nestedValue1": [1, 2, 3], "nestedValue2": 100}}' AS jsonString SELECT mapKeys(jparse(jsonString)) AS keys

WITH '{"value1": "test", "value2": {"nestedValue1": [1, 2, 3], "nestedValue2": 100}}' AS jsonString SELECT mapValues(jparse(jsonString)) AS values

WITH '1693242121842: value=\'{"user": {"name": "John", "id": 5}}\', useless=stuff' AS logMessage, aparse(logMessage, '%: value=\'*\'%') AS jsonString SELECT jparse(jsonString, 'user.name')

WITH 1234567 AS timestampValue SELECT toDatetime(timestampValue, 'yyyy-MM-dd', timezone:'America/Los_Angeles')

WITH 1234567 AS timestampValue SELECT toDatetime(timestampValue, 'yyyy-MM-dd') FROM Event WITH TIMEZONE 'America/Los_Angeles'

WITH 1234567 AS timestampValue SELECT toDatetime(timestampValue)

WITH attribute1 + attribute2 AS attrSum SELECT attrSum, attribute, function(attribute), * FROM ...

WITH concat('(', asnLatitude, ', ', asnLongitude, ')') AS coordinates SELECT coordinates, city, connectionSetupDuration + pageRenderingDuration AS partialDuration, * FROM PageView
