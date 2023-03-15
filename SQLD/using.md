# I. SQL 기본
## 1. 관계형 데이터베이스 개요
- DB : 특정 기업이나 조직 혹은 개인의 필요에 의해 데이터를 일정한 형태로 저장해 놓은 것
- DBMS : 효율적인 데이터관리 및 예기치 못한 사건으로 인한 데이터의 손상을 피하고, 필요시 데이터를 복귀하기 위한 SW
- SQL : 관계형 DB에서 데이터 정의, 조작, 제어를 위해 사용하는 언어
  - DML : SELECT, INSERT, UPDATE, DELETE
  - DDL : CREATE, ALTER, DROP, RENAME
  - DCL : GRANT, REVOKE
  - TCL : COMMIT, ROLLBACK
- 테이블 : DB의 기본단위로, 데이터를 저장하는 객체
  - 가로 : 행 = 로우 = 튜플 = 인스턴스
  - 세로 : 열 = 컬럼
- 정규화 : 데이터의 정합성 확보와 데이터 입력/수정/삭제시 발생할 수 있는 이상현상을 방지하기 위해 중복제거
- 기본키(PK) : 테이블에 존재하는 각 행을 한 가지 의미로 특정할 수 있는 한 개 이상의 컬럼
- 외부키(FK) : 다른 테이블의 기본키로 사용되고 있는 관계를 연결하는 컬럼
## 2. DDL
### 데이터 유형
- CHAR(s) :  고정 길이 문자열 정보. 최대 길이만큼 공간 채움
- VARCHAR(s) : 가변 길이 문자열 정보. 할당된 변수 값의 바이트만 적용
- NUMBER : 정수, 실수 등 숫자 정보
- DATE : 날짜와 시각 정보

<br/>

- 서로 다른 테이블명끼리 중복되면 안 된다
- 테이블 내의 컬럼명도 중복될 수 없다
- 각 컬럼들은 ,로 구분되고 ;로 끝난다
- 컬럼 뒤에 데이터 유형이 반드시 지정되어야 한다
- 테이블명과 컬럼명은 반드시 문자로 시작해야 한다
- A-Z, a-z, 0-9, _, $, #만 사용가능

### 제약조건; 데이터의 무결성 유지
- PRIMARY KEY : UNIQUE & NOT NULL
- UNIQUE KEY : 고유키 정의
- NOT NULL : NULL 값 입력 금지
- CHECK : 입력값 범위 제한
- FOREIGN KEY : NULL 가능, 여러속성 가능

### 테이블 생성
```SQL
CREATE TABLE PLAYER (PLAYER_ID CHAR(7) NOT NULL,
PLAYER NAME VARCHAR2(20) NOT NULL);
```
### 테이블 구조 변경
```SQL
# 추가
ALTER TABLE PLAYER ADD(ADDRESS VARCHAR2(80));

# 삭제
ALTER TABLE PLAYER DROP COLUMN ADDRESS;

# 수정
ALTER TABLE TEAM_TEMP MODIFY(ORIG_YYYY VARCHAR2(8) DEFAULT '20020129' NOT NULL);
```
```SQL
# 제약조건 삭제
DROP CONSTRAINT 조건명;

# 제약조건 추가
ADD CONSTRAINT 조건명 조건(컬럼명);

# 테이블명 변경
RENAME PLAYER TO PLAYER_BACKUP;

# 테이블 삭제
DROP TABLE PLAYER;

# 테이블 데이터 삭제
TRUNCATE TABLE PLAYER;

# 컬럼명 변경
RENAME COLUMN TEAM_ID TO T_ID;
```
## 3. DML
> DDL 실행 시 AUTO COMMIT 되나, DML의 경우는 COMMIT을 입력해야 함
### 예제
```SQL
INSERT INTO PLAYER (PLAYER) VALUES ('PJS');
UPDATE PLAYER SET BACK_NO = 60;
DELETE FROM PLAYER;
SELECT PLAYER_ID FROM PLAYER;
SELECT PLAYER AS "선수명" FROM PLAYER;
```
- DISTINCT : 중복시 1회만 출력
### 와일드카드
- \* : 모든
- % : 모든
- _ : 한글자

### 합성연산자
- || : 문자와 문자를 연결

