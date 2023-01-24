# SQL 기본
## 1. SQL 분류
### DML(데이터 조작 언어)
- 데이터조작에 사용되는 언어
- select, insert, update, delete
- 트랜잭션 발생, 필요시 SQL취소 가능
### DDL(데이터정의 언어)
- 데이터베이스 개체를 관리하는 언어
- create, alter, drop
- SQL수행 취소 불가
### DCL(데이터 제어언어)
- 사용자에 권한 부여/제거
- grant, revoke
### TCL(트랜젝션 제어언어)
- 데이터변경내용 DBMS 반영
- commit, rollback
## 2. 데이터 조회
### 기본명령문
```bash
show databases;
# database 조회
use (데이터베이스명);
# database 선택
show tables;
show table status;
# table 조회
desc (테이블명);
# table 구조 조회
```
### SELECT 기본구문
```bash
select (조회 표현식) from (테이블명) where (조건식) group by (칼럼명) having (집계조건식) order by (칼럼명) asc/desc limit (조회 row개수);
```
### SELECT 조회 표현식
```bash
select *
# 테이블 칼럼 전체 조회
select 칼럼명1, 칼럼명2, ...
# 테이블 특정 칼럼 조회
select 칼럼명1 as 별칭명
# 테이블 특정 칼럼 별칭 사용
select distinct 칼럼명
중복없는 데이터 조회
```
### FROM 테이블명
```bash
select * from 테이블명
# 현재 선택되어 있는 데이터베이스 내 테이블 사용
select * from 데이터베이스명.테이블명
# 특정 database 테이블 사용
```
### WHERE 조건식
```bash
select * from 테이블명 where 컬럼명 + (=,<,>,...) + 값;
# 조건식에 맞는 데이터만 조회하기 위한 문장
select * from 테이블명 where 조건 + (and/or) + 조건;
# and:전체만족, or:하나라도
select * from 테이블명 where 컬럼명 between 값1 and 값2;
# 사이값, 데이터가 숫자로 구성되고 연속적인 값인 경우
select * from 테이블명 where 컬럼명 in (값1, 값2...);
# 해당 값이 포함된 데이터 조회
select * from 테이블명 where 컬럼명 like '%검색어%/_검색어_';
# 검색어를 포함한 데이터 조회, % : 여러개 _ : 한개의 문자
select * from 테이블명 where 칼럼명 (=<>) any/all (select 칼럼명 from ...);
# SUBQUERY, any : 한개 이상 조건만족, all : 모든 조건만족
select * from 테이블명 order by 컬럼1, 컬럼2 desc;
# 선택한 컬럼의 데이터 순서대로 정렬, 기본 오름차순 정렬
select * from 테이블명 order by 칼럼1, 칼럼2 desc limit 조회갯수;
# 지정 갯수만큼 데이터 출력, DB 부하 감소
```
## 3. GROUP BY
### GROUP BY
```bash
select 칼럼명, sum(묶을 데이터) from 테이블명 group by 칼럼명;
# 칼럼명 기준으로 묶어서 조회
```
### 집계 함수
|함수명|함수 설명|
|---|---|
|sum()|합계|
|avg()|평균|
|min()|최소값|
|max()|최대값|
|count()|row개수|
|count(distict)|row개수(중복제거)|
|stdev()|표준편차|
|var_samp()|분산|

