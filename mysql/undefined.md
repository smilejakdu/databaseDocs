# 쿼리





show create table users;

{% code fullWidth="true" %}
```sql
// table 생성 쿼리 보기
show create table users;

// created_up, updated_at, deleted_at 테이블 변경
alter table users add column createdAt datetime(6) NOT NULL DEFAULT CURRENT_TIMESTAMP(6);
alter table users add column updatedAt datetime(6) NOT NULL DEFAULT CURRENT_TIMESTAMP(6) ON UPDATE CURRENT_TIMESTAMP(6);
alter table users add column deletedAt datetime(6) DEFAULT NULL;
```
{% endcode %}