## 4. TCL
- 트랜잭션 : 밀접히 관련되어 분리될 수 없는 1개 이상의 DB 조작하는 논리적 연산단위
### 구성
- COMMIT : 올바르게 반영된 데이터를 DB에 반영
- ROLLBACK : COMMIT 되지 않은 모든 트랜잭션 시작 이전의 상태로 되돌림
- SAVEPOINT : 저장 지점
### 트랜잭션의 특성
- 원자성 : 트랜잭션에서 정의된 연산들은 모두 성공적으로 실행되던가 전혀 실행되지 않거나 해야함
- 일관성 : 트랜잭션 실행 전 DB내용이 잘못되지 않으면 실행 후도 잘못되지 않아야 함
- 고립성 : 트랜잭션 실행 도중 다른 트랜잭션의 영향을 받아 잘못된 결과를 만들어서는 안됨
- 지속성 : 트랜잭션이 성공적으로 수행되면 DB의 내용은 영구적으로 저장됨

## 5. WHERE 절
> 연산자 우선 순위 : () $\rightarrow$ NOT $\rightarrow$ 비교연산자 $\rightarrow$ AND $\rightarrow$ OR
### 연산자의 종류
- BETWEEN A AND B : A와 B의 값 사이
- IN (list) : 리스트에 있는 값 중 어느 하나라도 일치하는 경우
- IS NULL : NULL값인 경우(Oracle에서는 VARCHAR2 빈 문자열을 NULL로 판단)
- IS NOT NULL : NULL값이 아닌 경우
- NOT IN (list) : list의 값과 하나도 일치하지 않음
- LIKE '비교문자열' : 비교문자열과 형태가 일치
```SQL
SELECT PLAYER_NAME 선수명 FROM PLAYER WHERE TEAM_ID = 'K2';
SELECT PLAYER_NAME 선수명 FROM PLAYER WHERE TEAM_ID IN ('K2','K7')
SELECT PLAYER_NAME 선수명 FROM PLAYER WHERE HEIGHT BETWEEN 170 AND 180;
SELECT PLAYER_NAME 선수명 FROM PLAYER WHERE POSITION IS NULL;
```
- ROWNUM : 원하는 만큼의 행을 가져올때 사용
```SQL
SELECT * FROM PLAYER_NAME FROM PLAYER WHERE ROWNUM = 1;
```
### NULL값 연산
- NULL 값과의 수치연산은 NULL값을 리턴
- NULL 값과의 비교연산은 FALSE를 리턴

## 6. 함수
### 단일행 함수
- SELECT, WHERE, ORDER BY 절에서 사용가능
- 행에 개별적 조작
- 여러 인자가 있어도 결과는 1개만 출력
- 함수 인자에 상수, 변수, 표현식 사용 가능
- 함수 중첩 가능

### 문자형 함수
- LOWER : 문자열을 소문자로
- UPPER : 문자열을 대문자로
- ACSII : 문자의 ASCII값 반환
- CHR : ASCII 값에 해당하는 문자 반환
- CONCAT : 문자열 1, 2를 연결
- SUBSTR : 문자열 중 M위치에서 N개의 문자 반환
- LENGTH : 문자열 길이를 숫자 값으로 변환
```
CONCAT('RDBMS', 'SQL') -> 'RDBMS SQL'
SUBSTR('SQL EXPERT', 5, 3) -> 'EXP'
LTRIM('xxxYYZZxYZ', 'x') -> 'YYZZxYZ'
TRIM('x' FROM 'xxYYZZxYZxx') -> 'YYZZxYZ'
```

### 숫자형 함수
- SIGN(숫자) : 숫자가 양수면 1, 음수면 -1, 0이면 0반환
- MOD(숫자1, 숫자2) : 숫자1을 숫자2로 나누어 나머지 반환
- CEIL(숫자) : 크거나 같은 최소정수 반환
- FLOOR(숫자) : 작거나 같은 최소정수 반환
```
ROUND(38.5235, 3) -> 38.524   # 반올림
TRUNC(38.5235, 3) -> 38.523   # 버림
```

### 날짜형 함수
- SYSDATE : 현재날짜와 시각 출력
- EXTRACT : 날짜에서 데이터 출력
- TO_NUMBER(TO_CHAR(d, 'YYYY')) : 연도를 숫자로 출력
- 1 = 하루, 1/24 = 한 시간, 1/24/60 = 1분
  
