# 1. SQL 기본
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
## DDL
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
## DML
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

## TCL
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

## 연산자
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

## 함수
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

## 집계
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

## 정렬
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

## JOIN
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

# 2. SQL 활용
## 연산자
## 