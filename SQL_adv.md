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

