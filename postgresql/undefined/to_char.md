# TO\_CHAR



`TO_CHAR` 함수는 SQL에서 날짜, 숫자, 타임스탬프 등의 데이터 타입을 문자열로 변환하는 데 사용됩니다. 이 함수는 특히 PostgreSQL 같은 데이터베이스 관리 시스템에서 지원됩니다. `TO_CHAR` 함수의 주요 목적은 데이터 포맷팅입니다. 사용자가 지정한 형식에 따라 데이터를 문자열로 변환하여, 보고서 작성, 사용자 인터페이스 표시, 데이터 처리 등에서 사용하기 용이한 형태로 만들어 줍니다.

#### 기본 사용법:

```sql
TO_CHAR(value, format)
```

* `value`: 변환하려는 날짜, 숫자, 또는 타임스탬프 값입니다.
* `format`: 변환된 문자열의 형식을 지정하는 문자열입니다. 날짜, 시간, 숫자 등에 대한 다양한 포맷 코드를 지원합니다.

#### 예시:

1.  **날짜 변환**:

    ```sql
    SELECT TO_CHAR(NOW(), 'YYYY-MM-DD HH24:MI:SS');
    ```

    이 예시는 현재 시간(`NOW()`)을 `'년-월-일 시:분:초'` 형식의 문자열로 변환합니다.\
    ex) 2024-04-10 06:37:12
2.  **숫자 변환**:

    ```sql
    SELECT TO_CHAR(123456789, 'FM999,999,999');
    ```

    이 예시는 숫자 `123456789`를 `'123,456,789'` 형식의 문자열로 변환합니다. `FM` 모디파이어는 앞쪽의 공백을 제거합니다.

#### 포맷 코드:

`TO_CHAR` 함수에서 사용할 수 있는 포맷 코드는 데이터 타입(날짜/시간, 숫자 등)에 따라 다릅니다. 예를 들어, 날짜/시간 형식에서는 `YYYY`는 연도를 4자리 숫자로, `MM`은 월을 2자리 숫자로, `DD`는 일을 2자리 숫자로 표현합니다. 시간에 대해서는 `HH24`가 24시간 형식의 시간을 나타냅니다.

`TO_CHAR` 함수를 사용함으로써, 데이터를 데이터베이스에서 직접 포맷팅하여 애플리케이션 로직이나 프론트엔드에서 별도의 변환 처리를 하지 않고도 바로 사용할 수 있는 형태로 만들 수 있습니다. 이는 특히 데이터 포맷팅 요구사항이 복잡하거나, 다양한 로케일 및 포맷을 지원해야 하는 경우 유용합니다.



