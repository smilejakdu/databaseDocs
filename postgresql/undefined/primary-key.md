# primary key 값 초기화





```
delete from potential_guest_memo;
delete from potential_guest;

ALTER SEQUENCE potential_guest_id_seq RESTART WITH 1;
ALTER SEQUENCE potential_guest_memo_id_seq RESTART WITH 1;
```
