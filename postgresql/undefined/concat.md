# CONCAT



`CONCAT` 함수는 여러 문자열을 하나로 결합하는 SQL 함수입니다. 대부분의 데이터베이스 관리 시스템(DBMS)에서 지원되며, 문자열을 순차적으로 결합해 새로운 문자열을 생성합니다. 사용 방식은 데이터베이스마다 조금씩 다를 수 있지만, 기본적인 개념은 동일합니다.

#### 기본 사용법:

```sql
CONCAT(string1, string2, ..., stringN)
```

* `string1, string2, ..., stringN`: 결합하고자 하는 문자열들입니다. 두 개 이상의 문자열을 인자로 넣을 수 있으며, 순서대로 결합됩니다.

#### 예시:

1.  **기본 문자열 결합**:

    ```sql
    SELECT CONCAT('Hello', ' ', 'World');
    ```

    이 예시는 `'Hello'`, 공백 `' '`, 그리고 `'World'` 세 문자열을 결합하여 `'Hello World'`를 반환합니다.
2.  **데이터베이스 컬럼과 문자열 결합**:

    ```sql
    SELECT CONCAT(firstName, ' ', lastName) AS fullName FROM Users;
    ```

    이 예시는 `Users` 테이블의 `firstName` 컬럼과 `lastName` 컬럼을 공백으로 구분하여 결합한 `fullName`을 반환합니다. 사용자의 이름을 전체 이름으로 표시하는 데 사용할 수 있습니다.

#### PostgreSQL과 MySQL에서의 차이점:

* **PostgreSQL**: `CONCAT` 함수는 `NULL` 값을 문자열로 처리합니다. 즉, 인자 중 하나라도 `NULL`이면, 그 값은 무시되고 나머지 문자열만 결합됩니다.
* **MySQL**: `CONCAT` 함수도 `NULL` 값을 만나면 전체 결과가 `NULL`이 됩니다. 이를 방지하고자 `CONCAT_WS` 함수를 사용하여 `NULL` 값을 무시할 수 있습니다.

#### 주의사항:

* `CONCAT` 함수를 사용할 때 `NULL` 값에 주의해야 합니다. 어떤 DBMS는 `NULL`을 문자열로 취급하지 않아 전체 결과가 `NULL`이 될 수 있습니다. 이러한 경우 `COALESCE` 함수나 다른 방법을 사용하여 `NULL` 값을 적절한 기본값으로 변환할 수 있습니다.
* `CONCAT` 함수는 문자열 결합을 위한 기본적인 함수이지만, 복잡한 로직이 필요한 경우 다른 문자열 처리 함수와 함께 사용될 수 있습니다. 예를 들어, 문자열을 조건에 따라 변형하거나, 특정 패턴을 기준으로 분할하는 등의 작업입니다.

`CONCAT` 함수는 SQL 쿼리 내에서 동적으로 문자열을 생성하거나, 데이터베이스 컬럼 값들을 조합하여 새로운 문자열 형태의 데이터를 만들고자 할 때 유용하게 사용됩니다.