### HAVING
```bash
select 칼럼명, sum(묶을 데이터) from 테이블명 group by 칼럼명 having sum(묶을 데이터)>100;
# group by 조건식에서 추가 조건 부여
```
### WITH ROLLUP
```bash
select 칼럼명, sum(묶을 데이터) from 테이블명 group by 칼럼명 with rollup;
# 총합 or 중간합계에 활용
```
## 4. 데이터 변경
### INSERT
```bash
insert into 테이블명(컬럼1, 컬럼2...) values (값1, 값2...);
# 테이블에 데이터 추가
alter table 테이블명 auto_increment = 초기값 set @@auto_increment_increment = 증가값
# 자동 증가값 설정, 숫자 형태만 가능, null시 자동 +1
insert into 테이블명(컬럼1, 컬럼2) select문
# 테이블에 select문 실행결과 데이터를 추가
create table 테이블명 select문
# select문 실행결과로 테이블 생성
# 칼럼 속성은 생성되나 key는 미생성
```
### 조건부 데이터 입력
- 데이터 추가 시 중간에 오류가 발생하면 이후 데이터추가 없이 종료됨
```bash
insert ignore into 테이블명(칼럼1, 칼럼2...) values(값1, 값2);
# 데이터 추가시 오류발생해도 이후 문장 수행
insert into 테이블명(칼럼1, 칼럼2...) values(값1, 값2...) on duplicate key update 칼럼1=값1, 칼럼2=값2
# pk 중복되지 않을 경우 insert, 중복될 경우 update
```
### UPDATE
```bash
update 테이블명 set 칼럼1 = 값1, 컬럼2 = 값2 ... where 조건문
# 테이블 데이터를 변경, where 생략 시 모든 행의 데이터 변경
```
### DELETE
```bash
delete from 테이블명 where 조건문;
# 조건문에 맞는 데이터의 row 삭제, where 생략 시 모든 행의 데이터 삭제
```
### 테이블 전체 데이터 삭제
```bash
delete from 테이블명;
# 테이블 내 데이터 삭제, 트랜젝션 로그에 기록하여 데이터 양이 많으면 서버 부하 가능성
drop table 테이블명;
# 테이블째 삭제
truncate 테이블명;
# 데이터만 삭제, 트랜젝션 로그 기록안함
```
## 5. WITH절과 CTE개요
### WITH 구문
```bash
with cte테이블명(칼럼1, 칼럼2...)
as (select 문) select 컬럼1, 컬럼2... from cte테이블명
# 자주 쓰이는 SUBQUERY를 with 절로 따로 빼 사용
```
# SQL 고급
## DATA TYPE/형 변환
### DATA TYPE : 숫자형
- INT : 정수 : 약 -21억 ~ 약 21억
- BIG INT : 정수 : 약-900경 ~약900경
- DECIMAL(m,[d]) : 실수(m전체자릿수, d소수점이하 자릿수)
### DATA TYPE : 문자형
- char(n) : 고정길이 문자 : 1~255
- varchar(n) : 가변길이 문자 : 1~65535
- longtext : 대용량 text데이터 : ~4G
- longblob : 대용량 blob데이터 : ~4G
### DATA TYPE : 날짜/시간
- DATE : YYYY-MM-DD
- TIME : HH::MM::SS
- DATETIME : YYYY:MM:DD HH:MM:SS
### 변수의 사용
```bash
set @ 변수명 = 값;
select @ 변수명;
# SQL에서 변수 선언 및 활용
declare 변수명
set 변수명 = 값;
select 변수명;
# 스토어드 프로그램에서의 변수선언
```
### DATA 형 변환
```bash
cast(expression as 데이터형식[(길이)])
convert(expression, 데이터형식[(길이)])
# 함수를 이용한 형 변환
```
```bash
select'100'+'200';
# 300
select concat (100,'200')
# 100200, concat 영향
select 1> '2dolors';
# 0, 연산자 영향으로 2dolor 2로 반환
```
## 내장/윈도우 함수
### 제어흐름 함수
```bash
if (수식, 값1, 값2)
# 수식이 참이면 값1, 거짓이면 값2
ifnull(수식1, 수식2)
# 수식1이 null이 아니면 수식1, null이면 수식2
nullif(수식1, 수식2)
# 수식1=수식2 이면 null, 아니면 수식1
case 수식 when 수식1 then 값1 when 수식2 then 값2 else 값3 end
# 수식=수식1;값1, 수식=수식2;값2, 모두 같지 않으면 값3
```
### 문자열 함수
```bash
ASCII(아스키코드)
# 아스키코드에 해당하는 숫자값
CHR(숫자)
# 숫자에 해당하는 아스키코드값
BIT_LENGTH(문자열)
# 문자열의 BIT크기
CHAR_LENGTH(문자열)
# 문자열의 문자 개수
LENGTH(문자열)
# 문자열의 BYTES크기
CONCAT(문자열1, 문자열2)
# 문자열과 문자열을 연결
INSTR(문자열, 검색어)
# 문자열 내 검색어를 찾아 시작위치 반환
FORMAT(숫자, 소수점자리수)
# 소수점자리수만큼 표현
INSERT(문자열, 위치, 길이, 추가문자열)
# 위치부터 길이만큼 삭제 후 추가문자열 삽입
LEFT/RIGHT(문자열, 길이)
# 왼쪽/오른쪽 문자열에서 길이만큼
UPPER/LOWER(문자열, 길이)
# 소/대문자를 대/소문자로 변경
LPAD/RPAD(문자열, 길이, 채울문자열)
# 문자열 좌/우측에 길이만큼 추가
(L/R)TRIM()
# 앞뒤(왼/오른쪽) 공백제거
REPEAT(문자열, 횟수)
# 문자열을 횟수만큼 반복
REPLACE(문자열, 기존문자열, 바꿀 문자열)
# 문자열에서 기존문자열을 바꿀문자열로 변환
REVERSE(문자열)
# 문자열의 순서를 거꾸로 변환
SPACE(길이)
# 길이만큼 공백 반환
SUBSTRING(문자열, 시작위치, 길이)
# 시작위치부터 길이만큼 문자열 반환, 길이 생략시 문자열 끝까지
SUBSTRING_INDEX(문자열, 구분자, 횟수)
# 구분자가 횟수번째 나오면 이후 버림, 횟수 음수이면 ~~!!!!!!14p
```
### 수학 함수
```bash
ABS(숫자)
# 숫자의 절대값
SIN(), COS(), TAN(), ASIN(), ACOS(), ATAN()
# 삼각함수 결과값
CEIL(숫자)
# 숫자의 올림값
FLOOR(숫자)
# 숫자의 내림값
ROUND(숫자)
# 숫자의 반올림값
MOD(숫자1, 숫자2)
# 숫자1을 숫자2로 나눈 나머지
POW(숫자1, 숫자2)
# 숫자1의 숫자2제곱값
SQRT(숫자)
# 숫자의 제곱근
RAND()
# 0이상 1미만의 실수
TRUNCATE(숫자, 소수점자리수)
# 숫자를 소수점자리수까지 구하고 나머지 버림
```
### 날짜 및 시간함수
```bash
ADDDATE/SUBDATE(날짜, 차이)
# 날짜를 기준으로 차이를 더함/뺌
ADDTIME/SUBTIME(날짜/시간, 시간)
# 날짜/시간 기준으로 시간을 더함/뺌
CURDATE()
# 현재 년-월-일
CURTIME()
# 현재 시:분:초
NOW()/SYSDATE()
# 현재 연-월-일 시:분:초
YEAR(날짜)/MONTH(날짜)/DAY(날짜)
# 연도/월/일 반환
HOUR(시간)/MINUTE(시간)/SECOND(시간)/MICROSECOND(시간)
# 시/분/초/밀리초 반환
DATE(날짜/시간)
# 연-월-일 반환
TIME(날짜/시간)
# 시:분:초 반환
DATEDIFF(날짜1, 날짜2)
# 두 날짜의 차이값
TIMEDIFF(시간1, 시간2)
#  두 시간의 차이값
DAYOFWEEK(날짜)
# 요일 반환(1:일 ~ 7:토)
MONTHNAME(날짜)
# 월 이름 반환
DAYOFYEAR(날짜)
# 1년 중 몇번째 날짜인지 반환
LAST_DAY(날짜)
# 날짜 월의 마지막 날짜
MAKEDATE(연도, 정수)
# 연도에서 정수만큼 지난 날짜
MAKETIME(시, 분, 초)
# 시:분:초 형태로 반환
PERIOD_ADD(연월, 개월수)
# 연월에서 개월수만큼 지난 연월
PERIOD_DIFF(연월1, 연월2)
# 연월1-연월2 개월 수
QUARTER(날짜)
# 날짜가 몇 분기인지
TIME_TO_SEC(시간)
# 시간을 초단위로 구함
```
### 시스템 정보 함수
```bash
USER()
# 현재 사용자 확인
DATABASE()
# 현재 사용중인 데이터베이스 확인
FOUND_ROWS()
# 바로 앞에 실행한 SELECT문에서 조회된 행의 개수 반환
ROW_COUNT()
# 바로 앞에 실행한 INSERT, UPDATE, DELETE문에서 입력, 수정, 삭제한 행의 개수 반환
VERSION()
# 현재 MariaDB버전 확인
SLEEP(초)
# 쿼리의 실행을 초만큼 멈춤
```
### Text 데이터 대용량 입력하기
저장하고자 하는 문자가 16M 이상 대용량일 경우

