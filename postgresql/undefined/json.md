# JSON



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



## Postgres JSON\_AGG & STRING\_AGG

```sql
select cg.id,
       cg.name,
       cg.ci,
       (select count(*) from claim_guest cg2 where cg2.ci = cg.ci) as "가입수",
       STRING_AGG(cg.channel || ', ' || TO_CHAR(cg."createdAt", 'YYYY-MM-DD HH24:MI:SS'), '; ') AS "가입채널/회원가입일"
from claim_guest cg
group by cg.ci, cg.id, cg.name;

SELECT
    cg.id,
    cg.name,
    cg.ci,
    (SELECT COUNT(*) FROM claim_guest cg2 WHERE cg2.ci = cg.ci) AS "가입수",
    json_agg(json_build_object('channel', cg.channel, 'createdAt', TO_CHAR(cg."createdAt", 'YYYY-MM-DD HH24:MI:SS'))) AS "가입채널/회원가입일"
FROM
    claim_guest cg
GROUP BY
    cg.ci, cg.id, cg.name;

SELECT
    MIN(cg.id) AS id,
    MAX(cg.name) AS name,
    cg.ci,
    COUNT(*) AS "가입수",
    json_agg(json_build_object('channel', cg.channel, 'createdAt', TO_CHAR(cg."createdAt", 'YYYY-MM-DD HH24:MI:SS'))) AS "가입채널/회원가입일"
FROM
    claim_guest cg
GROUP BY
    cg.ci;

```

```sql
select cg.id,
       cg.name,
       cg.ci,
       (select count(*) from claim_guest cg2 where cg2.ci = cg.ci) as "가입수",
       STRING_AGG(cg.channel || ', ' || TO_CHAR(cg."createdAt", 'YYYY-MM-DD HH24:MI:SS'), '; ') AS "가입채널/회원가입일"
from claim_guest cg
group by cg.ci, cg.id, cg.name;

```

위의 쿼리를 실행하게 되면

아래는 요청하신 데이터를 엑셀 테이블 형식으로 정리한 내용입니다. 각 열에 해당하는 데이터를 정확하게 나열하였습니다.

| id | name | ci                                                                                       | 가입수 | 가입채널 및 가입일                    |
| -- | ---- | ---------------------------------------------------------------------------------------- | --- | ----------------------------- |
| 5  | 안승현  | Uy4PhEwgTuzhpmvygiBxYfTstwUd9aQo2hV4TgA5f6EI57rjEwiE2TGUGt0XlfBQEnhIK8iyO35zq2UkEG259Q== | 2   | KB\_CARD, 2024-05-07 06:15:17 |
| 10 | 박정환  | w+V3tmHBTJtUH0EJbTgjKz/qMYv3vpjjpSQ+BgLPu62R7suX/YEr4gubVXItXtPCNhHHTTKU7H2tNlldvygvGw== | 1   | KB\_CARD, 2024-05-07 12:01:13 |
| 11 | 안승현  | Uy4PhEwgTuzhpmvygiBxYfTstwUd9aQo2hV4TgA5f6EI57rjEwiE2TGUGt0XlfBQEnhIK8iyO35zq2UkEG259Q== | 2   | HIWEBNET, 2024-05-10 10:35:32 |

#### 설명

* **id**: 각 고객의 고유 식별자입니다.
* **name**: 고객의 이름입니다.
* **ci**: 고객의 고유 암호화된 식별 정보입니다.
* **가입수**: 각 고객이 해당 CI로 몇 번 가입했는지 나타냅니다. 같은 CI를 가진 `안승현`의 경우 다른 가입 채널 및 날짜를 가지고 있으므로 각기 다른 행으로 표시됩니다.
* **가입채널 및 가입일**: 고객이 가입한 채널과 가입한 날짜 및 시간입니다.

이 표는 Excel에 직접 입력하거나, 필요에 따라 적절한 소프트웨어를 사용하여 생성할 수 있습니다. 데이터를 정확하게 입력하고 필요한 경우 추가적인 서식을 적용하세요.

```sql
SELECT
    cg.id,
    cg.name,
    cg.ci,
    (SELECT COUNT(*) FROM claim_guest cg2 WHERE cg2.ci = cg.ci) AS "가입수",
    json_agg(json_build_object('channel', cg.channel, 'createdAt', TO_CHAR(cg."createdAt", 'YYYY-MM-DD HH24:MI:SS'))) AS "가입채널/회원가입일"
FROM
    claim_guest cg
GROUP BY
    cg.ci, cg.id, cg.name;

```

위의 쿼리를 실행하게 되면

아래는 요청하신 데이터를 엑셀 테이블 포맷으로 정리한 것입니다. 표는 각 고객의 정보와 가입 채널 및 가입일 데이터를 나타내고 있습니다.

