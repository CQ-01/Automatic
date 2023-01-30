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