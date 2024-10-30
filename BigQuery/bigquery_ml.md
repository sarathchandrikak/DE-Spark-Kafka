# Big Query ML for Sales data


1. Create a dataset named bqml and read data from the public dataset available at [bigquery public dataset](https://console.cloud.google.com/marketplace/product/obfuscated-ga360-data/obfuscated-ga360-data). 
After reading save the view as training_data 


```
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20160801' AND '20170631'
LIMIT 10000;

```

2. Create a model to predict whether a visitor will make a transaction

```
CREATE
  OR
  REPLACE MODEL `bqml.sample_model`
  OPTIONS(model_type='logistic_reg')
AS
  SELECT * from `bqml.training_data`;
```


3. Evaluate the model 

```
SELECT
  *
FROM
  ml.EVALUATE(MODEL `bqml.sample_model`);
```

4. Filter data and get only July month data of year 2017 and save as view july_data

```
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews,
  fullVisitorId
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801';
```

5. Predict purchases per country or region

```
SELECT
  country,
  SUM(predicted_label) as total_predicted_purchases
FROM
  ml.PREDICT(MODEL `bqml.sample_model`, (
SELECT * FROM `bqml.july_data`))
GROUP BY country
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```

6. Predict purchases per user

```
SELECT
  fullVisitorId,
  SUM(predicted_label) as total_predicted_purchases
FROM
  ml.PREDICT(MODEL `bqml.sample_model`, (
SELECT * FROM `bqml.july_data`))
GROUP BY fullVisitorId
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```