### NULL관련 함수
- NVL(식1, 식2) : 식1의 값이 NULL이면 식2 출력
- NULLIF(식1, 식2) : 식1이 식2와 같으면 NULL, 아니면 식1 출력
- COALESCE(식1, 식2) : NULL이 아닌 최초의 표현식을 반환, 모두 NULL이면 NULL반환
```
COALESCE(NULL, NULL, 'abc') -> 'abc'
```

## 7. Group By, Having 절
### 다중성 집계 함수
- 여러 행들의 그룹이 모여 그룹당 하나의 결과를 반환
- GROUP BY 절은 행들을 소그룹화
- SELECT, HAVING, ORDER BY 절에 사용가능
  - ALL : Default, 생략가능
  - DISTINCT : 같은 값을 하나의 데이터로 간주
### 예제
- COUNT(*) : NULL포함 행의 수
- COUNT(표현식) : NULL제외 행의 수
- SUM, AVG : NULL제외 합계, 평균
- STDDEV : 표준편차
- VARIAN : 분산
- MAX, MIN : 최대, 최소값

### GROUP BY, HAVING 절의 특징
- GROUP BY 절을 통해 소그룹별 기준을 정한 후, SELECT 절에 집계 함수를 사용한다
- 집계 함수의 통계 정보는 NULL값을 가진 행을 제외하고 수행한다
- GROUP BY 절에서는 ALIAS 사용불가
- 집계함수는 WHERE절에 올 수 없다
- HAVING 절에는 집계함수를 이용하여 조건 표시 가능
- HAVING 절은 일반적으로 GROUP BY 뒤에 위치

## 8. ORDER BY절
### ORDER BY 특징
- SQL문으로 조회된 데이터들을 다양한 목적에 맞게 특정한 컬럼을 기준으로 정렬하여 출력
- ORDER BY절에 컬럼명 대신 ALIAS 명이나 컬럼 순서를 나타내는 정수도 사용가능
- DEFAULT 값으로 오름차순(ASC)이 적용되며 DESC 옵션을 통해 내림차순 정렬가능
- SQL문장의 제일 마지막에 위치한다
- SELECT절에서 정의하지 않은 컬럼 사용 가능
- ORACLE에서는 NULL을 가장 큰 값, SQL server에서는 가장 작은 값으로 취급

### SELECT 문장 실행 순서
- SELECT $\rightarrow$ ALIAS $\rightarrow$ FROM $\rightarrow$ WHERE $\rightarrow$ GROUP BY $\rightarrow$ HAVING $\rightarrow$ SELECT $\rightarrow$ ORDER BY
- 메모리에 모든 컬럼을 올리므로 ORDER BY에서 SELECT에 정의 안된 컬럼 사용가능

### SQL Server의 WITH TIES
```SQL
SELECT TOP(2) WITH TIES ENAME, SAL FROM EMP ORDER BY SAL DESC;
# 급여가 높은 2명을 내림차순으로 출력하되, 같은 급여를 받는 사원은 같이 출력
```

## 10. JOIN
> 두개 이상의 테이블들을 연결&middot;결합하여 데이터를 출력하는 것
<br/>
> 일반적으로 PK FK값의 연관에 의해 JOIN이 성립되나, 어떤 경우에는 논리적인 값들의 연관만으로 JOIN성립

### EQUI JOIN
> 2개의 테이블 간에 컬럼 값들이 서로 정확히 일치하는 경우에 사용, 대부분 PK, FK 관계 기반
```SQL
SELECT PLAYER.PLAYER_NAME FROM PLAYER;
```
- 위 예시처럼 컬럼명 앞에 테이블 명을 기술해줘야 함

### NON EQUI JOIN
> 2개의 테이블 간에 컬럼 값들이 서로 정확하게 일치하지 않는 경우에 사용
- '=' 연산자가 아닌 BETWEEN, >, <= 등의 연산자 사용
```SQL
SELECT E.ENAME, E.JOB, E.SAL, S.GRADE FROM EMP E, SALGRADE S WHERE E.SAL BETWEEN S.LOSAL AND S.HSAL;
```

