# 쿼리



## table settings 변경 쿼리

```sql
// table 생성 쿼리 보기
show create table users;
```



## table column settings 변경 쿼리&#x20;



{% code fullWidth="true" %}
```sql
// table 생성 쿼리 보기
show create table users;

// created_up, updated_at, deleted_at 테이블 변경
alter table users add column created_at datetime(6) NOT NULL DEFAULT CURRENT_TIMESTAMP(6);
alter table users add column updated_at datetime(6) NOT NULL DEFAULT CURRENT_TIMESTAMP(6) ON UPDATE CURRENT_TIMESTAMP(6);
alter table users add column deleted_at datetime(6) DEFAULT NULL;
```
{% endcode %}



위의 쿼리는 새롭게 생성하는 쿼리 입니다. 이미 만들어진

created\_at, updated\_at, deleted\_at 을 수정하려면

```sql
alter table packages modify created_at datetime(6) NOT NULL DEFAULT CURRENT_TIMESTAMP(6);
alter table packages modify updated_at datetime(6) NOT NULL DEFAULT CURRENT_TIMESTAMP(6) ON UPDATE CURRENT_TIMESTAMP(6);
alter table packages modify deleted_at datetime(6) DEFAULT NULL
```



## mysql 시간 검색 및 시간 변경

```dart
SELECT DATE_FORMAT(NOW(), '%Y년 %m월 %d일 %H시 %i분 %s초');
// 2023년 02월 16일 12시 09분 03초
SELECT @@global.time_zone, @@session.time_zone;
// Asia/Seoul
sudo timedatectl set-timezone Asia/Seoul
// 위의 명령어를 nginx 에서 치게 되면 시간대를 변경 할 수 있다.
```