`C:\Program Files\MariaDB 10.6\data\my.ini`

.my.ini 설정 파일에 max_allowed_packet 설정 값 추가

`[mysqld]max_allowed_packet=1000M`

마리아DB 재시작

`net stop/start mariadb`

파일로 저장한 대용량 데이터를 테이블 데이터로 저장

`(cmd) LOAD DATA LOCAL INFILE 파일경로/파일명 INTO 테이블명;`

### BLOB 파일 입력하기
```bash
insert into 테이블명(컬럼, BLOB컬럼 ...) values (값1, load_file(파일경로/파일명) ...);
# 이미지/동영상/PDF 문서 등 TEXT외 파일 BLOB 형태로 저장
select BLOB컬럼 into dumpfile 파일경로/파일명 from 테이블명 where 조건;
# BLOB 데이터 파일로 저장
```
### 윈도우함수
- 행과 행사이 관계를 쉽게 정의하기 위해 제공되는 함수
- 복잡한 SQL 쉽게 활용가능
- OVER절 포함 함수
- 집계함수 : AVG(), COUNT(), MAX(), MIN(), STDDEV(), SUM(), VARIANCE()
- 비집계함수
  - 순위함수 : ROW_NUMBER(), DENSE_RANK(), RANK(), NTILE()
  - 분석함수 : LEAD(), LAG(), FIRST_VALUE, CUME_DIST()