| id | name | ci                                                                                       | 가입수 | 가입채널/회원가입일                                                     |
| -- | ---- | ---------------------------------------------------------------------------------------- | --- | -------------------------------------------------------------- |
| 5  | 안승현  | Uy4PhEwgTuzhpmvygiBxYfTstwUd9aQo2hV4TgA5f6EI57rjEwiE2TGUGt0XlfBQEnhIK8iyO35zq2UkEG259Q== | 2   | \[{"channel": "KB\_CARD", "createdAt": "2024-05-07 06:15:17"}] |
| 10 | 박정환  | w+V3tmHBTJtUH0EJbTgjKz/qMYv3vpjjpSQ+BgLPu62R7suX/YEr4gubVXItXtPCNhHHTTKU7H2tNlldvygvGw== | 1   | \[{"channel": "KB\_CARD", "createdAt": "2024-05-07 12:01:13"}] |
| 11 | 안승현  | Uy4PhEwgTuzhpmvygiBxYfTstwUd9aQo2hV4TgA5f6EI57rjEwiE2TGUGt0XlfBQEnhIK8iyO35zq2UkEG259Q== | 2   | \[{"channel": "HIWEBNET", "createdAt": "2024-05-10 10:35:32"}] |

#### 설명

* **id**: 각 고객의 고유 식별자입니다.
* **name**: 고객의 이름입니다.
* **ci**: 고객의 고유 암호화된 식별 정보입니다.
* **가입수**: 각 ci를 가진 고객의 총 가입 수입니다. 이 경우 같은 ci 값을 가진 `안승현`의 가입수는 두 번째와 세 번째 행에서 똑같이 2로 나타납니다.
* **가입채널/회원가입일**: 고객이 가입한 채널과 그 채널의 가입 일시를 JSON 배열로 나타낸 것입니다.

이 표는 Excel에서 작성하거나, 해당 데이터를 엑셀 파일로 직접 입력하여 사용할 수 있습니다. 데이터를 엑셀에 정확히 맞게 입력하면 됩니다.

```sql
select cg.id,
       cg.name,
       cg.ci,
       (select count(*) from claim_guest cg2 where cg2.ci = cg.ci) as "가입수",
       STRING_AGG(cg.channel || ', ' || TO_CHAR(cg."createdAt", 'YYYY-MM-DD HH24:MI:SS'), '; ') AS "가입채널/회원가입일"
from claim_guest cg
group by cg.ci, cg.id, cg.name;

SELECT
    cg.id,
    cg.name,
    cg.ci,
    (SELECT COUNT(*) FROM claim_guest cg2 WHERE cg2.ci = cg.ci) AS "가입수",
    json_agg(json_build_object('channel', cg.channel, 'createdAt', TO_CHAR(cg."createdAt", 'YYYY-MM-DD HH24:MI:SS'))) AS "가입채널/회원가입일"
FROM
    claim_guest cg
GROUP BY
    cg.ci, cg.id, cg.name;

SELECT
    MIN(cg.id) AS id,
    MAX(cg.name) AS name,
    cg.ci,
    COUNT(*) AS "가입수",
    json_agg(json_build_object('channel', cg.channel, 'createdAt', TO_CHAR(cg."createdAt", 'YYYY-MM-DD HH24:MI:SS'))) AS "가입채널/회원가입일"
FROM
    claim_guest cg
GROUP BY
    cg.ci;

```

| id | name | ci                                                                                       | 가입수 | 가입채널/회원가입일                                                                                                                  |
| -- | ---- | ---------------------------------------------------------------------------------------- | --- | --------------------------------------------------------------------------------------------------------------------------- |
| 10 | 박정환  | w+V3tmHBTJtUH0EJbTgjKz/qMYv3vpjjpSQ+BgLPu62R7suX/YEr4gubVXItXtPCNhHHTTKU7H2tNlldvygvGw== | 1   | \[{"channel": "KB\_CARD", "createdAt": "2024-05-07 12:01:13"}]                                                              |
| 5  | 안승현  | Uy4PhEwgTuzhpmvygiBxYfTstwUd9aQo2hV4TgA5f6EI57rjEwiE2TGUGt0XlfBQEnhIK8iyO35zq2UkEG259Q== | 2   | \[{"channel": "KB\_CARD", "createdAt": "2024-05-07 06:15:17"}, {"channel": "HIWEBNET", "createdAt": "2024-05-10 10:35:32"}] |

#### 설명

* **id**: 고객의 고유 식별자입니다.
* **name**: 고객의 이름입니다.
* **ci**: 고객의 고유 암호화된 식별 정보입니다.
* **가입수**: 해당 ci를 가진 고객의 총 가입 수입니다.
* **가입채널/회원가입일**: 고객이 가입한 채널과 가입 일시의 정보를 JSON 배열로 나타낸 것입니다.

이 표는 Excel에서 작성하거나, 해당 데이터를 엑셀 파일로 직접 입력하여 사용할 수 있습니다. 각 열에 해당하는 데이터를 엑셀의 각 셀에 맞게 조정하여 입력하면 됩니다.



## JSON 출력 예쁘게 하기

```sql
select jsonb_pretty("iamportInfo") from iamport_payment
```

위처럼 출력하게 되면 줄바꿈이 적용되어서 데이터가 출력이 된다.