# II. SQL 활용
## 1. 표준 조인
### FROM 절 JOIN 형태
- INNER JOIN : JOIN 조건에서 동일한 값이 있는 행만 반환, USING&middot;ON절 필수사용
- NATURAL JOIN : 두 테이블 간의 동일한 이름을 갖는 모든 컬럼들에 대해 EQUI JOIN 수행, NATURAL JOIN이 명시되면 추가로 USIN, ON, WHERE절에서 JOIN조건을 정의할 수 없다. SQL서버 지원하지 않음
- USING 조건절 : 같은 이름을 가진 컬럼들 중 원하는 컬럼에 대해서만 선택적 EQUI JOIN. JOIN 컬럼에 대해 ALIAS나 테이블 이름과 같은 접두사 못 붙임. SQL서버 지원하지 않음
- ON 조건절 : ON 조건절과 WHERE 조건절을 분리하여 이해가 쉬우며, 컬럼명이 다르더라도 JOIN 조건을 사용가능한 장점. ALIAS나 테이블명 반드시 사용
- CROSS JOIN(카타시안 곱) : 양 집합의 M*N건의 데이터 조합이 발생
- OUTER JOIN : JOIN조건에서 동일한 값이 없는 행도 반환 가능, USING이나 ON조건절 반드시 사용, SQL식에서 +안 붙은 쪽으로 JOIN
  - LEFT OUTER JOIN : 좌측 테이블에 해당하는 데이터를 읽은 뒤, 우측 테이블에서 JOIN 대상 데이터를 읽어온다. 우측값에 같은 값이 없는 경우 NULL로 채움
  - RIGHT OUTER JOIN : 우측 테이블에 해당하는 데이터를 읽은 뒤, 좌측 테이블에서 JOIN 대상 데이터를 읽어온다. 좌측값에 같은 값이 없는 경우 NULL로 채움
  - FULL OUTER JOIN : 좌우측 테이블의 모든 데이터를 읽어 JOIN하여 결과 생성. 중복 삭제

## 2. 집합 연산자
> 두 개 이상의 테이블에서 조인을 사용하지 않고 연관된 데이터를 조회할 때 사용 SELECT절의 컬럼 수가 동일하고 SELECT절의 동일 위치에 존재하는 컬럼의 데이터 타입이 상호호환할때 사용가능
### 일반 집합 연산자
- UNION : 합집합(중복제거) 정렬
- UNION ALL : 합집합(중복 행도 표시) 정렬하지 않음
- INTERSECT : 교집합(중복제거)
- MINUS : 차집합(중복제거)
- CROSS JOIN : 곱집합(PRODUCT)
<BR/>
  <SPAN STYLE="COLOR : GREEN">\* ALIAS는 첫 테이블, 정렬은 마지막 테이블 기준</SPAN>
  
### 순수 관계 연산자
> 관계형 DB를 새롭게 구현
- SELECT : WHERE절로 구현
- PROJECT : SELECT절로 구현
- NATURAL JOIN : 다양한 JOIN으로 구현
- DIVIDE : 사용하지 않음
```SQL
{a, x}{a, y}{a, z} divide {x, z} = {a}
```
  
## 3. 계층형 질의와 셀프 조인
### 계층형 질의
> 테이블에 계층형 데이터가 존재하는 경우 데이터를 조회하기 위해 사용
- START WITH : 계층 구조 전개의 시작 위치 지정
- CONNECT BY : 다음에 전개될 자식 데이터 지정
- PRIOR : CONNECT BY절에 사용되며 현재 읽은 컬럼을 지정
```
PRIOR 자식 = 부모 : 계층구조에서 부모 데이터에서 자식 데이터방향으로 전개(순방향). 반대는 역방향
```
- NONCYCLE : 동일한 데이터가 전개되지 않음
- ORDER SIBLINGS BY : 형제 노드간의 정렬 수행
- WHERE : 모든 전개를 수행한 후에 지정된 조건을 만족하는 데이터만 필터링
- LEVEL : 루트 데이터 = 1, 하위 데이터 = 2, 리프데이터까지 1씩 증가
- CONNECT_BY_ISLEAF : 해당 데이터가 리프데이터면 1, 아니면 0
- CONNECT_BY_ISCYCLE : 해당데이터가 조상이면 1, 아니면 0(CYCLE 옵션 사용했을때만)
- SYS_CONNECT_BY_PATH : 루트 데이터부터 현재 전개할 데이터까지의 경로를 표시
- CONNECT_BY_ROOT : 현재 전개할 데이터의 루트 데이터를 표시. 단항 연산자
  