### 윈도우 순위 함수
#### 기본양식
```bash
순위함수() + over([partition by 컬럼] order by 컬럼 [desc]);
# 단순한 구문으로 순위를 표현
# partition by 구문으로 칼럼별 순위표현
```
#### 순위 표현
```bash
row_number()
# 순위를 첫행 1부터 1씩 증가
dense_rank()
# 동일한 값을 동순위로 처리
rank()
# 동순위가 있을 경우 다음행은 동순위 수만큼 +
ntile()
# 몇 개의 그룹으로 분할
```
```bash
# 샘플코드
select row_number() over(order by height desc) "키큰순위", name/addr/height from usertbl;
```
### 윈도우 분석 함수
```bash
lead()
# 다음행 데이터 값
lag()
# 이전행 데이터 값
first_value()
# 가장 큰 값
cume_dist()
# 누적합계의 백분율
```
```bash
# 샘플코드
select lead(height,1) over(order by height desc) as "n_height", height/name/addr from usertbl;
```
### 피벗구현
- 여러 행에 걸쳐 기록된 데이터를 열로 변환하고 필요시 집계 하는 것
- ex) 쇼핑몰 사이트에 기록된 여러 구매 정보를 구매자별로 변환
### JSON
- key와 value로 구성됨
- {"key1":"value1","key2":"value2"}
```bash
select json_object('키이름1','컬럼1','키이름2','컬럼2') from 테이블명 where 조건절;
# 테이블 데이터를 JSON형식으로 변환
```
## JOIN
### 개념
- 두개 이상의 테이블을 결합하는 것
### INNER JOIN
- 가장 많이 활용되는 조인
- 중첩되는 키값이 있을때만
```bash
# 기본구문
select 칼럼1, 칼럼2... from 테이블a inner join 테이블b on 테이블a.칼럼=테이블b.칼럼 where 조건절;
```
### OUTER JOIN
- 조인의 조건에 만족되지 않은 행까지 포함
- LEFT/RIGHT JOIN : 왼/오른쪽 테이블 데이터 모두 출력
```bash
# 기본구문
select 칼럼1, 칼럼2... from 테이블a left/right outer join 테이블b on 테이블a.칼럼=테이블b.칼럼 where 조건절;
```
### CROSS JOIN
- 한 테이블의 모든 행과 다른 테이블의 모든 행을 조인
- 두 테이블 개수를 곱한 개수
```bash
# 기본구문
select * from 테이블a cross join 테이블b where 조건절;
```
### SELF JOIN
- 자기자신과 자기자신의 테이블 조인
- 대표 예 : 조직도 테이블
```bash
# 샘플코드
select a.empid'사원id', a.empname'사원명',a.emptel'사원전화번호',b.empid'부서장id',b.empname'부서장성명',b.emptel'부서장 전화번호' from emptbl a inner join emeptbl b on a.managerid=b.empid where a.empid = '204'
```
### UNION/UNION ALL
- 두 쿼리의 결과를 행으로 결합
- union : 중복 row 제거
- union all :  모든 row 결합
### IN/NOT IN
- 데이터 포함/포함하지 않은 행 조회
### INLINE VIEW
```bash
# 기본구문
select a.컬럼1, a.컬럼2... from (select 컬럼1, 컬럼2... from 테이블명 where 조건절) a;
```
```bash
# WITH절 문장으로 변경
with a(컬럼1, 컬럼2) as (select 컬럼1, 컬럼2... from 테이블명 where 조건절) select a.컬럼1, a.컬럼2 from a;
```
## SQL 프로그래밍
### 스토어드 프로시저 프로그램
- 분기, 제어, 반복문 등 기본 프로그래밍 기능 제공
```bash
# 기본 구문
delimiter $$ create procedure 스토어드 프로시저명 (IN/OUT 파라미터) begin + (SQL 프로그램 코딩) + end $$ delimiter;
call 스토어드 프로시저명();
```
### 분기문 (IF문)'
```bash
if 부울표현식1 then SQL문장1;
esleif 부울표현식2 then SQL문장2;
else SQL문장3 end if;
# 부울표현식1이 참이면 SQL문장1 실행, 거짓이고 부울표현식2가 참이면 SQL문장2 실행, 둘다 거짓이면 SQL문장3
```
### 분기문(CASE 문)
```bash
case when 부울표현식1 then SQL문장1;
when 부울표현식2 then SQL문장2;
else SQL문장3; end case;
```
- 데이터 조건별로 처리할 경우 많이 사용
### 반복문 (WHILE 문)
```bash
while 부울표현식 do SQL문장들;
end while;
```
- iterate : 파이썬 continue
- leave : 파이썬 break
### 오류 처리
```bash
# 기본 구문
declare 액션 handler for 오류조건 처리문장;
```
#### 액션 : 오류 발생시 프로그램 진행여부 결정
- continue : declare문의 처리문장 수행
- exit : 프로그램 종료
#### 오류조건 : 어떤 종류의 오류를 처리할 것인지 정의
- sqlexception : 대부분의 발생오류
- sqlwarning : 경고 메시지
- not found : 데이터가 없음
#### 처리문장
```bash
# 처리 문장이 여러개일 경우 샘플 코드
declare continue handler for sqlexception
begin
show error;
rollback;
end;
```
- show error : 오류 코드 및 메시지 출력
- roll back : 트랜젝션 취소
### 동적 SQL
- 쿼리문장을 변수에 담아 실행
```bash
prepare 변수 from 'SQL쿼리문';
# 쿼리문을 변수에 담기
execute 변수;
# 변수에 담긴 쿼리문 실행
deallocate prepare 변수;
# 변수에 담긴 쿼리문 해제
```
# 테이블과 뷰
## TABLE 생성
```bash
# 테이블 생성 및 키 설정
create table 테이블명(컬럼명 타입(길이) [not null] [primary key],
칼럼명 타입(길이) [not null], [foreign key references 테이블명(칼럼명)]);
```
### 데이터 생성
```bash
insert into 테이블명 values (칼럼1 데이터, 칼럼2 데이터...);
```

