# settings

## localhost install

Mac 기준입니다

#### macOS에서 PostgreSQL 설치하기

macOS에서 PostgreSQL을 설치하는 데는 여러 방법이 있으나, 가장 일반적인 방법은 Homebrew를 사용하는 것입니다.

1. **Homebrew 설치**: Homebrew가 설치되어 있지 않다면, [Homebrew 웹사이트](https://brew.sh)에 방문하여 설치 지침을 따릅니다.
2. **터미널 열기**: `Finder`에서 `응용 프로그램` > `유틸리티`로 이동하여 `Terminal`을 실행합니다.
3.  **PostgreSQL 설치**: 터미널에서 다음 명령어를 입력하여 PostgreSQL을 설치합니다:

    ```
    brew install postgresql
    ```
4.  **서비스 시작**: 설치가 완료되면, PostgreSQL 서비스를 시작합니다. 이는 Homebrew를 통해 관리됩니다. 서비스를 시작하려면 다음 명령어를 사용합니다:

    ```
    brew services start postgresql
    ```
5. **사용자 설정 및 데이터베이스 생성**: 필요한 경우, `createuser`와 `createdb` 명령어를 사용하여 데이터베이스 사용자와 데이터베이스를 생성할 수 있습니다.

설치 후, `psql` 명령어를 사용하여 PostgreSQL 데이터베이스에 접속하고 관리할 수 있습니다.



설치가 다 됐다면&#x20;

`psql -d postgres` 로 들어갔을때 접속이 됩니다.



## 사용자 생성

PostgreSQL에서 사용자 이름(username)과 비밀번호(password)를 설정하는 과정은 몇 단계로 나눌 수 있습니다. 이 과정은 PostgreSQL의 `psql` 명령어 인터페이스를 사용합니다.

1. **psql에 접속**:
   * 먼저 터미널에서 PostgreSQL의 `psql` 도구에 접속합니다. 기본적으로, 설치 후 첫 번째 사용자는 시스템의 현재 사용자와 동일하며, 이 사용자는 PostgreSQL의 슈퍼유저 권한을 가지고 있습니다.
   *   다음 명령어를 사용하여 `psql`에 접속합니다:

       ```sh
       psql -U your_username -d postgres
       ```
   * 여기서 `your_username`은 PostgreSQL에 설정된 사용자 이름입니다. 처음에는 시스템 사용자 이름일 수 있습니다.
2. **새 사용자 생성**:
   *   새 PostgreSQL 사용자를 생성하려면, 다음 SQL 명령어를 `psql` 프롬프트에서 실행합니다:

       ```sql
       CREATE USER new_username WITH PASSWORD 'new_password';

       postgres=# CREATE USER root WITH PASSWORD 'test1234';
       생성이 가능합니다. 
       ```
3. **데이터베이스 권한 부여**:
   *   새 사용자에게 특정 데이터베이스에 대한 접근 권한을 부여하려면 다음과 같이 실행합니다:

       ```sql
       GRANT ALL PRIVILEGES ON DATABASE database_name TO new_username;
       ```
   * `database_name`을 해당 사용자에게 권한을 부여하려는 데이터베이스 이름으로 변경합니다.
4. **접속 확인**:
   *   새로 생성된 사용자로 접속을 시도하여 설정이 올바르게 이루어졌는지 확인할 수 있습니다:

       ```sh
       psql -U new_username -d database_name
       ```
   * 여기서 `database_name`은 해당 사용자가 접근할 데이터베이스 이름입니다.
5. **psql 종료**:
   * 작업을 마친 후, `psql`에서 나가려면 `\q`를 입력합니다.

이렇게 하면 새로운 PostgreSQL 사용자를 생성하고, 해당 사용자에게 데이터베이스 접근 권한을 부여할 수 있습니다. 사용자 관리와 관련된 보다 복잡한 설정을 원한다면, PostgreSQL의 공식 문서에서 더 많은 정보를 찾아볼 수 있습니다.