### 셀프 조인
> 한 테이블 내 두 컬럼이 연관 관계가 있을 때 동일 테이블 사이의 조인. FROM절에 동일 테이블이 2번 이상 나타남. 반드시 테이블 별칭을 사용

## 4. 서브 쿼리
> 하나의 SQL문 안에 포함되어 있는 또다른 SQL문
### 서브쿼리 특징
- 서브쿼리를 괄호로 감싸서 사용
- 단일 행 또는 복수 행 비교 연산자와 함께 사용가능
  - 단일 행 비교 연산자는 서브쿼리 결과가 반드시 1건 이하, 복수행 연산자는 상관 없음
- 서브쿼리에서는 ORDER BY 사용불가
- SELECT, FROM, WHERE, HAVING, ORDER BY, INSERT-VALUES, UPDATE-SET절에 사용가능

### 서브쿼리 용어
- 단일 행 비교 연산자 : =, <, > 등
- 다중 행 비교 연산자 : IN, ALL, ANY, SOME 등
- 스칼라 서브쿼리 : 한 행, 한 컬럼만을 반환하는 서브쿼리

### 서브쿼리 분류
- 동작 방식에 따라
  - 비연관 서브쿼리 : 서브쿼리가 메인쿼리 컬럼을 갖지 않음. 메인쿼리에 값 제공 목적
  - 연관 서브쿼리 : 서브쿼리가 메인쿼리 컬럼을 가짐

- 반환 데이터에 따라
  - 단일행 서브쿼리 : 실행결과 1건 이하
  - 다중행 서브쿼리 :  실행결과 여러 건
  - 다중컬럼 서브쿼리 : 실행결과 컬럼 여러 개
  
### 뷰
> 실제로 데이터를 갖지 않는 가상 테이블. 실행시점에 SQL재작성하여 수행됨
- 인라인 뷰 : FROM 절에서 사용되는 서브쿼리, ORDER BY 사용가능
- 사용 장점
  - 독립성 : 테이블 구조가 변경되어도 뷰를 사용하는 응용프로그램은 변경할 필요 없음
  - 편리성 : 복잡한 질의를 뷰로 생성함으로써 관련 질의를 단순하게 작성 가능
  - 보안성 : 직원의 급여정보와 같이 숨기고 싶은 정보가 존재할 때 사용
```SQL
CREATE VIEW V_PLAYER_TEAM AS SELECT...;
DROP VIEW V_PLAYER_TEAM;
```

## 5. 그룹 함수
### ROLL UP
- Subtotal을 생성하기 위해 사용
- Grouping Columns의 수를 N이라고 했을때 N+1 Level의 Subtotal생성
### GROUPING
- 집계 표시면 1, 아니면 0
### CUBE
- 결합 가능한 모든 값에 대하여 다차원 집계를 생성
- ROLL UP에 비해 시스템 부하 가중
### GROUPING SETS
- 인수들에 대한 개별 집계를 구할 수 있음
- 다양한 소계 집합 생성 가능

## 6. 윈도우 함수
> 행간의 관계를 정의하거나 행과 행간을 비교, 연산하는 함수
### 순위 관련 함수
- RANK : (100, 90, 90, 80) $\rightarrow$ (1, 2, 2, 4)
- DENSE_RANK : (100, 90, 90, 80) $\rightarrow$ (1, 2, 2, 3)
- ROW_NUMBER : (100, 90, 90, 80) $\rightarrow$ (1, 2, 3, 4)

### 집계 관련 함수
- SUM : 파티션별 윈도우의 합
- MAX, MIN : 파티션별 윈도우의 최대, 최소값
- AVG : 원하는 조건에 맞는 데이터에 대한 통계(평균) 값
<BR/>
`ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING`
- COUNT : 조건에 맞는 데이터에 대한 통계(개수) 값

### 행 순서 관련 함수
> SQL Server 지원안됨
- FIRST_VALUE : 파티션별 윈도우의 처음 값
- LAST_VALUE : 파티션별 윈도우의 마지막 값
- LAG : 파티션별 윈도우에서 이전 몇 번째 행의 값
- LEAD : 파티션별 윈도우에서 이후 몇 번째 행의 값