## 제약 조건
### 제약조건 설정
- 데이터무결성 확보를 위함
- primary key : 기본키
- foreign kwy : 외래 키
- unique kwy : 유일 키, null 가능
- check : 조건 맞지 않으면 입력 불가
- default 정의 : 자동입력 기본 값
- null 허용 : null 허용여부
### PRIMARY KEY
- 테이블 내 데이터 구분 식별자
- 확장성, 대표성 고려
- 중복 불가, null 불가
- 칼럼 2개 이상 조합하여 PK설정 가능
```bash
alter table 테이블명 add primary key(컬럼1, 컬럼2...);
# 키 생성
alter table 테이블명 drop primary key;
# 키 삭제
show index form 테이블명
# 키 확인
```
### FOREIGN KEY
- 외래키 테이블 입력 시 기준 테이블에 데이터 존재해야 함
- 기준테이블에서 PK칼럼, unique 제약 조건 설정된 칼럼 참조
```bash
# 테이블 생성 시 외래 키 지정
create table 테이블명(컬럼설정, constraint 외래키명 foreign key(컬럼명) references 기준테이블명(기준테이블 컬럼명);)
# 테이블 생성 후 외래 키 지정
alter table 테이블명 add constraint 외래키명 foreign key(컬럼명) reference 기준테이블명(기준테이블 컬럼명);
# 외래 키 삭제
alter table 테이블명 drop constraint 외래키명;
# 기준테이블 데이터 수정/삭제 시 외래 키 테이블 동시 적용
alter table 테이블명 add constraint 외래키명 foreign key(컬럼명) references 기준테이블명(기준테이블 컬럼명) on update cascade on delete cascade;
```
### UNIQUE KEY
- 중복되지 않는 유일값만 허용
- NULL값 허용(PK와의 차이점)
```bash
# 테이블 설정 후 유니크키 설정
alter table 테이블명 add constraint 유니크키명 unique key(컬럼명);
```
### CHECK
- 데이터 입력 조건을 설정하여 조건에 부합하는 데이터만 저장가능
```bash
# 테이블 생성 후 체크키 설정
alter table 테이블명 add constraint 체크명 check (조건식);
```
### DEFAULT
- 값 없이 입력 시 자동으로 입력되는 기본 값
```bash
# 테이블 생성 후 디폴트값 설정
alter table 테이블명 alter column 컬럼명 set default 컬럼값;
```
### NULL 허용
```bash
# 테이블 생성 후 NULL 설정
alter table 테이블명 modify column 컬럼명 컬럼타입(길이) NULL/NOT NULL;
```
## TABLE 수정/삭제
### TABLE 압축
- 대용량 테이블 공간 절약
- 데이터 insert 시간 증가
```bash
# 생성 구문
create table 테이블명(컬럼설정) row_format=compressed;
```
### 임시 TABLE
- 필요에 의해 잠깐 사용
- 생성한 클라이언트에서만 사용가능
- 세션 종료 시 자동 삭제
```bash
# 생성 구문
create temporary table 테이블명(컬럼 설정);
```
### TABLE 삭제
- 외래키 제약 조건의 기준테이블 삭제 불가(외래 키 테이블 삭제 후 삭제 가능)
```bash
# 기본 구문
drop table 테이블명;
# 외래 키 테이블 검색
select * from information_schema.check_constraints where constraints_schema = 데이터베이스명 and table_name = 테이블명;
```
### TABLE 수정
```bash
# 기본 구문
alter table 테이블명 [add/drop/modify/change] 컬럼명
```
- add : 컬럼 추가
- drop : 컬럼 삭제
- modify : 컬럼 속성 변경
- change : 컬럼명 변경
## VIEW
### VIEW
- select를 통해 조회한 결과를 테이블 형태로 볼 수 있도록 생성
```bash
# 뷰 생성
create view 뷰명 as select 조회문장;
# 뷰 삭제
drop view 뷰명;
```
- 보통 읽기 전용으로 활용
- 뷰를 통해 원본테이블 수정/삭제 가능
- 사용자별 데이터 및 칼럼에 대한 접근권한 제어가능
- 복잡한 쿼리 단순화 가능
# 인덱스
## INDEX 개요
### INDEX 개념
- 인덱스 생성 컬럼의 데이터를 정렬하여 별도 메모리 공간에 물리적 주소와 함께 저장해 검색속도 상향
- 데이터의 중복도가 높지 않아야 함
- 테이블 공간대비 약 10-20%공간 차지로 테이블 데이터가 많은 경우 최초 INDEX 생성시 많은 시간 소요
## INDEX 종류/특징
### 클러스터형 인덱스
- PRIMARY KEY
- 테이블당 한개 생성
- 물리적 데이터 정렬
- PK가 없을 경우 UNIQUE NOT NULL 칼럼
- 인덱스 생성시 데이터 전체 재정렬
- 보조 인덱스보다 검색속도는 빠르나 데이터 입력/수정/삭제는 느림
### 보조 인덱스
- 테이블당 여러개 생성 가능
- 데이터 페이지의 위치 정보를 인덱스로 구성
- 여러 컬럼을 조합하여 생성 가능
- 인덱스 생성시 데이터 정렬 불필요
- 여러개 생성 가능하나 많으면 성능저하
## INDEX 생성/삭제
### 인덱스 생성
```bash
# 생성 구문
create [unique] index 인덱스명 on 테이블명 (인덱스컬럼1, 인덱스컬럼2);
```
- unique : 고유 인덱스 생성시 사용
- create index로 생성시 보조인덱스
### 인덱스 삭제
```bash
# 삭제 구문
drop index 인덱스명 on 테이블명;
# PK제거 시 클러스터 인덱스 삭제됨
alter table 테이블명 drop primary key;
```
- 보조 인덱스 부터 삭제
- 활용도가 적은 인덱스 삭제
## INDEX 성능
### 인덱스 성능
```bash
# 인덱스 생성 후 테이블 통계정보 생성
analyze table 테이블명;
# 인덱스 사용현황(인덱스명, 검색 건수 등) 확인
explain select 문장;
# 강제 인덱스 제외
explain select 컬럼 from 테이블명 ignore index(인덱스명) where 조건문;
# 칼럼 가공시 인덱스 미적용
explain select 칼럼 from 테이블명 where left (칼럼, 2) = 값;
# 강제 인덱스 적용
explain select 컬럼 from 테이블명 use index(인덱스명) where 조건문;
```
### 인덱스 설정 기준
- where 조건에 자주 사용되는 컬럼
- join이 자주 사용되는 컬럼
- 범위로 사용하거나 집계함수 사용되는 컬럼
- order by 절에 자주 사용되는 컬럼

