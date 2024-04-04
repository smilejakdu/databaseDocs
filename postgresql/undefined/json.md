# Json



```sql
select * from customer_insurance;

select jsonb_pretty(mydata) from customer_insurance where "customerId" = 1;

SELECT jsonb_pretty(mydata->'insuredList') as insuredList
FROM customer_insurance
WHERE "customerId" = 1;

SELECT
    jsonb_pretty(jsonb_array_elements(mydata->'insuredList')->'contractList') as contractList
FROM
    customer_insurance
WHERE
    "customerId" = 1;

SELECT
    jsonb_pretty(jsonb_array_elements(mydata->'insuredList')->'contractList') as contractList
FROM
    customer_insurance
WHERE
    "customerId" = 1;


SELECT
    COUNT(*) AS contractListCount
FROM
    customer_insurance,
    jsonb_array_elements(mydata->'insuredList') AS insured,
    jsonb_array_elements(insured->'contractList') AS contract
WHERE
    "customerId" = 1;

```



빈객체를 제외하고 불러오기

```sql
SELECT *
FROM customer_insurance
WHERE your_json_column <> '{}'::jsonb;
```