### 비율 관련 함수
- RATIO_TO_REPORT : 파티션 내 전체 SUM에 대한 행별 컬럼 값의 백분율을 소수점으로 표시
- PERCENT_RANK : 파티션별 윈도우에서 처음 값을 0, 마지막 값을 1로 하여 행의 순서별 백분율 표시
- CUME_DIST : 현재 행보다 작거나 같은 건수에 대한 누적백분율
- NTILE : 파티션별 전체 건수를 인수값으로 N등분한 결과
  
## 7. DCL
> 유저 생성 및 권한 제어 명령어
### Oracle과 SQL Server의 사용자 아키텍쳐 차이
- Oracle : 유저를 통해 DB에 접속을 하는 형태, ID와 PW방식으로 인스턴스에 접속하고 그에 해당하는 스키마에 오브젝트 생성 등의 권한을 부여받음
- SQL Server : 인스턴스에 접속하기 위해 로그인을 생성하며, 인스턴스 내에 존재하는 다수의 DB와 연결하여 작업하기 위해 유저를 생성한 후 로그인과 유저를 매핑
  
### 시스템 권한
- GRANT : 권한 부여
- REVOKE : 권한 취소
```SQL
GRANT CREATE USER TO SCOTT;
CONN SCOTT/TIGER(ID/P2W)
CREATE USER PJS IDENTIFIED BY KOREA7;
GRANT CREATE SESSION TO PJS;
GRANT CREATE TABLE TO PJS;
REVOKE CREATE TABLE FROM PJS;
```
- 모든 유저는 자신이 생성한 테이블 외의 테이블에 접근하려면 해당 테이블에 대한 오브젝트 권한을 소유자로부터 부여받아야 한다
- ROLE : 유저에게 알맞은 권한들을 한 번에 부여하기 위해 사용
- CASCADE : 하위 오브젝트까지 삭제
```SQL
CREATE ROLE LOGIN_TABLE;
GRANT CREATE TABLE TO LOGIN_TABLE;
DROP USER PJS CASCADE;
```

## 8. 절차형 SQL
> SQL문의 연속적인 실행이나 조건에 따른 분기처리를 이용하여 특정 기능을 수행하는 저장 모듈을 생성
- PROCEDURE, USER, DEFINED FUNCTION, TRIGGER 등
- 저장모듈 : PL/SQL 문장을 DB서버에 저장하여 사용자와 어플리케이션 사이에서 공유할 수 있도록 만든 SQL 컴포넌트 프로그램, 완전한 실행 프로그램

### PL/SQL
> Oracle에서 제공되는 절차형 SQL언어, SQL Server : T-SQL
- 특징
  - BLOCK 구조로 되어 있어 각 기능별 모듈화 가능
  - 변수, 상수 등을 선언하여 SQL 문장 간 값을 교환
  - IF, LOOP 등의 절차형 언어를 사용하여 절차적 프로그램 가능케함
  - DBMS 정의 에러나 사용자 정의 에러를 정의하여 사용가능
  - PL/SQL은 Oracle에 내장되어 있으므로 호환성이 좋다
  - 응용 프로그램의 성능을 향상시킨다
  - BLOCK 단위로 처리하여 통신량을 줄일 수 있다
- 구조
  - DECLARE : BEGIN ~ END 절에서 사용될 변수와 인수에 대한 정의 및 데이터 타입 선언부
  - BEGIN ~ END : 개발자가 처리하고자 하는 SQL문과 여러가지 비교문, 제어문을 이용하여 필요한 로직 처리
  - EXCEPTION : BEGIN ~ END 절에서 SQL문이 실행될때 발생하는 에러를 어떻게 처리할 지 정의하는 예외 처리부
### T-SQL
- 근본적으로 SQL Server를 제어하는 언어
```SQL
CREATE Procedure schema_NAME.Procedure_name
```
### Trigger
- 특정한 테이블에 INSERT, UPDATE, DELETE와 같은 DML문이 수행되었을 때 DB에서 자동으로 동작하도록 작성된 프로그램, 사용자 호출이 아닌 DB 자동 수행

### 프로시저와 트리거의 차이점
- 프로시저 : BEGIN ~ END 절 내에 COMMIT, ROLLBACK과 같은 트랜잭션 종료 명령어 사용가능. EXECUTE 명령어로 실행
- 트리거 : BEGIN ~ END 절 내에 사용 불가. 생성 후 자동 실행