### 인덱스 설정시 고려사항
- insert/update/delete 빈도 수가 많은 경우
- insert가 빈번한 테이블은 PK보다 UNIQUE kEY 설정 고려
- 생성한 인덱스 중 사용빈도가 적은 인덱스는 제거
# 스토어드 프로그램
## 스토어드 프로시저
### 프로시저 개요
- DB에서 제공하는 프로그램 언어
- 자주 사용되는 일반적인 쿼리를 모듈화하여 필요할때마다 호출
- 긴 SQL코드 대신 스토어드 프로시저 이름과 매개 변수 전송으로 결과값 확인
- Python, Java등에서 스토어드 프로시저 이름만 호출하여 간단히 수행
```bash
# 프로시저 생성
delimiter $$
create procedure 프로시저명 (IN/OUT 파라미터)
begin
    SQL프로그램 코딩 영역:
end $$
delimiter ;
call 프로시저명();
# 프로시저 삭제
drop procedure 프로시저명;
```
### 매개 변수
```bash
# 입력 매개변수 지정
in 입력_매개변수_이름 데이터_형식
# 매개변수 프로시저 실행
call 프로시저_이름(전달_값);
# 출력 매개 변수 지정
out 출력_매개변수_이름 데이터_형식
# 출력 매개 변수가 있는 프로시저 실행
call 프로시저명(@변수명);
select @변수명;
```
## 스토어드 함수
### 개요
- 사용자가 직접 만들어 사용하는 함수
```bash
delimiter $$
create function 함수명(파라미터)
returns 반환형식
begin
    SQL프로그램코딩:
    return 반환값;
end $$
delimiter;
select 함수명() [into @변수명];
```
### 프로시저와 차이점
- 입력 파라미터만 가능
- 하나의 값만 return문으로 제공 가능
- 집합 결과를 사용하는 select 사용불가
- select 문장으로 호출(프로시저는 call) 
## 커서
### 개요
- 테이블의 여러행을 조회하여 행별로 데이터를 처리
```bash
# 기본구문
declare 변수명 boolean default false;
declare 커서명 cursor for select 문장;
declare continue handler
    for not found set 체크변수 = true;
open 커서명;
루프명:loop
    fetch 커서명 into 변수명;
    if 체크변수 then leave 루프명;
    end if;
end loop 루프명;
close 커서명;
```
## 트리거
### 개요
- 테이블 CUD 발생 시 트리거 설정되어 있으면 트리거 자동 실행
- 제약조건과 함께 데이터 무결성을 위해 DBMS에서 제공하는 기능
- 스토어드 프로시저와 비슷한 문법으로 내용 작성
### 트리거 생성
```bash
delimiter $$
create trigger 트리거명
    {ager/before}{insert/update/delete}
    on 테이블명
    for each row
begin
    SQL 프로그램 코딩:
end $$
delimiter;
```
- after : 테이블 데이터에 변경이 가해진 후 작동
- before : 변경이 가해지기 전에 작동
- insert/update/delete : 트리거 이벤트 발생 기준
### 트리거 삭제
```bash
drop trigger 트리거명;
```
### 트리거 생성 임시테이블
|CUD|NEW TABLE|TARGET TABLE|OLD TABLE|COMMENT|
|--|--|--|--|--|
|INSERT(N)|NEW|NEW||NEW|
|UPDATE(O,N)|NEW|~~OLD~~,NEW|OLD|NEW,OLD|
|DELETE(O)||~~OLD~~|OLD|OLD|
# 전체 텍스트 검색과 파티션
## 전체 텍스트 검색 개요
- 긴 문장으로 구성된 텍스트 내용을 빠르게 검색
- 저장된 텍스트에서 키워드를 추출하여 인덱스로 설정하는 방식
- 첫 글자 뿐만 아니라 중간 단어와 문장으로도 인덱스 생성가능
- char, varchar, text타입에만 인덱스 생성 가능
```bash
select * from 테이블명 where 검색컬럼명 like '단어%';
select * from 테이블명 where 검색컬럼명 like '%단어%';
```
## 전체 텍스트 인덱스 생성, 삭제
### 전체 텍스트 인덱스 생성
```bash
# 테이블 생성 시 인덱스 생성
create table 테이블명(
    컬럼 설정, full text 인덱스명(컬럼명));
# alter 명령어로 테이블 생성 후 인덱스 생성
alter table 테이블명
    add fulltext 인덱스명(컬럼명));
# create 명령어로 테이블 생성 후 인덱스 생성
create fulltext index 인덱스명
     on 테이블명(컬럼명);
```
### 전체 텍스트 인덱스 삭제
```bash
# alter 명령어 사용
alter table 테이블명
    drop index 인덱스명;
# drop 명령어 사용
drop index 인덱스명
    on 테이블명;
```
### 중지단어
- 긴 문장은 효율성을 위해 검색에서 무시할 만한 단어들은 제외
## 전체 텍스트 검색 쿼리
### 검색 쿼리
```bash
# 테이블 생성시 인덱스 생성
match(컬럼1, 컬럼2...)
against (검색표현식
    in natural language mode
    |in boolean mode
    |with query expansion);
```
- in natural language mode
  - 옵션 미설정 시 기본 제공모드
  - 정확히 일치하는 단어만 검색
  - 두 단어중 하나 포함여부 검색 : against('단어1 단어2')
