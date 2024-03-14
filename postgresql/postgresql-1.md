# postgresql 덤프



## 특정 테이블 백업

```sql
# to run: sh microprotect-pgdump-dev.sh

DATE=`date +%Y-%m-%d`

# info rds
#DEV_DB_HOST=localhost
#DEV_DB_PORT=5433
#DEV_DB_USER=dev
#DEV_DB_PWD=dev_password

# info local
LOCAL_DB_HOST=127.0.0.1
LOCAL_DB_PORT=5432
LOCAL_DB_USER=anseunghyeon
LOCAL_DB_PWD=tkakrnl12
TABLE_NAME=customer

# 전체 덤프


# 로컬 데이터베이스에 백업 적용
#pg_restore --no-owner -h $LOCAL_DB_HOST -U $LOCAL_DB_USER -p $LOCAL_DB_PORT -d microprotect -v "microprotect-dump_$DATE.sql"

```





## 전체 데이터 백업

```sql
# to run: sh microprotect-pgdump-dev.sh

DATE=`date +%Y-%m-%d`

# info rds
DEV_DB_HOST=localhost
DEV_DB_PORT=5433
DEV_DB_USER=dev
DEV_DB_PWD=microprotect1234!
DATABASE_NAME=microprotect

# info local
LOCAL_DB_HOST=127.0.0.1
LOCAL_DB_PORT=5432
LOCAL_DB_USER=anseunghyeon
LOCAL_DB_PWD=tkakrnl12
TABLE_NAME=customer

# 특정 테이블 백업
#pg_dump -h $LOCAL_DB_HOST -U $LOCAL_DB_USER -p $LOCAL_DB_PORT --schema-only -t $TABLE_NAME -f "$TABLE_NAME-dump_$DATE.sql" microprotect
# 전체 덤프
pg_dump -h $DEV_DB_HOST -U $DEV_DB_USER -p $DEV_DB_PORT -W -F c -b -v -f "${DATABASE_NAME}-dump_${DATE}.sql" $DATABASE_NAME

# 로컬 데이터베이스에 백업 적용
#pg_restore --no-owner -h $LOCAL_DB_HOST -U $LOCAL_DB_USER -p $LOCAL_DB_PORT -d microprotect -v "microprotect-dump_$DATE.sql"

```
