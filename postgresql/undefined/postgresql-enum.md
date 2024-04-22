# postgresql 에서 enum 칼럼 변경



만약 사용하던 enum 에서 다른 enum 으로 변경하고싶다면

text 나 varchar 로 변환후에 다른 enum 으로 변경해야합니다.

```sql
-- 열을 text 타입으로 변경
ALTER TABLE public.test1
ALTER COLUMN gender TYPE text USING gender::text;

-- text 타입에서 consulting_channel_enum 타입으로 변경
ALTER TABLE public.test1
ALTER COLUMN gender TYPE consulting_channel_enum USING gender::consulting_channel_enum;

```