- in boolean mode
  - 검색어를 포함한 단어나 문장 검색 가능
  - + : 반드시 포함, - : 제외, * : 부분검색
- with query expansion
  - 검색 완료 후 관련있는 내용 추가 검색 결과 제공
### IN NATURAL LANGUAGE MODE
```bash
# article 중 영화 단어가 있는 데이터 조회
select * from newspaper where match(article) against('영화');
# article 중 영화 또는 배우 단어가 있는 데이터 조회
select * from newspaper where match(article) against('영화배우');
```
### IN BOOLEAN MODE
```bash
# article 중 영화 단어가 있는 데이터 조회
select * from newspaper where match(article) against('영화*' in boolean mode);
# article 중 영화 또는 배우 단어가 있는 데이터 조회
select * from newspaper where match(article) against('영화배우', in boolean mode);
# 영화가 정확히 들어간 결과 중 공포가 반드시 포함된 데이터 조회
select * from newspaper where match(article) against('영화+공포' in boolean mode);
# 영화 단어가 정확히 들어간 결과 중 남자가 포함되지 않은 데이터 조회
select * from newspaper where match(article) against('영화-남자' in boolean mode);
```
### WITH QUERY EXPANSION
- 검색 후 데이터와 관련있는 내용 추가 검색 결과 포함해 제공
### 인덱스 생성 규칙 설정
```bash
# 인덱스 생성 단어 최소길이 확인
show variables like 'innodb_ft_min_token_size';
# 2글자까지 인덱스 생성되도록 변경 (my.ini 파일 [mysqld] 아래쪽에 세팅값 추가)
innodb_ft_min_token_size=2
# MariaDB restart
net stop mariadb
net start mariadb
```
### 중지(제외) 단어 설정
```bash
# 중지(제외) 단어 설정 테이블 생성
create table user_stopword(value varchar(30));
# 중지(제외) 단어 insert
insert into user_stopword values('단어1', '단어2'...);
# innodb GLOBAL 변수 설정/확인
set global innodb_ft_server_stopword_table = 'fulltextdb/user_stopword';
show global variables like 'innodb_ft_server_stopword_table';
# 전체 텍스트 인덱스 생성/확인
create fulltext index idx_description on fulltextTBL(description);
select * from information_schema.innodb_ft_index_table;
```
## 파티션 개요와 실습
### 파티션 생성
```bash
# range partition 생성
create table 테이블명(컬럼설정)
partition by range(컬럼명)
    (partition 파티션명 values less than (숫자),
    partition 파티션명 values less than (숫자),
    partition 파티션명 values less than maxvalue);
```
- 파티션 테이블에 PK 설정 불가
- 파티션 키로 설정한 컬럼을 포함하여 PK설정은 가능
- 숫자형 데이터만 파티션 키 설정 가능
```bash
# list partition 생성
create table 테이블명(컬럼설정)
partition by list column (컬럼명)
    partition 파티션명 values in (값1, 값2...),
    partition 파티션명 values in (값3, 값4...);
```
### 파티션 확인
```bash
# 파티션 정보 조회
select table_schema, table_name, partition_name, partition_ordinal_position, table_rows
from information_schema.partitions
where table_name = 테이블명;
# 테이블 검색 시 사용된 파티션 테이블 확인
explain paritition select 문장;
```
### 파티션 관리
```bash
# 파티션 분리
alter table 테이블명
    reorganize partition 분리할_파티션명 into
    (partition 분리_파티션명1 values less than (숫자),
    partition 분리_파티션명2 values less than maxvalue);
# 파티션 병합
alter table 테이블명
    reorganize partition 합할_파티션명1, 합할_파티션명2 into
    (partition 파티션명 values less than(숫자));
# 파티션 삭제
alter table 테이블명
    drop partition 파티션명;
# 파티션 재구성 (분리/합한뒤 수행)
optimizez table 테이블명
```
# 파이썬과 MariaDB 연동
## 환경설정
- cmd : pymysql 설치

