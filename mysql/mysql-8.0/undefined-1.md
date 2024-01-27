# 옵티마이저

MySQL 8.0의 옵티마이저는 JOIN 순서를 자동으로 최적화하는 고급 기능을 제공하지만, 때로는 특정 쿼리의 성능을 개선하기 위해 개발자가 직접 JOIN 순서를 조정할 필요가 있을 수 있습니다. 여기서는 JOIN 순서가 실행 계획과 쿼리 성능에 미치는 영향에 대한 간단한 예를 들어보겠습니다.

#### 가정

* `employees` 테이블과 `departments` 테이블이 있고, 여러 명의 직원이 하나의 부서에 속할 수 있습니다.
* `employees` 테이블은 수백만 개의 레코드를 가지고 있고, `departments` 테이블은 수십 개의 레코드만 가지고 있습니다.
* 목표는 특정 조건을 만족하는 직원과 그들의 부서 정보를 조회하는 것입니다.

#### JOIN 순서의 중요성

*   **쿼리 1:** 먼저 `departments`를 조회하고 그 결과를 바탕으로 `employees`를 JOIN하는 경우

    ```sql
    SELECT e.*, d.*
    FROM departments d
    JOIN employees e ON e.department_id = d.id
    WHERE d.location = 'New York';
    ```

    이 경우, 먼저 `departments`에서 'New York'에 위치한 부서를 찾고, 그 결과에 해당하는 직원 정보를 `employees`에서 검색합니다. 부서의 수가 적기 때문에 이러한 순서는 초기 필터링을 통해 조회해야 할 직원의 수를 효과적으로 줄일 수 있습니다.
*   **쿼리 2:** 반대로 먼저 `employees`를 조회하고 그 결과를 바탕으로 `departments`를 JOIN하는 경우

    ```sql
    SELECT e.*, d.*
    FROM employees e
    JOIN departments d ON e.department_id = d.id
    WHERE d.location = 'New York';
    ```

    이 경우, 모든 직원 정보를 스캔한 후에 부서 정보와 JOIN하게 됩니다. 이는 `departments`에서 먼저 필터링을 하는 경우보다 훨씬 많은 양의 데이터를 처리해야 하므로 비효율적일 수 있습니다.

#### 결론

실제로 MySQL 옵티마이저는 이러한 쿼리를 실행할 때 자동으로 더 효율적인 순서를 선택하려고 시도합니다. 그러나 복잡한 쿼리에서는 옵티마이저가 최적의 순서를 찾지 못할 수도 있고, 통계 정보가 최신 상태가 아니어서 비효율적인 실행 계획을 선택할 수도 있습니다. 따라서, 성능에 민감한 쿼리의 경우 `EXPLAIN`을 사용하여 실행 계획을 분석하고, 필요에 따라 JOIN 순서를 조정하여 최적의 성능을 달성할 수 있습니다.