## 입력 프로그램
### 파이썬 코딩 순서
- MariaDB 연결 : pymysql.connect(연결옵션)
- 커서 생성 : 연결자.cursor()
- 데이터 입력 : 커서명.execute("insert 문장")
- 입력 데이터 저장 : 연결자.commit()
- MariaDB 연결종료 : 연결자.close()
### 파이썬 예제
```bash
import pymysql

conn = pymysql.connect(host='localhost', port=3306, user='root', password='1234', db='sqldb')
cur = conn.cursor()
sql = "insert into usertbl(userid, name, brithyear, addr) values('KIM', '김씨', 1991, '서울');"
cur.execute(sql)
conn.commit()
conn.close()
```
## 조회 프로그램
### 파이썬 코딩 순서
- MariaDB 연결 : 연결자 = pymysql.connect(연결옵션)
- 커서 생성 : 커서명 = 연결자.cursor()
- 데이터조회 : 커서명.execute("SELECT문")
- 조회 데이터 출력 : 커서명.fetchone()
- MariaDB 연결종료 : 연결자.close()
### 파이썬 예제
```bash
import pymysql

conn = pymysql.connect(host='localhost', port=3306, user='root', passwd='1234', db='sqldb')
cur = conn.cursor()
sql = "SELECT userid, name FROM userTBL;"
cur.execute(sql)
while True:
	row = cur.fetchone()
	if row == None: break;
print(row[0], row[1])
conn.close()
```